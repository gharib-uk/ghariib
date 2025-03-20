---
title: "3 Questions: Ron Rivest on trusting electronic voting systems"
date: 2025-01-06
categories: 
  - "cryptography"
  - "cryptography-library"
  - "cybersecurity"
  - "cybersecurity-awareness"
  - "encryption"
  - "security"
tags: 
  - "dataprivacy"
  - "dataprotection"
  - "interview"
  - "mitschwarzmancollegeofcomputing"
  - "schoolofhumanitiesartsandsocialsciences"
---

_Ron Rivest is an MIT Institute Professor in the Department of Electrical Engineering and Computer Science. He’s an authority on algorithms and an inventor of the RSA public-key cryptosystem, one of the most widely used algorithms to securely transmit data. Since the 1980s, he’s taught students how to use cryptography to help secure voting systems. Then, in 2000, an historic recount in Florida determined the outcome of the U.S. presidential election, and the Caltech / MIT Voting Technology Project was founded with the mission to secure future elections, pulling in Rivest, who has been involved since, as well as other MIT faculty from the Department of Political Science and the MIT Sloan School of Management._

_For five years, Rivest advised the U.S. Election Assistance Commission, where he helped set standards for voting system certification. In that time, he became an advocate for keeping paper ballots and auditing election outcomes based on a statistical analysis of a random sample of ballots, recommended steps to verify the reported outcome. In his research, he’s also developed technologies to use cryptography for voting, helping to secure elections dependent on electronic records._

_As election security becomes a top concern in the United States, Rivest continues applying his cryptography expertise to help improve voting systems. Here, he discusses the major issues with securing all-electronic voting systems and explains why he prefers keeping paper ballots as backup to verify voter intentions have been recorded_ — _and that the election outcome isn’t based on a computer bug._

**Q:** If an electronic voting system has been certified, does that mean it’s secure?

**A:** You have to be careful with what you expect from the certification process. You could go into it thinking, well, I’m going to have these systems certified, and because they’re certified, they’re secure, and therefore, because the voting systems are secure, I can trust the election outcomes are right. And that turns out not to be a terribly good mode of thought. For one thing, you can’t really show that something is secure by testing.

Security relates to the absence of ways an adversary can affect the election outcome. Testing a voting system may show that certain adversarial attacks don’t work, but it doesn’t reveal that there are no attacks that work. Furthermore, commercial software is well-known to have several bugs per thousand lines of code, any of which could be a security hole. Finding all bugs is well-nigh impossible, even with a vigorous testing, so certification will never provide a guarantee that a voting system is secure.

**Q:** Can an electronic voting system ever be secure?

**A:** One major problem is that you never know that the system that is running is actually the system that was tested. There are procedures in place that are supposed to ensure that, but a common way to attack a system is to attack the supply chain, so that the voting system somebody installs is not what they think it is.

You never want to be in a position where you have to say, “I trust the election outcomes because I trust the computer.” Because computers, in the end, aren’t that trustworthy. They can be manipulated. They can have their programming changed. Every day new breaches of major computer systems are reported. Computer systems just are very difficult to make secure, especially for something that's very important, like elections.

All-electronic voting systems are therefore next-to-impossible to secure. A voting system founded instead on voter-verifiable paper ballots (preferably hand-marked paper ballots) provides a basis for checking that election outcomes are correctly derived from expressed voter intentions, instead of from some computer bug.

**Q:** What’s your new philosophy when it comes to securing U.S. elections?

**A:** The new philosophy and the change of perspective I’ve adopted is not to believe an election outcome is right because you believe that machinery is doing the right thing, but rather to check that the outcome is right for each election.

You look at a sample of the paper ballots, you use some statistics, and you confirm with high confidence that the reported election outcome is consistent with that sample. A number of us have been working on that technology; Philip Stark at University of California at Berkeley is the leading statistician involved with this and the inventor of such “risk-limiting audits.”

There was a panel I was on recently for the National Academy of Sciences that produced a report called “Securing the Vote” (September 2018). That report recommended two things strongly: using paper ballots, and performing statistical post-election audits to check the tabulation of the paper ballots. In conjunction with other procedures (that, for example, ensure that the paper ballots checked are those that are cast and tabulated), one can develop confidence that our election outcomes are indeed correct.
