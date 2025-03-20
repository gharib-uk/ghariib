---
title: "How we train AI to uncover malicious JavaScript intent and make web surfing safer"
date: 2025-03-19
---

Modern websites rely heavily on JavaScript. Leveraging third-party scripts accelerates web app development, enabling organizations to deploy new features faster without building everything from scratch. However, supply chain attacks targeting third-party JavaScript are no longer just a theoretical concern — they have become a reality, as recent incidents have shown. Given the vast number of scripts and the rapid pace of updates, manually reviewing each one is not a scalable security strategy.

Cloudflare provides automated client-side protection through Page Shield. Until now, Page Shield could scan JavaScript dependencies on a web page, flagging obfuscated script content which also exfiltrates data. However, these are only indirect indicators of compromise or malicious intent. Our original approach didn’t provide clear insights into a script’s specific malicious objectives or the type of attack it was designed to execute.

Taking things a step further, we have developed a new AI model that allows us to detect the exact malicious intent behind each script. This intelligence is now integrated into Page Shield, available to all Page Shield add-on customers. We are starting with three key threat categories: Magecart, crypto mining, and malware.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/6EefJpCcho3DIjbVbQIjuz/68852b905955065e48ec2aa4648621cd/1.png)

_Screenshot of Page Shield dashboard showing results of three types of analysis._

With these improvements, Page Shield provides deeper visibility into client-side threats, empowering organizations to better protect their users from evolving security risks. This new capability is available to all Page Shield customers with the add-on. Head over to the dashboard, and you can find the new malicious code analysis for each of the scripts monitored.

In the following sections, we take a deep dive into how we developed this model.

### Training the model to detect hidden malicious intent

We built this new Page Shield AI model to detect the intent of JavaScript threats at scale. Training such a model for JavaScript comes with unique challenges, including dealing with web code written in many different styles, often obfuscated yet benign. For instance, the following three snippets serve the same function.

```
//Readable, plain code
function sayHi(name) {
  console.log(
    `Hello ${
      name ?? 
      "World" //default
    }!`
  );
}
sayHi("Internet");

//Minified
function sayHi(l){console.log(`Hello ${l??"World"}!`)}sayHi("Internet");

//Obfuscated
var h=Q;(function(V,A){var J=Q,p=V();while(!![]){try{var b=-parseInt(J('0x79'))/0x1*(-parseInt(J('0x6e'))/0x2)+-parseInt(J('0x80'))/0x3+parseInt(J('0x76'))/0x4*(-parseInt(J('0x72'))/0x5)+parseInt(J('0x6a'))/0x6+parseInt(J('0x84'))/0x7+-parseInt(J('0x6d'))/0x8*(-parseInt(J('0x7d'))/0x9)+parseInt(J('0x73'))/0xa*(-parseInt(J('0x7c'))/0xb);if(b===A)break;else p['push'](p['shift']());}catch(U){p['push'](p['shift']());}}}(S,0x22097));function sayHi(p){var Y=Q,b=(function(){var W=!![];return function(e,x){var B=W?function(){var m=Q;if(x){var G=x[m('0x71')](e,arguments);return x=null,G;}}:function(){};return W=![],B;};}()),U=b(this,function(){var s=Q,W=typeof window!==s('0x6b')?window:typeof process===s('0x6c')&&typeof require===s('0x7b')&&typeof global==='object'?global:this,e=W['console']=W['console']||{},x=[s('0x78'),s('0x70'),'info',s('0x69'),s('0x77'),'table',s('0x7f')];for(var B=0x0;B<x[s('0x83')];B++){var G=b[s('0x75')][s('0x6f')][s('0x74')](b),t=x[B],X=e[t]||G;G['__proto__']=b[s('0x74')](b),G['toString']=X[s('0x7e')]['bind'](X),e[t]=G;}});U(),console['log'](Y('0x81')+(p??Y('0x7a'))+'!');}sayHi(h('0x82'));function Q(V,A){var p=S();return Q=function(b,U){b=b-0x69;var W=p[b];return W;},Q(V,A);}function S(){var v=['Internet','length','77966Hcxgji','error','1078032RtaGFM','undefined','object','8zrzBEk','244xEPFaR','prototype','warn','apply','10LQgYRU','400TNVOzq','bind','constructor','146612cfnkCX','exception','log','1513TBJIGL','World','function','57541MkoqrR','2362383dtBFrf','toString','trace','647766YvOJOm','Hellox20'];S=function(){return v;};return S();}
```

