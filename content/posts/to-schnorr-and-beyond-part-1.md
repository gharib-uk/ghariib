---
title: "To Schnorr and beyond (Part 1)"
date: 2025-01-15
categories: 
  - "general"
tags: 
  - "ces"
  - "lg"
  - "llm"
---

_Warning: extremely wonky cryptography post. Also, possibly stupid and bound for nowhere._

One of the hardest problems in applied cryptography (and perhaps all of computer science!) is explaining _why_ our tools work the way they do. After all, we’ve been gifted an amazing basket of useful algorithms from those who came before us. Hence it’s perfectly understandable for practitioners to want to take those gifts and simply start to apply them. But sometimes this approach leaves us wondering _why_ we’re doing certain things: in these cases it’s helpful to take a step back and think about what’s actually going on, and perhaps what was in the inventors’ heads when the tools were first invented.

In this post I’m going to talk about signature schemes, and specifically the Schnorr signature, as well as some related schemes like ECDSA. These signature schemes have a handful of unique properties that make them quite special among cryptographic constructions. Moreover, understanding the motivation of Schnorr signatures can help understand a number of more recent proposals, including post-quantum schemes like Dilithium — which we’ll discuss in the second part of this series.

As a motivation for this post, I want to talk about this tweet:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/09/schnorrpeikert.png?w=1024)

Instead of just dumping Schnorr signatures onto you, I’m going to take a more circuitous approach. Here we’ll start from the very most basic building blocks (including the basic concept of an identification protocol) and then work our way gradually towards an abstract framework.

### Identification protocols: our most useful building block

If you want to understand Schnorr signatures, the very first thing you need to understand is that they weren’t really designed to be signatures at all, at least not at first. The Schnorr protocol was designed as an interactive _identification_ scheme, which can be “flattened” into the signature scheme we know and love.

An identification scheme consists of a key generation algorithm for generating a “keypair” comprising a public and secret key, as well as an interactive protocol (the “identification protocol”) that uses these keys. The public key represents its owners’ identity, and can be given out to anyone. The secret key is, naturally, _secret_. We will assume that it is carefully stored by its owner, who can later use it to prove that she “owns” the public key.

The identification protocol itself is run interactively between two parties — meaning that the parties will exchange multiple messages in each direction. We’ll often call these parties the “prover” and the “verifier”, and many older papers used to give them cute names like “Peggy” and “Victor”. I find this slightly twee, but will adopt those names for this discussion just because I don’t have any better ideas.

To begin the identification protocol, Victor must obtain a copy of Peggy’s public key. Peggy for her part will possess her secret key. The goal of the protocol is for Victor to decide whether he trusts Peggy:

<figure>

