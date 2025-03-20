---
title: "So many tokens, so little time: Introducing a faster, more flexible byte-pair tokenizer"
date: 2025-01-03
categories: 
  - "general"
tags: 
  - "development"
  - "git"
  - "github"
  - "gitlab"
  - "software"
---

We released a new open source byte-pair tokenizer that is faster and more flexible than popular alternatives.

The post So many tokens, so little time: Introducing a faster, more flexible byte-pair tokenizer appeared first on The GitHub Blog.

Large language models (LLMs), such as those used by GitHub Copilot, do not operate directly on bytes but on _tokens_, which can complicate scaling. In this post, we explain how we solved that challenge at GitHub to support the growing number of Copilot users and features since the first launching Copilot two years ago.

_Tokenization_ is the process of turning bytes into tokens. The byte-pair encoding (BPE) algorithm is such a tokenizer, used (for example) by the OpenAI models we use at GitHub. The fastest of these algorithms not only have at least an _O(n log(n))_ complexity, they are also not incremental, and therefore badly suited for our use cases, which go beyond encoding an upfront known input. This limitation resulted in a number of scaling challenges that led us to create a novel algorithm to address them. Our algorithm not only scales linearly, but also easily out-performs popular libraries for all inputs.

Read on to find out more about why and how we created the open source bpe algorithm that substantially improves state of the art BPE implementations in order to address our broader set of use cases.

## The importance of fast, flexible tokenization

Retrieval augmented generation (RAG) is essential for GitHub Copilot’s capabilities. RAG is used to improve model output by augmenting the user’s prompt with relevant snippets of text or code. A typical RAG approach works as follows:

- Index repository content into an embeddings database to allow semantic search.
- Given a user’s prompt, search the embeddings database for relevant snippets and include those snippets in the prompt for the LLM.

Tokenization is important for both of these steps. Most code files exceed the number of tokens that can be encoded into a single embedding, so we need to split the files into chunks that are within the token limit. When building a prompt there are also limits on the number of tokens that can be used. The amount of tokens can also impact response time and cost. Therefore, it is common to have some kind of budgeting strategy, which requires being able to track the number of tokens that components of the prompt contribute. In both of these cases, we are dynamically constructing a text and constantly need the updated token count during that process to decide how to proceed. However, most tokenizers provide only a single operation: encode a complete input text into tokens.

When scaling to millions of repositories and billions of embeddings, the efficiency of token calculation really starts to matter. Additionally, we need to consider the worst-case performance of tokenization for the stability of our production systems. A system that processes untrusted user input in the form of billions of source code files cannot allow data that, intentionally or not, causes pathological run times and threatens availability. (See this discussion of potential denial-of-service issues in the context of OpenAI’s tiktoken tokenizer.)

Some of the features that can help address these needs are:

- Keep track of the token count for a chunk while it is being built up.
- Count tokens for slices of an original text or abort counting when the text exceeds a given limit.
- Split text within a certain amount of tokens at a proper UTF-8 character boundary.

Implementing these operations using current tokenization algorithms would result in at least quadratic runtime, when we would like the runtime to be linear.

## bpe: A fast tokenizer

We were able to make substantial improvements to the state of the art BPE implementations in order to address the broader set of use cases that we have. Not only were we able to support more features, but we do so with much better performance and scalability than the existing libraries provide.

Our implementation is open source with an MIT license can be found at https://github.com/github/rust-gems. The Rust crates are also published to crates.io as bpe and bpe-openai. The former contains the BPE implementation itself. The latter exposes convenience tokenizers (including pre-tokenization) for recent OpenAI token models.

Read on for benchmark results and an introduction to the algorithm itself.

## Performance comparison

We compare performance with two benchmarks. Both compare tokenization on randomly generated inputs of different sizes.

- The first uses any inputs, most of which will be split into smaller pieces during pre-tokenization. Pre-tokenization splits the input text into pieces (for example, using regular expressions). BPE is applied to those pieces instead of the complete input text. Since these pieces are typically small, this can significantly impact overall performance and hide the performance characteristics of the underlying BPE implementation. This benchmark allows comparing the performance that can be expected in practice.
- The second uses pathological inputs that won’t be split by pre-tokenization. This benchmark allows comparing the performance of the underlying BPE implementations and reflects worst-case performance.