With such a variance of styles (and many more), our machine learning solution needs to balance precision (low false positive rate), recall (don’t miss an attack vector), and speed. Here’s how we do it:

#### Using syntax trees to classify malicious code

JavaScript files are parsed into syntax trees (connected acyclic graphs). These serve as the input to a Graph Neural Network (GNN). GNNs are used because they effectively capture the interdependencies (relationships between nodes) in executing code, such as a function calling another function. This contrasts with treating the code as merely a sequence of words — something a code compiler, incidentally, does not do. Another motivation to use GNNs is the insight that the syntax trees of malicious versus benign JavaScript tend to be different. For example, it’s not rare to find attacks that consist of malicious snippets inserted into, but otherwise isolated from, the rest of a benign base code.

To parse the files, the tree-sitter library was chosen for its speed. One peculiarity of this parser, specialized for text editors, is that it parses out concrete syntax trees (CST). CSTs retain everything from the original text input, including spacing information, comments, and even nodes attempting to repair syntax errors. This differs from abstract syntax trees (AST), the data structures used in compilers, which have just the essential information to execute the underlying code while ignoring the rest. One key reason for wanting to convert the CST to an AST-like structure, is that it reduces the tree size, which in turn reduces computation and memory usage. To do that, we abstract and filter out unnecessary nodes such as code comments. Consider for instance, how the following snippet

```
x = `result: ${(10+5) *   3}`;;; //this is a comment
```

… gets converted to an AST-like representation:

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/4KweWZ4yIzOTiIcYqHC682/56c059b38ad46949e7285d84438be4c9/2.png)

_Abstract Syntax Tree (AST) representation of the sample code above. Unnecessary elements get removed (e.g. comments, spacing) whereas others get encoded in the tree structure (order of operations due to parentheses)._

One benefit of working with parsed syntax trees is that tokenization comes for free! We collect and treat the node leaves’ text as our tokens, which will be used as features (inputs) for the machine learning model. Note that multiple characters in the original input, for instance backticks to form a template string, are not treated as tokens per se, but remain encoded in the graph structure given to the GNN. (Notice in the sample tree representations the different node types, such as “assignment\_expression”). Moreover, some details in the exact text input become irrelevant in the executing AST, such as whether a string was originally written using double quotes vs. single quotes.

We encode the node tokens and node types into a matrix of counts. Currently, we lowercase the nodes' text to reduce vocabulary size, improving efficiency and reducing sparsity. Note that JavaScript is a case-sensitive language, so this is a trade-off we continue to explore. This matrix and, importantly, the information about the node edges within the tree, is the input to the GNN.

How do we deal with obfuscated code? We don’t treat it specially. Rather, we always parse the JavaScript text as is, which incidentally unescapes escape characters too. For instance, the resulting AST shown below for the following input exemplifies that:

```
atob('x55x32x56x75x5ax45x52x68x64x47x45x3d') == "SendData"
```

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/sV2qPtj8G30EFnW6rwCav/ef0a970e338e5610da08fa3ccbdeafcc/3.png)

_Abstract Syntax Tree (AST) representation of the sample code above. JavaScript escape characters are unescaped._

Moreover, our vocabulary contains several tokens that are commonly used in obfuscated code, such as double escaped hexadecimal-encoded characters. That, together with the graph structure information, is giving us satisfying results — the model successfully classifies malicious code whether it's obfuscated or not. Analogously, our model’s scores remain stable when applied to plain benign scripts compared to obfuscating them in different ways. In other words, the model’s score on a script is similar to the score on an obfuscated version of the same script. Having said that, some of our model's false positives (FPs) originate from benign but obfuscated code, so we continue to investigate how we can improve our model's intelligence.

#### Architecting the Graph Neural Network

We train a message-passing graph convolutional network (MPGCN) that processes the input trees. The message-passing layers iteratively update each node’s internal representation, encoded in a matrix, by aggregating information from its neighbors (parent and child nodes in the tree). A pooling layer then condenses this matrix into a feature vector, discarding the explicit graph structure (edge connections between nodes). At this point, standard neural network layers, such as fully connected layers, can be applied to progressively refine the representation. Finally, a softmax activation layer produces a probability distribution over the four possible classes: benign, magecart, cryptomining, and malware.