![](https://blog.cryptographyengineering.com/wp-content/uploads/2022/07/image-1.png?w=1024)

<figcaption>

High level view of a generic interactive identification protocol. We’ll assume the public key was generated in a previous key generation phase. (No, I don’t know why the Verifier has a tennis racket.)

</figcaption>

</figure>

Note that this “proof of ownership” does not need to be 100% perfect. We only ask that it is sound _with extremely high probability_. Roughly speaking, we want to ensure that if Peggy really owns the key, then Victor will always be convinced of this fact. At the same time, someone who is impersonating Peggy — i.e., does not know her secret key — should fail to convince Victor, except with some astronomically small (negligible) probability.

(Why do we accept this tiny probability of an impersonator succeeding? It turns out that this is basically unavoidable for any identification protocol. This is because the number of bits Peggy sends to Victor must be finite, and we already said there must exist at least one “successful” response that will make Victor accept. Hence there clearly exists an adversary who just _guesses_ the right strings and gets lucky very ocasionally. As long as the number of bits Peggy sends is reasonably large, then such a “dumb” adversary should almost never succeed, but they will do so with non-zero probability.)

The above description is nearly sufficient to explain the security goals of an identification scheme, and yet it’s not quite complete. If it was, then there would be a very simple (and yet obviously _bad)_ protocol that solves the problem: the Prover could simply transmit its secret key to the Verifier, who can presumably test that it matches with the public key:

<figure>

![](https://blog.cryptographyengineering.com/wp-content/uploads/2022/07/image-2.png?w=1024)

<figcaption>

This usually works, but _don’t do this, please._

</figcaption>

</figure>

If all we cared about was solving the basic problem of proving ownership _in a world with exactly one Verifier who only needs to run the protocol once,_ the protocol above would work fine! Unfortunately in the real world we often need to prove identity to multiple different Verifiers, or to repeatedly convince the same Verifier of our identity. The problem with the strawman proposal above is that at the end of a single execution, Victor has learned Peggy’s secret key (as does anyone else who happened to eavesdrop on their communication.) This means that Victor, or any eavesdropper, will now be able to impersonate Peggy in future interactions.

And that’s a fairly bad feature for an identification protocol. To deal with this problem, a truly useful identification protocol should add at least one additional security requirement: _at the completion of this protocol, Victor (or an eavesdropper) should not gain the ability to mimic Peggy’s identity to another Verifier_. The above protocol clearly fails this requirement, since Victor will now possess all of the secret information that Peggy once had.

This requirement also helps to why identification protocols are (necessarily) interactive, or at least _stateful_: even if Victor did not receive Peggy’s secret key, he might still be able to record any messages sent by Peggy during her execution of the protocol with him. If the protocol was fully non-interactive (meaning, it consists of exactly one message from Peggy to Victor) then Victor could later “replay” his recorded message to some other Verifier, thus convincing that person that he is actually Peggy. Many protocols have suffered from this problem, including older vehicle immobilizers.

The classical solution to this problem is to organize the identification protocol to have a _challenge-response_ structure, consisting of multiple interactive moves. In this approach, Victor first sends some random “challenge” message to Peggy, and then Peggy then constructs her response so that it is specifically based on Victor’s challenge. Should a malicious Victor attempt to impersonate Peggy to a different Verifier, say Veronica, the expectation is that Veronica will send a _different_ challenge value (with high probability), and so Victor will not be able to use Peggy’s original response to satisfy Veronica’s new challenge.

(While interaction is generally required, in some instances we can seemingly “sneak around” this requirement by “extracting a challenge from the environment.” For example, real-world protocols will sometimes ‘bind’ the identification protocol to metadata such as a timestamp, transaction details, or the Verifier’s name. This doesn’t strictly prevent replay attacks — replays of one-message protocols are always possible! — but it can help Verifiers detect and reject such replays. For example, Veronica might not accept messages with out-of-date timestamps. I would further argue that, if one squints hard enough, these protocols are still _interactive_. It’s just that the first move of the interaction \[say, querying the clock for a timestamp\] is now being moved outside of the protocol.)

### How do we build identification schemes?

Once you’ve come up with the idea of an identification scheme, the obvious question is how to build one.

The simplest idea you might come up with is to use some one-way function as your basic building block. The critical feature of these functions is that they are “easy” to compute in one direction (_e.g.,_ for some string _x_, the function _F(x)_ can be computed very efficiently.) At the same time, one-way functions are hard to _invert:_ this means that given _F(x)_ for some random input string _x_ — let’s imagine _x_ is something like a 128-bit string in this example — it should take an unreasonable amount of computational effort to recover x.

I’m selecting one-way functions because we have a number of candidates for them, including cryptographic hash functions as well as fancier number-theoretic constructions. Theoretical cryptographers also prefer them to other assumptions, in the sense that the existence of such functions is considered to be one of the most “plausible” cryptographic assumptions we have, which means that they’re much likelier to exist than more fancy building blocks.

The problem is that building a good identification protocol from simple one-way functions is challenging. An obvious starting point for such a protocol would be for Peggy to construct her secret key by selecting a random string _sk_ (for example, a 128-bit random string) and then computing her public key as _pk = F(sk)_.

Now to conduct the identification protocol, Peggy would… um… well, it’s not really clear what she would do.

The “obvious” answer would be for Peggy to send her secret key _sk_ over to Victor, and then Victor could just check that _pk = F(sk)_. But this is obviously bad for the reasons discussed above: Victor would then be able to impersonate Peggy after she conducted the protocol with him even one time. And fixing this problem turns out to be somewhat non-trivial!

There are, of course, some clever solutions — but each one entails some limitations and costs. A “folklore”1 approach works like this:

1. Instead of picking _one_ secret string, Peggy picks _N different_ secret strings ![sk_1, dots, sk_N](https://s0.wp.com/latex.php?latex=sk_1%2C+%5Cdots%2C+sk_N&bg=ffffff&fg=000000&s=0&c=20201002) to be her “secret key.”

4. She now sets her “public key” to be ![pk = F(sk_1), dots, F(sk_N)](https://s0.wp.com/latex.php?latex=pk+%3D+F%28sk_1%29%2C+%5Cdots%2C+F%28sk_N%29&bg=ffffff&fg=000000&s=0&c=20201002).

7. In the identification protocol, Victor will challenge Peggy by asking her for a random _k_\-sized subset of Peggy’s strings (here _k_ is much smaller than _N_.)

10. Peggy will send back the appropriate list of _k_ secret strings.

13. Victor will check each string against the appropriate position in Peggy’s public key.

The idea here is that, after running this protocol one time, Victor learns some _but not all_ of Peggy’s secret strings. If Victor was then to attempt to impersonate Peggy to another person — say, Veronica — then Veronica would pick _her own random sub_set of _k_ strings for Victor to respond to. If this subset is identical to the one Victor chose when he interacted with Peggy, then Victor will succeed: otherwise, Victor will not be able to answer Veronica’s challenge. By carefully selecting the values of _N_ and _k_, we can ensure that this probability is very small.2

An obvious problem with this proposal is that it falls apart very quickly _if Victor can convince Peggy to run the protocol with him multiple times._

If Victor can send Peggy several different challenges, he will learn many more than _k_ of Peggy’s secret strings. As the number of strings Victor learns increases, Victor’s ability to answer Veronica’s queries will improve dramatically: eventually he will be able to impersonate Peggy nearly all of the time. There are some clever ways to address this problem while still using simple one-way functions, but they all tend to be relatively “advanced” and costly in terms of bandwidth and computation. (I promise to talk about them in some other post.)

### Schnorr

So far we have a motivation: we would like to build an identification protocol that is _multi-use_ — in the sense that Peggy can run the protocol many times with Victor (or other verifiers) without losing security. And yet one that is also _efficient_ in the sense that Peggy doesn’t have to exchange a huge amount of data with Victor, or have huge public keys.

Now there have been a large number of identity protocols. Schnorr is not even the first one to propose several of the ideas it uses. “Schnorr” just happens to be the name we generally use for a class of efficient protocols that meet this specific set of requirements.

Some time back when Twitter was still Twitter, I asked if anyone could describe the rationale for the Schnorr protocol in two tweets or less. I admit I was fishing for a particular answer, and I got it from Chris Peikert:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/09/schnorrpeikert.png?w=1024)

I really like Chris’s explanation of the Schnorr protocol, and it’s something I’ve wanted to unpack for while now. I promise that all you really need to understand this is a little bit of middle-school algebra and a “magic box”, which we’ll do away with later on.

Let’s tackle it one step at a time.

First, Chris proposes that Peggy must choose “a random line.” Recalling our grade-school algebra, the equation for a line is _y = mx + b_, where “_m”_ is the line’s slope and “_b_” its _y-_intercept. Hence, Chris is really asking us to select a pair of random numbers (_m, b_). (For the purposes of this informal discussion you can just pretend these are real numbers in some range. However later on we’ll have them be elements of a very large finite field or ring, which will eliminate many obvious objections.)

Here we will let “_m_” be Peggy’s secret key, which she will choose one time and keep the same forever. Peggy will choose a fresh random value “_b_” each time she runs the protocol. Critically, Peggy will put both of those numbers into a pair of Chris’s magic box(es) and send them over to Victor.

Finally, Victor will challenge Peggy to evaluate her line at one specific (random) point _x_ that he selects. This is easy for Peggy, who can compute the corresponding value _y_ using her linear equation. Now Victor possesses a point (_x, y_) that — if Peggy answered correctly — should lie on the line defined by (_m, b_). He simply needs to use the “magic boxes” to check this fact.

Here’s the whole protocol:

<figure>

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/09/untitled-2.png?w=1024)

<figcaption>

Chris Peikert’s “magic box” protocol. The only thing I’ve changed from his explanation is that there are now _two_ magic boxes, one that contains “_m_” and one that contains “_b_“. Victor can use them together to check Peggy’s response _y_ at the end of the protocol.

</figcaption>

</figure>

Clearly this is not a real protocol, since it relies fundamentally on magic. With that said, we can still observe some nice features about it.

A first thing we can observe about this protocol is that _if the final check is satisfied,_ then Victor should be reasonably convinced that he’s really talking to Peggy. Intuitively, here’s a (non-formal!) argument for why this is the case. Notice that to complete the protocol, Peggy must answer Victor’s query on _any random x_ that Victor chooses. If Peggy, or someone impersonating Peggy, is able to do this with high probability for any random point _x_ that Victor might choose_,_ then intuitively it’s reasonable that she could (in her own head, at least) compute a similar response for a second random point _x’_. Critically, given two separate points _(x,y), (x’, y’)_ all on the same line, it’s easy to calculate the secret slope _m_ — ergo, a person who can easily compute points on a line almost certainly knows Peggy’s secret key. (This is not a proof! It’s only an intuition. However the real proof uses a similar principle.2)

The question, then, is what Victor learns after running the protocol with Peggy.

If we ignore the magical aspects of the protocol, the only thing that Victor “learns” by at end of the protocol is a single point (_x, y_) that happens to lie on the random line chosen by Peggy. Fortunately, this doesn’t reveal very much about Peggy’s line, and in particular, it reveals very little about her secret (slope) key. The reason is that for every possible slope value _m_ that Peggy might have chosen as her key, there exists a value _b_ that produces a line that intersects (_x, y_). We can illustrate this graphically for a few different examples:

<figure>

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/09/points.png?w=762)

<figcaption>

I obviously did not use a graphing tool to make this disaster.

</figcaption>

</figure>

Naturally this falls apart if Victor sees _two_ different points on the same line. Fortunately this never happens, because Peggy chooses a different line (by selecting a new _b_ value) every time she runs the protocol. (It would be a terrible disaster if she forgot to do this!)

The existence of these magic boxes obviously makes security a bit harder to think about, since now Victor can do various tests using the “boxes” to test out different values of _m_, _b_ to see if he can find a secret line that matches. But fortunately these boxes are “magic”, in the sense that all Victor can really do is _test_ whether his guesses are successful: provided there are many possible values of _m_, this means actually searching for a matching value will take far too long to be useful.

Now, you might ask: _why a line?_ Why not a plane, or a degree-8 polynomial?

The answer is pretty simple: a line happens to be one of the simplest mathematical structures that suits our needs. We require an equation for which we can “safely” reveal exactly one solution, without fully constraining the terms of its equation. Higher-degree polynomials and planar equations also possess this capability (indeed we can reveal more points in these structures), but each has a larger and more complex equation that would necessitate a fancier “magic box.”

### How do we know if the “magic box” is magic enough?

Normally when people learn Schnorr, they are not taught about magic boxes. In fact, they’re typically presented with a bunch of boring details about cyclic groups.

The problem with that approach is that it doesn’t teach us anything about what we need from that magic box. And that’s a shame, because there is not one specific box we can use to realize this class of protocols. Indeed, it’s better to think of this protocol as a set of general ideas that can be filled in, or “instantiated” with different ingredients.

Hence: I’m going to try a different approach. Rather than just provide you with something that works to realize our magic box as a _fait accompli_, let’s instead try to figure out what properties our magical box must have, in order for it to provide us with a secure protocol.

### Simulating Peggy

There are essentially three requirements for a secure identification protocol. First, the protocol needs to be _correct —_ meaning that Victor is always convinced following a legitimate interaction with Peggy. Second, it needs to be _sound,_ meaning that only Peggy (and not an impersonator) can convince Victor to accept.

We’ve made an informal argument for both of these properties above. It’s important to note that each of these arguments relies primarily on the fact that our magic box _works_ as advertised — i.e., Victor can reliably “test” Peggy’s response against the boxed information. Soundness also requires that bad players cannot “unbox” Peggy’s secret key and fully recover her secret slope _m_, which is something that should be true of any one-way function.

But these arguments don’t dwell thoroughly on how secure the boxes must be. Is it ok if an attacker can learn a few bits of _m_ and _b_? Or do they need to be completely ideal. To address these questions, we need to consider a third requirement.

That requirement is that that Victor, having run the protocol with Peggy, should not learn anything more useful than he already knew from having Peggy’s public key. This argument really requires us to argue that these boxes are quite strong — i.e., they’re not going to leak any useful information about the valuable secrets beyond what Victor can get from black-box testing.

Recall that our basic concern here is that Victor will run the protocol with Peggy, possibly multiple times. At the end of each run of the protocol, Victor will learn a “transcript”. This contents of this transcript are 1) one magic box containing “_b_“, 2) the challenge value _x_ that Victor chose, and 3) the response _y_ that Peggy answered with. We are also going to assume that Victor chose the value _x_ “honestly” at random, so really there are only two interesting values that he obtained from Peggy.