We used OpenAI’s o200k\_base token model and compared our implementation with tiktoken-rs, a wrapper for OpenAI’s tiktoken library, and Huggingface’s tokenizers. All benchmarks were run single-threaded on an Apple M1 MacBook Pro.

Here are the results, showing single-threaded throughput in MiB/s:

![Line graph displaying results for the benchmark that includes pre-tokenization. Our tokenizer outperforms tiktoken by almost 4x and Huggingface by about 10x.](https://github.blog/wp-content/uploads/2024/12/throughput-with-pre-tokenization.png?w=300&resize=300%2C300)

![Line graph displaying the worst case complexity difference between our linear, Huggingface’s heap-based, and tiktoken’s quadratic implementation.](https://github.blog/wp-content/uploads/2024/12/throughput-without-pre-tokenization.png?w=300&resize=300%2C300)

The first figure shows the results for the benchmark that includes pre-tokenization. We see that our tokenizer outperforms tiktoken by almost 4x and Huggingface by about 10x. (These numbers are in line with tiktoken’s reported performance results. Note that our single-threaded performance is only matched when using eight threads.) The second figure shows the worst case complexity difference between our linear, Huggingface’s heap-based, and tiktoken’s quadratic implementation.

The rest of this post details how we achieved this result. We explain the basic principle of byte-pair encoding, the insight that allows the faster algorithm, and a high-level description of the algorithm itself.

## Byte-pair encoding

BPE is a technique to encode text as a sequence of tokens from a token dictionary. The token dictionary is just an ordered list of tokens. Each token is either a single byte, or the concatenation of a pair of previously defined tokens. A string is encoded by replacing bytes with single-byte tokens and token pairs by concatenated tokens, in dictionary order.

Let’s see how the string `abacbb` is tokenized using the following dictionary:

```plaintext
a b c ac bb ab acbb
```

Initially, the string is tokenized into the single-byte tokens. Next, all occurrences (left to right) of the token pair `a c` are replaced by the token `ac`. This procedure is repeated until no more replacements are possible. For our input string `abacbb`, tokenization proceeds as follows:

```plaintext
1. a b a c b b
2. a b ac  b b
3. a b ac  bb
4. ab  ac  bb
5. ab  acbb
```

Note that initially we have several pairs of single-byte tokens that appear in the dictionary, such as `a b` and `a c`. Even though `ab` appears earlier in the string, `ac` is chosen because the token appears first in the token dictionary. It is this behavior that makes BPE non-incremental with respect to string operations such as slicing or appending. For example, the substring `abacb` is tokenized as `ab ac b`, but if another `b` is added, the resulting string `abacbb` is tokenized as `ab acbb`. Two tokens from the prefix `abacb` are gone, and the encoding for the longer string even ends up being shorter.

The two main strategies for implementing BPE are:

- A naive approach that repeatedly iterates over the tokens to merge the next eligible token pair, resulting in quadratic complexity.
- A heap-based approach that keeps eligible token pairs sorted, resulting in _O(n log(n))_ complexity.

However, all tokenizers require the full text up front. Tokenizing a substring or an extended string means starting the encoding from scratch. For that reason, the more interesting use cases from above quickly become very expensive (at least _O(n2 log(n))_). So, how can we do better?

## Composing valid encodings

The difficulty of the byte-pair encoding algorithm (as described above) is that token pair replacements can happen anywhere in the string and can influence the final tokens at the beginning of the string. However, it turns out that there is a property, which we call _compatibility_, that allows us to build up tokenizations left-to-right:

_Given a valid encoding, we can append an additional token to produce a new valid encoding if the pair of the last token and the appended token are a valid encoding._

A valid encoding means that the original BPE algorithm produces that same encoding. We’ll show what this means with an example, and refer to the crate’s README for a detailed proof.

The sequence `ab ac` is a valid encoding for our example token dictionary.

- Is `ab ac b` a valid encoding? Check if the pair `ac b` is compatible:
    
    ```plaintext
    1. a c b
    2. ac  b
    ```
    
    We got the same tokens back, which means `ab ac b` is the encoding `ab ac b`.
    
- Is `ab ac bb` a valid encoding? Again, check if the pair `ac bb` is compatible:
    
    ```plaintext
    1. a c b b
    2. ac  b b
    3. ac  bb 
    4. acbb 
    ```
    
    In this case, the tokens are incompatible, and `ab ac bb` is not valid.
    

The next section explains how we can go from _building_ valid encodings to _finding_ the encoding for a given input string.

## Linear encoding

Using the compatibility rule, we can implement linear encoding with a dynamic programming algorithm.

The algorithm works by checking for each of the possible last tokens whether it is compatible with the tokenization of the remaining prefix. As we saw in the previous section, we only need the last token of the prefix’s encoding to decide this.

Let’s apply this idea to our example `abacbb` and write down the full encodings for every prefix:

- `a` ——-> `a`
- `ab` ——> `ab`
- `aba` —–> `ab a`
- `abac` —-> `ab ac`
- `abacb` —> `ab ac b`
- `abacbb` –> `ab acbb`

We only store the last token for every prefix. This gives us `a ab a ac b` for the first five prefixes. We can find the last token for a prefix with a simple lookup in the list. For example, the last token for `ab` is `ab`, and the last token for `abac` is `ac`.

For the last token of `abacbb` we have three token candidates: `b`, `bb`, and `acbb`. For each of these we must check whether it is compatible with the last token of the remaining prefix: `b b`, `ac bb`, or `ab acbb`. Retokenizing these combinations gives `bb`, `acbb`, and `ab acbb`, which means `acbb` is the _only_ valid choice here. The algorithm works forward by computing the last token for every position in the input, using the last tokens for previous positions in the way we just described.

The resulting algorithm looks roughly like this:

```
let last_tokens = vec![];
for pos in 0..text.len() {
  for candidate in all_potential_tokens_for_suffix(text[0..pos + 1]) {
    if token_len(candidate) == pos + 1 {
      last_tokens.push(candidate);
      break;
    } else if is_compatible(
      last_tokens[pos + 1 - token_len(candidate)],
      candidate,
    ) {
      last_tokens.push(candidate);
      break;
    }
  }
}
```

How do we implement this efficiently?

- Use an Aho-Corasick string matching automaton to get all suffix tokens of the string until position _i_. Start with the longest token, since those are more likely to survive than shorter tokens.
- Retokenize the token pair efficiently to implement the compatibility check. We could precompute and store all valid pairings, but with large token dictionaries this will be a lot of data. It turns out that we can retokenize pairs on the fly in effectively constant time.

The string matching automaton is linear for the input length. Both the number of overlapping tokens and retokenization are bounded by constants determined by the token dictionary. Together, this gives us a linear runtime.

## Putting it together

The Rust crate contains several different encoders based on this approach:

- **Appending and prepending encoders that incrementally encode a text when content is added to it.** The algorithm works using the approach outlined above, storing the last token for every text position. Whenever the text is extended, the last tokens for the new positions are computed using the previously computed last tokens. At any point the current token count is available in constant time. Furthermore, constant time snapshots and rollbacks are supported, which make it easy to implement dynamic chunk construction approaches.
- **A fast full-text encoder based on backtracking.** Instead of storing the last token for every text position, it only stores the tokens for the full input. The algorithm works left to right, picking a candidate token for the remaining input, and checking if it is compatible with the last token. The algorithm backtracks on the last token if none of the candidates are compatible. By trying the longest candidates first, very little backtracking happens in practice.
- **An interval encoder which allows _O(1)_ token counting on subranges of the original text (after preprocessing the text in _O(n)_ time).** The algorithm works by encoding the substring until the last token lines up with the token at that position for the original text. Usually, only a small prefix needs to be encoded before this alignment happens.

We have explained our algorithm at a high level so far. The crate’s README contains more technical details and is a great starting point for studying the code itself.

The post So many tokens, so little time: Introducing a faster, more flexible byte-pair tokenizer appeared first on The GitHub Blog.

Go to Source