We use the TF-GNN library to implement graph neural networks, with Keras serving as the high-level frontend for model building and training. This works well for us with one exception: TF-GNN does not support sparse matrices / tensors. (That lack of support increases memory consumption, which also adds some latency.) Because of this, we are considering switching to PyTorch Geometric instead.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/79SbvJgWq3mwMks6Vtxpgs/5ab55f19b6cf90dc070b5f0d70abdde9/4.png)

_Graph neural network architecture, transforming the input tree with features down to the 4 classification probabilities._

The model’s output probabilities are finally inverted and scaled into scores (ranging from 1 to 99). The “js\_integrity” score aggregates the malicious classes (magecart, malware, cryptomining). A low score means likely malicious, and a high score means likely benign. We use this output format for consistency with other Cloudflare detection systems, such as Bot Management and the WAF Attack Score. The following diagram illustrates the preprocessing and feature analysis pipeline of the model down to the inference results.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/zLPW9kydBIJySfjaN2TTI/d7b9a7f51c1bb501aac2b7a724d62a1d/5.png)

_Model inference pipeline to sniff out and alert on malicious JavaScript._

#### Tackling unbalanced data: malicious scripts are the minority

Finding malicious scripts is like finding a needle in a haystack; they are anomalies among plenty of otherwise benign JavaScript. This naturally results in a highly imbalanced dataset. For example, our Magecart-labeled scripts only account for ~6% of the total dataset.

Not only that, but the “benign” category contains an immense variance (and amount) of JavaScript to classify. The lengths of the scripts are highly diverse (ranging from just a few bytes to several megabytes), their coding styles vary widely, some are obfuscated whereas others are not, etc. To make matters worse, malicious payloads are often just small, carefully inserted fragments within an otherwise perfectly valid and functional benign script. This all creates a cacophony of token distributions for an ML model to make sense of.

Still, our biggest problem remains finding enough malevolent JavaScript to add to our training dataset. Thus, simplifying it, our strategy for data collection and annotation is two-fold:

1. Malicious scripts are about quantity → the more, the merrier (for our model, that is 😉). Of course, we still care about quality and diversity. But because we have so few of them (in comparison to the number of benign scripts), we take what we can.
    
2. Benign scripts are about quality → the more _variance_, the merrier. Here we have the opposite situation. Because we can collect so many of them easily, the value is in adding differentiated scripts.
    

##### Learning key scripts only: reduce false positives with minimal annotation time

To filter out semantically-similar scripts (mostly benign), we employed the latest advancements in LLM for generating code embeddings. We added those scripts that are distant enough from each other to our dataset, as measured by vector cosine similarity. Our methodology is simple — for a batch of potentially new scripts:

- Initialize an empty vector database. For local experimentation, we are fans of Chroma DB.
    
- For each script:
    
    - Call an LLM to generate its embedding. We’ve had good results with starcoder2, and most recently qwen2.5-coder.
        
    - Search in the database for the top-1 closest other script’s vectors.
        
    - If the distance > threshold (0.10), select it and add it to the database.
        
    - Else, discard the script (though we consider it for further validations and tests).
        

Although this methodology has an inherent bias in gradually favoring the first seen scripts, in practice we’ve used it for batches of newly and randomly sampled JavaScript only. To review the whole existing dataset, we could employ other but similar strategies, like applying HDBSCAN to identify an unknown number of clusters and then selecting the medoids, boundary, and anomaly data points.

We’ve successfully employed this strategy for pinpointing a few highly varied scripts that were relevant for the model to learn from. Our security researchers save a tremendous amount of time on manual annotation, while false positives are drastically reduced. For instance, in a large and unlabeled bucket of scripts, one of our early evaluation models identified ~3,000 of them as malicious. That’s too many to manually review! By removing near duplicates, we narrowed the need for annotation down to only 196 samples, less than 7% of the original amount (see the t-SNE visualization below of selected points and clusters). Three of those scripts were actually malicious, one we could not fully determine, and the rest were benign. By just re-training with these new labeled scripts, a tiny fraction of our whole dataset, we reduced false positives by 50% (as gauged in the same bucket and in a controlled test set). We have consistently repeated this procedure to iteratively enhance successive model versions.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/hHw00ojXE4CdorMQI5b56/897e0e045230522478e0735c3e28ff12/6.png)

_2D visualization of scripts projected onto an embedding space, highlighting those sufficiently dissimilar from one another._

#### From the lab, to the real world