A question we might ask is: _how useful is the information in this transcript to Victor,_ assuming he wants to do something creepy like pretend to be Peggy_?_

Ideally, the answer should be “not very useful at all.”

The clever way to argue this point is to show that Victor can perfectly “simulate” these transcripts without every even talking to Peggy at all. The argument thus proceeds as follows: if Victor (all by his lonesome) can manufacture a transcript that is statistically _identical_ to the ones he’d get from talking to Peggy, then what precisely has he “learned” from getting real ones from Peggy at all? Implicitly the answer is: not very much.

So let’s take a moment to think about how Victor might (all by himself) produce a “fake” transcript without talking to Peggy. As a reminder, here’s the “magic box” protocol from up above:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/09/untitled-3.png?w=1024)

One obvious (wrong) idea for simulating a transcript is that Victor could first select some random value _b_, and put it into a brand new “magic box”. Then he can pick _x_ at random, as in the real protocol. But this straightforward attempt crashes pretty quickly: Victor will have a hard time computing _y = mx + b_, since he doesn’t know Peggy’s secret key _m._ His best attempt, as we discussed, would be to _guess_ different values and test them, which will take too long (if the field is large.)

So clearly this approach does not work. But note that Victor doesn’t necessarily need to fake this transcript “in order.” An alternative idea is that Victor can try to make a fake transcript by working through the protocol in a different order. Specifically:

1. Victor can pick a random _x,_ just as in the real protocol.

4. Now he can pick the value _y_ also at random.  
    _Note that for every “m” there will exist a line that passes through (x, y)._

7. But now Victor has a problem: to complete the protocol, he will need to make a new box containing “b”, such that _b = y – mx._

There is no obvious way for Victor to calculate _b_ given only the information he has in the clear. To address this third requirement, we must therefore demand a fundamentally new capability from our magic boxes. Concretely, we can imagine that there is some way to “manufacture” new magic boxes from existing ones, such that the new boxes contain a calculated value. This amounts to reversing the linear equation and then performing multiplication and subtraction on “boxed” values, so that we end up with:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/09/eqn2.png?w=428)

What’s that, you say? This new requirement looks totally arbitrary? Well, of course it is. But let’s keep in mind that we started out by demanding _magical boxes_ with special capabilities. Now I’m simply adding one more magical capability. Who’s to say that I can’t do this?

Recall that the resulting transcript must be _statistically identical_ to the ones that Victor would get from Peggy. It’s easy enough to show that the literal values (_x, y, b_) will all have the same distribution in both versions. The statistical distribution of our “manufactured magical boxes” is a little bit more complicated, because what the heck does it mean to “manufacture a box from another box,” anyway? But we’ll just specify that the manufactured ones must look identical to the ones created in the real protocol.

