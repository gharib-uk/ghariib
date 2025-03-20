---
title: "<div>You Don't Need to Burn off Your Fingertips (and Other Biometric Authentication Myths)</div>"
date: 2025-01-05
categories: 
  - "cybersecurity"
  - "security"
  - "vulnerabilities"
tags: 
  - "penetrationtesting"
---

![You Don't Need to Burn off Your Fingertips (and Other Biometric Authentication Myths)](https://www.troyhunt.com/content/images/2021/09/bond-finger.jpg)

111 years ago almost to the day, a murder was committed which ultimately led to the first criminal trial to use fingerprints as evidence. We've all since watched enough crime shows to understand that fingerprints are unique personal biometric attributes and to date, no two people have ever been found to have a matching set. As technology has evolved, fingers (and palms and irises and faces) have increasingly been used as a means of biometric authentication. I'm writing this on a PC that uses a Verifi fingerprint reader. I'll probably continue to draft it from a comfy spot later on using my Lenovo laptop that has a built in reader. I'll also go backwards and forward between my iPhone and iPad with Face ID.

But doesn't this all make biometrics like passwords? What happens if someone obtains, say, my fingerprint just like they may obtain my password in a data breach or a phishing attack? I've lost count of the number of times I've heard someone say, "don't use biometrics because you can't change your fingerprints". That's an absurd statement... because you can. There are acids, blowtorches, belt sanders and the good old boxcutter, to name but a few approaches. Thing is though, whilst that may provide (some) benefit to criminals seeking to evade law enforcement matching them up to a scene of a crime, it really provides no benefit whatsoever to those who've had their biometric secrets revealed. So, stop burning off your fingertips, and read on...

### Obtaining a Print Versus Obtaining a Password

In James Bond's Diamonds Are Forever, there's a scene where 007 arrives at an apartment and is served a drink by a scantily clad villain. She then takes the glass, wanders into a back room, applies a dust to it and snaps a photo whereupon she is able to confirm that the print does indeed belong to who she thinks it does (more on that soon).

![You Don't Need to Burn off Your Fingertips (and Other Biometric Authentication Myths)](https://www.troyhunt.com/content/images/2021/09/image.png)

So, what was required to obtain the print and how does it differ from obtaining a password? Assuming we're attempting to compare them both as means of uniquely identifying an individual, of course. A knowledge of spy tradecraft, specialist equipment and perhaps most importantly, physical presence - _she had to obtain the glass with his print on it_. A password? _Very_ simple - data breaches, phishing, shoulder-surfing and in many cases, simply guessing. Only one of those requires physical presence with the rest enabling any kid anywhere in the world with an internet connection to grab troves of them with ease. It's trivial to obtain literally _billions_ of passwords associated to usernames.

### Using a Print Versus Using a Password

Because he's a clever guy, Bond didn't actually leave his own fingerprints on the glass, he left those of Peter Franks whom he was impersonating. He did this by first obtaining Franks' prints (assumedly using a similar technique to above), then creating very convincing prosthetic fingerprints which were subsequently stuck on over his own:

![You Don't Need to Burn off Your Fingertips (and Other Biometric Authentication Myths)](https://www.troyhunt.com/content/images/2021/09/bond-finger.jpg)

Ok, it was Q who created them and not Bond but hey, when you have the might of MI6 behind you there's some really cool stuff you can do! Which brings me to the point I want to get at here: creating a print that was sufficient for purpose (in this case, fooling _the human eye_ of a Bond villain), took Britain's finest spy agency. (It's also a fictional movie, but I'll come back to the real world shortly.) How does that compare to using a password? My kids know how to use passwords. You see the password, you type it into the right place, and it works! Just because you can obtain a print doesn't mean you can actually do anything useful with it, so let's look closer at verifying it.

### Verifying a Print Versus Verifying a Password

Let's do this one the other way around: what's required to verify a password? The one in storage matches the one provided at the time of authentication. That is all. Maybe (hopefully) it's hashed, but it's still the same process and all the verifier ultimately needs to do is reproduce the hash which, given the same input, will produce the same output. And fingerprints? Here's how the Bond villain did the verification:

![You Don't Need to Burn off Your Fingertips (and Other Biometric Authentication Myths)](https://www.troyhunt.com/content/images/2021/09/image-1.png)

She eyeballed it. That is all. This, arguably, is a _weak_ form of verification in that the relative difficulty of producing a functional copy is low (I know I just mentioned Q having all this MI6-backed sophistication, but this one obviously wasn't a big stretch). And this is a critical component of the discussion around the strength of biometric authentication: _how easy is it to fool the verifier?_

Google around for "fingerprint scanner hack" and you'll find plenty of material. Stories involving gummy bears are common, as are examples where nice clean prints have been lifted off smooth surfaces such as glass bottles, and there are even examples where a mere _photo_ has been used to then create a prosthetic that can fool a verifier. Which begs the question: how good is the verifier at spotting fakes?

It's a nuanced question; how good is the original print? How good is the prosthetic? How many attempts do you get before the device locks out? And perhaps most importantly, what attributes is the verifier actually reading? I've been mostly focused on fingerprints, but my iThings unlock with my face. Now we're talking about a device that uses a flood illuminator to detect the face combined with an infrared camera in conjunction with a projector that blasts 30,000 invisible dots onto your face. I've previously used a Logitech Brio on my PC with Windows Hello face authentication and like the Apple devices, we're now talking both visual and infrared imaging. All this compared to simply matching 2 strings as is done with password authentication.

### Who is Your Adversary?

So, here's where we're presently at: a password can be easily obtained, easily used and easily pass verification. A usable biometric representation is much harder to obtain, much harder to transform into a usable asset and much harder to fool a verifier with. Much harder on all fronts, but not impossible. Which begs another question: who are you worried about? Who's your adversary? _Your threat actor?_ And, most importantly, do they have the sophistication to successfully carry off an attack against biometrics?

My kids, for one. They're sneaky little buggers and I could envisage them observing my password as I type it into my PC. I can't, however, see them having the sophistication to lift a clean print off a glass, melt down a bunch of gummy bears and create a prosthetic that could fool the fingerprint reader on my PC.

Someone who steals my phone off a bar. Criminal elements are everywhere and one moment of opportunity on their behalf and its game over. They could easily have shoulder-surfed and observed me entering my PIN beforehand then it's game over. I can't, however, see them creating some sort of mannequin head with a pulse that can fool Face ID on my phone within a few goes.

A nation state actor? Yes, and there's several things they could do here. Firstly, they could go all James Bond, call in Q and break through biometric auth. Maybe - it's nontrivial even for a sophisticated actor - but then do they even need to? Maybe instead they go all FBI and San Bernardino and get in even without biometric when there's only a PIN protecting it. Or... they just beat the shit out of you until you unlock the phone for them:

![You Don't Need to Burn off Your Fingertips (and Other Biometric Authentication Myths)](https://www.troyhunt.com/content/images/2021/09/security.png)

Speaking of the authorities, I've often heard the argument from friends in the US that you shouldn't use biometrics as law enforcement can force you to unlock it. Well, if the cops are your threat actor, then don't use biometrics! Instead, just sit in jail for refusing to unlock your PIN-enabled device. Or perhaps they actually can't force you to unlock it anyway. For the vast majority of people, this whole thing about US law enforcement and PIN versus biometrics is a non-event and often ends up with increasingly absurd arguments. In one discussion I had some time back, the person arguing against biometric auth posed the following scenario: "What if you're driving along in your car and the cops are shooting your friend and you're filming it with your phone then they demand you hand over your unlocked phone so they can erase the evidence?!" What? Seriously?! _They're going to shoot you too and take your phone, that's what!_

Coming back to reality for a moment, the premise is simply this: those that have the capability to bypass biometric authentication are almost certainly not the ones that want to do so with your personal device. The ones that want to do so are unsophisticated and opportunistic. Almost without exception, _that's_ your threat actor.

### Side Channel Attacks

A fun one just to establish an important distinction. This video is less than 2 minutes so give it a quick watch:

<iframe width="100%" height="480" src="https://www.youtube.com/embed/XXW27KKHtc8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

This is not a biometric flaw, it's a flaw in the relay mechanism. You could put a keypad in place of the fingerprint reader and have precisely the same problem. I raise that here because headlines often conflate the core issue; why go to all the trouble of lifting prints and melting gummy bears when you can just pull out a magnet?! But that's not a biometric flaw 🙂

### Unhealthy Security Absolutism

This almost feels like the anti-vaxxer argument; because one in a gazillion people have an adverse reaction to a vaccine, there are those amongst us that claim vaccines are too risky despite the overwhelmingly positive impact they have on health. (These people also aren't real solid with maths.) Likewise, those that latch onto the examples where a weakness in biometrics is demonstrated whilst simultaneous discarding the strengths of the technology are doing themselves a disservice. I've written about unhealthy security absolutism in the past and it boils down to people having a myopic view of a security control where they assess it in isolation without considering how it compares to the alternatives. It's akin to the old Voltaire "Perfect is the enemy of good" aphorism.

I'll give you a few examples and these are specific to biometric on mobile devices which is where the vast majority of people currently experience auth'ing with their body parts. I wrote about Apple's introduction of Face ID 4 years ago now and in that blog post, I shared these 2 images:

![You Don't Need to Burn off Your Fingertips (and Other Biometric Authentication Myths)](https://www.troyhunt.com/content/images/2017/09/IMG_5559.jpg)![You Don't Need to Burn off Your Fingertips (and Other Biometric Authentication Myths)](https://www.troyhunt.com/content/images/2017/09/IMG_5561.jpg)

Biometrics have made a _huge_ difference to people's willingness to secure their devices because it lowers the barrier to entry. Compared to simply looking at your phone, PINs are a pain in the arse and because of that, people aren't real keen on using them. Sure, you still have a PIN on your iPhone and you'll fall back to that on multiple biometric failures or reboot, but the _prevalence_ with which you need to use it is greatly reduced. (The current face mask situation is impacting that, but it's mitigated by those who also have an Apple Watch.)

And when you _do_ unlock your biometric-enabled device, you can do so in front of people whom you wouldn't want to know your PIN. You can be on public transport, standing in a queue or even sitting down at your biometric-enabled PC with a friend and authenticate without disclosing an easily reusable secret.

The risks associated with biometrics can only ever be fairly assessed when viewed alongside the risks of _not_ using biometrics. If you're only looking at one side of the scale, you're in no position to make a rational argument for the use of one over the other.

### Summary

Let me bullet this succinctly:

1. Obtaining a usable biometric artefact is much harder than obtaining a password
2. Creating a usable biometric prosthetic is much harder than using a password
3. Fooling a biometric verifier is much harder than fooling a verifier you can provide a valid password to
4. Those with the capability to do the above 3 things are not the ones who are most likely to obtain your biometrically protected things

Use biometrics. It incentivises people to secure more things, it's resilient to all sorts of risks passwords are not and as an added bonus, it makes your digital life a whole lot easier 🙂

Go to Source