Our latest model in evaluation has both a macro accuracy and an overall malicious precision nearing 99%(!) on our test dataset. So we are done, right? Wrong! The real world is not the same as the lab, where many more variances of benign JavaScript can be seen. To further assure minimum prediction changes between model releases, we follow these three anti-fool measures:

##### Evaluate metrics uncertainty

First, we thoroughly estimate the _uncertainty_ of our offline evaluation metrics. How accurate are our accuracy metrics themselves? To gauge that, we calculate the standard error and confidence intervals for our offline metrics (precision, recall, F1 measure). To do that, we calculate the model’s predicted scores on the test set once (the original sample), and then generate bootstrapped resamples from it. We use simple random (re-)sampling as it offers us a more conservative estimate of error than stratified or balanced sampling.

We would generate 1,000 resamples, each a fraction of 15% resampled from the original test sample, then calculate the metrics for each individual resample. This results in a distribution of sampled data points. We measure its mean, the standard deviation (with Bessel’s correction), and finally the standard error and a confidence interval (CI) (using the percentile method, such as the 2.5 and 97.5 percentiles for a 95% CI). See below for an example of a bootstrapped distribution for precision (P), illustrating that a model’s performance is a continuum rather than a fixed value, and that might exhibit subtly (left-)skewed tails. For some of our internally evaluated models, it can easily happen that some of the sub-sampled metrics decrease by up to 20 percentage points within a 95% confidence range. High standard errors and/or confidence ranges signal needs for model improvement and for improving and increasing our test set.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/2x3X2oVv2EfIkLYFrcjLWK/985a685e565f759b7781821595ac4ff7/7.png)

_An evaluation metric, here precision (P), might change significantly depending on what’s exactly tested. We thoroughly estimate the metric’s standard error and confidence intervals._

##### Benchmark against massive offline unlabeled dataset

We run our model on the entire corpus of scripts seen by Cloudflare's network and temporarily cached in the last 90 days. By the way, that’s nearly 1 TiB and 26 million different JavaScript files! With that, we can observe the model’s behavior against real traffic, yet completely offline (to ensure no impact to production). We check the malicious prediction rate, latency, throughput, etc. and sample some of the predictions for verification and annotation.

##### Review in staging and shadow mode

Only after all the previous checks were cleared, we then run this new tentative version in our staging environment. For major model upgrades, we also deploy them in shadow mode (log-only mode) — running on production, alongside our existing model. We study the model’s behavior for a while before finally marking it as production ready, otherwise we go back to the drawing board.

### AI inference at scale

At the time of writing, Page Shield sees an average of _40,000 scripts per second_. Many of those scripts are repeated, though. Everything on the Internet follows a Zipf's law distribution, and JavaScript seen on the Cloudflare network is no exception. For instance, it is estimated that different versions of the Bootstrap library run on more than 20% of websites. It would be a waste of computing resources if we repeatedly re-ran the AI model for the very same inputs — inference result caching is needed. Not to mention, GPU utilization is expensive!

The question is, what is the best way to cache the scripts? We could take an SHA-256 hash of the plain content as is. However, any single change in the transmitted content (comments, spacing, or a different character set) changes the SHA-256 output hash.

A better caching approach? Since we need to parse the code into syntax trees for our GNN model anyway, this tree structure and content is what we use to hash the JavaScript. As described above, we filter out nodes in the syntax tree like comments or empty statements. In addition, some irrelevant details get abstracted out in the AST (escape sequences are unescaped, the way of writing strings is normalized, unnecessary parentheses are removed for the operations order is encoded in the tree, etc.).

Using such a tree-based approach to caching, we can conclude that at any moment over 99.9% of reported scripts have already been seen in our network! Unless we deploy a new model with significant improvements, we don’t re-score previously seen JavaScript but just return the cached score. As a result, the model only needs to be called _fewer than 10 times per minute_, even during peak times!

### Let AI help ease PCI DSS v4 compliance

One of the most popular use cases for deploying Page Shield is to help meet the two new client-side security requirements in PCI DSS v4 — 6.4.3 and 11.6.1. These requirements make companies responsible for approving scripts used in payment pages, where payment card data could be compromised by malicious JavaScript. Both of these requirements become effective on March 31, 2025.

Page Shield with AI malicious JavaScript detection can be deployed with just a few clicks, especially if your website is already proxied through Cloudflare. Sign up here to fast track your onboarding!

Go to Source