Of course back in the real world this matters a lot. We’ll need to make sure that our magical box objects have the necessary features, which are (1) the ability to test whether a given (_x, y_) is on the line, and (2) the ability to manufacture new boxes containing “_b_” from another box containing “_m_” and a point (_x, y_), while ensuring that the manufactured boxes are identical to magical boxes made the ordinary way.

### How do we build a magical box?

An obvious idea might be to place the secret values _m_ and _b_ each into a standard one-way function and then send over _F(m)_ and _F(b)_. This clearly achieves the goal of _hiding_ the values of these two values: unfortunately, it doesn’t let us do very much else with them.

Indeed, the biggest problem with simple one-way functions is that there is only _one thing you can do with them._ That is: you can generate a secret _x,_ you can compute the one-way function _F(x)_, and then you can reveal _x_ for someone else to verify. Once you’ve done this, the secret is “gone.” That makes simple one-way functions fairly limiting.

But what if _F_ is a different type of one-way function that has some additional capabilities?

In the early 1980s many researchers were thinking about such one-way functions. More concretely, researchers such as Tahir Elgamal were looking at a then-new “candidate” one-way function that had been proposed by Whitfield Diffie and Martin Hellman, for use in their eponymous key exchange protocol.

Concretely: let _p_ be some large non-secret prime number that defines a finite field. And let _g_ be the “generator” of some large cyclic subgroup of prime order _q_ contained within that field.3 If these values are chosen appropriately, we can define a function _F(x)_ as follows:

![F(x) = g^x~mod~p](https://s0.wp.com/latex.php?latex=F%28x%29+%3D+g%5Ex~mod~p&bg=ffffff&fg=000000&s=0&c=20201002)

The nice thing about this function is that, provided _g_ and _p_ are selected appropriately, it is (1) easy to compute this function in the normal direction (using square-and-multiply modular exponentiation) and yet is (2) generally believed to be hard to invert. Concretely, as long _x_ is randomly selected from the finite field defined by _{0, …, q-1}_, then recovering _x_ from _F(x)_ is equivalent to the discrete logarithm problem.

But what’s particularly nifty about this function is that it has nice algebraic properties. Concretely, given _F(a)_ and _F(b)_ computed using the function above, we can easily compute _F(a + b mod q)_. This is because:

![g^a cdot g^b~mod~p = g^{a+b~mod~q}~mod~p](https://s0.wp.com/latex.php?latex=g%5Ea+%5Ccdot+g%5Eb~mod~p+%3D+g%5E%7Ba%2Bb~mod~q%7D~mod~p&bg=ffffff&fg=000000&s=0&c=20201002)

Similarly, given _F(a)_ and some known scalar _c_, we can compute F(a cdot c):

![(g^a)^c~mod~p= g^{a cdot c~mod~q}~mod~p](https://s0.wp.com/latex.php?latex=%28g%5Ea%29%5Ec~mod~p%3D+g%5E%7Ba+%5Ccdot+c~mod~q%7D~mod~p&bg=ffffff&fg=000000&s=0&c=20201002)

We can also combine these capabilities. Given _F(m)_ and _F(b)_ and some _x,_ we can compute F(y) where y _= mx + b_ mod q. Almost magically means we can compute linear equations over values that have been “hidden” inside a one-way function, and then we can compare the result to a direct (alleged) calculation of _y_ that someone else has handed us:

![(g^y)~mod~p = (g^{m})^x cdot g^{b}~mod~p](https://s0.wp.com/latex.php?latex=%28g%5Ey%29~mod~p+%3D+%28g%5E%7Bm%7D%29%5Ex+%5Ccdot+g%5E%7Bb%7D~mod~p&bg=ffffff&fg=000000&s=0&c=20201002)

Implicitly, this gives us the magic box we need to realize Chris’s protocol from the previous section. The final protocol looks like this:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/09/finalschnorr.png?w=1024)

Appropriate cyclic groups can also be constructed within certain elliptic curves, such as the NIST P-256 and secp256k1 curve (used for Schnorr signatures in Bitcoin) as well as the EdDSA standard, which is simply a Schnorr signature implemented in the Ed25519 Edwards curve. Here the exponentiation is replaced with scalar point multiplication, but the core principles are exactly the same.

For most people, you’re probably done at this point. You may have accepted my claim that these “discrete logarithm”-based one-way functions are sufficient to hide the values (_m, b_) and hence they’re magic-box-like.

But you shouldn’t! This is actually a terrible thing for you to accept. After all, modular-exponentiation functions are _not_ magical boxes. They’re real “things” that might potentially leak information about the points “m” and “b”, particularly since Victor will be given many different values to work with after several runs of the protocol.

To convince ourselves that the boxes don’t leak, we must use the intuition I discussed further above. Specifically, we need to show that it’s possible to “simulate” transcripts without ever talking to Peggy herself, given only her public key ![g^m~mod~p](https://s0.wp.com/latex.php?latex=g%5Em~mod~p&bg=ffffff&fg=000000&s=0&c=20201002). Recall that in the discussion above, the approach we used was to pick a random point (_x, y)_ first, and then “manufacture” a box as follows:

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/09/eqn2.png?w=428)

In our realized setting, this is equivalent to computing ![g^b~mod~p](https://s0.wp.com/latex.php?latex=g%5Eb~mod~p&bg=ffffff&fg=000000&s=0&c=20201002) directly from ![g^m~mod~p](https://s0.wp.com/latex.php?latex=g%5Em~mod~p&bg=ffffff&fg=000000&s=0&c=20201002) and _(x, y)._ Which we can do as follows:

![g^b~mod~p = frac{g^y~mod~p}{(g^m~mod~p)^x~mod~p}](https://s0.wp.com/latex.php?latex=g%5Eb~mod~p+%3D+%5Cfrac%7Bg%5Ey~mod~p%7D%7B%28g%5Em~mod~p%29%5Ex~mod~p%7D&bg=ffffff&fg=000000&s=0&c=20201002)

(If you’re picky about things, here we’re abusing division as shorthand to imply multiplication by the multiplicative inverse of the final term.)

It’s easy enough to see that the implied value _b = y – mx_ is itself distributed identically to the real protocol as long as (x, y) are chosen randomly. In that case it holds that ![g^b~mod~p](https://s0.wp.com/latex.php?latex=g%5Eb~mod~p&bg=ffffff&fg=000000&s=0&c=20201002) will be distributed identically as well, since there is a one-to-one mapping between between each _b_ and the value in the exponent. This is an extremely convenient feature of this specific magic box. Hence we can hope that this primitive meets all of our security requirements.

### From ID protocols to signatures: Fiat-Shamir

While the post so far has been about identification _protocols_, you’ll notice that relatively few people use interactive ID protocols these days. In practice, when you hear the name “Schnorr” it’s almost always associated with signature schemes. These Schnorr signatures are quite common these days: they’re used in Bitcoin and form the basis for schemes like EdDSA.

There is, of course, a reason I’ve spent so much time on identification protocols when our goal was to get to signature schemes. That reason is due to a beautiful “trick” called the Fiat-Shamir heuristic that allows us to effortlessly move from three-move identification protocols (often called “sigma protocols”, based on the shape of the capital greek letter) to _non-interactive signatures_.

Let’s talk briefly about how this works.

The key observation of Fiat and Shamir was that Victor doesn’t really do very much within a three-move ID protocol: indeed, his major task is simply to _select a random challenge._ Surely if Peggy could choose a random challenge on her own, perhaps somehow based off a “message” of her choice, then she could eliminate the need to interact with Victor at all.

In this new setting, Peggy would compute the entire transcript on her own, and she could simply hand Victor a transcript of the protocol she ran with herself (as well as the message.) Provided the challenge value _x_ could be bound “tightly” to a message, then this would convert an interactive protocol like the Schnorr identification protocol into a signature scheme.

One obvious idea would be to take some message _M_ and compute the challenge as _x = H(M)._

Of course, as we’ve already seen above, this is a pretty terrible idea. If Peggy is allowed to know the challenge value _x_, then she can trivially “simulate” a protocol execution transcript using the approach described in the previous section — _even if she does not know the secret key_. The resulting signature would be worthless.

For Peggy to pick the challenge value _x_ by herself, therefore, she requires a strategy for generating _x_ that (1) can only be executed after she’s “committed” to her first magic box containing _b_, and (2) does not allow her predict or “steer” the value _x_ that she’ll get at the end of this process.

The critical observation made by Fiat and Shamir was that Peggy could do this if she possessed a sufficiently strong hash function _H_. Their idea was as follows. First, Peggy will generate her value _b_. Then she will place it into a “magic box” as in the normal protocol (as ![g^b](https://s0.wp.com/latex.php?latex=g%5Eb&bg=ffffff&fg=000000&s=0&c=20201002) in the instantiation above.) Finally, she will feed her boxed value(s) for both _m_ and _b_ as well as an optional “message” M into the hash function as follows:

![x = H(pk | g^b | M)](https://s0.wp.com/latex.php?latex=x+%3D+H%28pk+%5C%7C+g%5Eb+%5C%7C+M%29&bg=ffffff&fg=000000&s=0&c=20201002)

<figure>

![](https://blog.cryptographyengineering.com/wp-content/uploads/2023/10/image-1.png?w=440)

<figcaption>

An evasive puzzle.

</figcaption>

</figure>

Finally, she’ll compute the rest of the protocol as expected, and hand Victor the transcript ![(g^b, M, y)](https://s0.wp.com/latex.php?latex=%28g%5Eb%2C+M%2C+y%29&bg=ffffff&fg=000000&s=0&c=20201002) which he can check by re-computing the hash function on the inputs to obtain _x_ and verifying that _y_ is correct (as in the original protocol.)

(A variant of this approach has Peggy give Victor a slightly different transcript: here she sends ![(M, x, y)](https://s0.wp.com/latex.php?latex=%28M%2C+x%2C+y%29&bg=ffffff&fg=000000&s=0&c=20201002) to Victor, who now computes ![B = frac{g^y}{pk^{x}}](https://s0.wp.com/latex.php?latex=B+%3D+%5Cfrac%7Bg%5Ey%7D%7Bpk%5E%7Bx%7D%7D&bg=ffffff&fg=000000&s=0&c=20201002) and tests whether ![x = H(pk | B | M)](https://s0.wp.com/latex.php?latex=x+%3D+H%28pk+%5C%7C+B+%5C%7C+M%29&bg=ffffff&fg=000000&s=0&c=20201002). I will leave the logic of this equation for the reader to work out. Commenter Imuli below points to a great StackExchange post that shows all the different variants of Schnorr people have built by using tricks like this.)

For this entire idea to work properly, it must be hard for Peggy to identify a useful input to the hash function that provides an output that she can use to fake the transcript. In practice, this requires a hash function where the “relation” between input and output is what we call _evasive_: namely, that it is hard to find two points that have a useful relationship for simulating the protocol.

In practice we often model these hash functions in security proofs as though they’re random functions, which means the output is _verifiably_ unrelated to the input. For long and boring reasons, this model is a bit contrived. We still use it anyway.

### What other magic boxes might there be?

As noted above, a critical requirement of the “magic box Schnorr” style of scheme is that the boxes themselves must be instantiated by some kind of _one-way function_: that is, there must be no efficient algorithm that can recover Peggy’s random secret key from within the box she produces, at least without exhaustively testing, or using some similarly expensive (super-polynomial time) attack.

The cyclic group instantiation given above satisfies this requirement provided that the discrete logarithm problem (DLP) is hard in the specific group used to compute it. Assuming your attacker only has a classical computer, this assumption is conjectured to hold for sufficiently-large groups constructed using finite-field based cryptography and in certain elliptic curves.

But nothing says your adversary has to be a classical computer. And this should worry us, since we happen to know that the discrete logarithm problem is _not_ particularly hard to solve given an appropriate quantum computer. This is due to the existence of efficient quantum algorithms for solving the DLP (and ECDLP) based on Shor’s algorithm. To deal with this, cryptographers have come up with a variety of new signature schemes that use different assumptions.

In my next post I’m going to talk about one of those schemes, namely the Dilithium signature scheme, and show exactly how it relates to Schnorr signatures.

_This post is continued in Part 2._

_Notes:_

1. “Folklore” in cryptography means that nobody knows who came up with the idea. In this case these ideas were proposed in a slightly different context (one-time signatures) by folks like Ralph Merkle.

4. Since there are ![{N choose k}](https://s0.wp.com/latex.php?latex=%7BN+%5Cchoose+k%7D&bg=ffffff&fg=000000&s=0&c=20201002) distinct subsets to pick from, the probability that Veronica will select _exactly the same subset_ as Victor did — allowing him to answer her challenge properly — can be made quite small, provided _N_ and _k_ are chosen carefully. (For example, N=128 and k=30 gives about ![{N choose k} approx 2^{96}](https://s0.wp.com/latex.php?latex=%7BN+%5Cchoose+k%7D+%5Capprox+2%5E%7B96%7D&bg=ffffff&fg=000000&s=0&c=20201002) and so Evil Victor will almost never succeed.)

7. Some of these ideas date back to the Elgamal signature scheme, although that scheme does not have a nice security reduction.

10. In the real proof, we actually rely on a property called “rewinding.” Here we can make the statement that if there exists some algorithm (more specifically, an efficient probabilistic Turing Machine) that, given only Peggy’s public key, can impersonate Peggy with high probability: then it must be possible to “extract” Peggy’s secret value _m_ from this algorithm. Here we rely on the fact that if we are handed such a Turing machine, we can run it (at least) twice while feeding in the same random tape, but specifying two different _x_ challenges. If such an algorithm succeeds with reasonable probability in the general case, then we should be able to obtain two distinct points (_x, y_), (_x’, y’_) and then we can just solve for (_m, b_).

13. I’m specifying a prime-order subgroup here not because it’s _strictly_ necessary, but because it’s very convenient to have our “exponent” values in the finite field defined by _{0, …, q-1}_ for some prime q. To construct such groups, you must select the primes _q, p_ such that _p = 2q + 1_. This ensures that there will exist a subgroup of order _q_ within the larger group defined by the field _Fp_.

Go to Source
