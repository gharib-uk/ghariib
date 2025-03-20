---
title: "Convincing a billion users to love passkeys: UX design insights from Microsoft to boost adoption and security"
date: 2025-01-02
categories: 
  - "security"
---

There’s no doubt about it: The password era is ending. Bad actors know it, which is why they’re desperately accelerating password-related attacks while they still can.

At Microsoft, we block 7,000 attacks on passwords per second—almost double from a year ago. At the same time, we’ve seen adversary-in-the-middle phishing attacks increase by 146% year over year.1 Fortunately, we’ve never had a better solution to these pervasive attacks: passkeys.

Passkeys not only offer an improved user experience by letting you sign in faster with your face, fingerprint, or PIN, but they also aren’t susceptible to the same kinds of attacks as passwords. Plus, passkeys eliminate forgotten passwords and one-time codes and reduce support calls.

In this blog, we’ll share how Microsoft approached this unique opportunity to bring passkeys to consumers.

## Embracing the opportunity to improve sign-ins

In May 2024, Microsoft announced that you can sign in to your favorite consumer apps and services, such as Xbox, Microsoft 365, or Microsoft Copilot, using a passkey. Since passkeys are still a relatively new technology, as we began this journey, we asked ourselves: How are we going to convince more than a billion people to love passkeys as much as we do?

Somehow, we had to convince an incredibly large and diverse population to permanently change a familiar behavior—and be excited about it.

To make sure we got our passkey experience right, we adopted a simple methodology: Start small, experiment, then scale like crazy. The results have been encouraging:

- Signing in with a passkey is **three times faster** than using a traditional password and **eight times faster** than a password and traditional multifactor authentication.

- Users are **three times more successful** signing in with passkeys than with passwords (98% versus 32%).

- **99% of users** who start the passkey registration flow complete it.

## Step 1: Start small

Our first step was to build support for passkeys that could work across our apps. In May 2024, we added a simple option to the Microsoft account settings page to enroll a passkey:

!["Add a new way to sign in or verify" dialog box offering three options:
<div></div>
Face, fingerprint, PIN, or security key: Use a device to sign in with a passkey (accompanied by a person and key icon).
Use an app: Approve sign-in notifications on a phone (represented by a shield with a lock icon).
Email a code: Receive a code via email to sign in (depicted with an envelope icon).
A "More choices" button is displayed at the bottom.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture1.jpg)

We also added a new option to authenticate with a passkey on our sign-in page:

![	
"Microsoft Sign-in options" dialog box featuring three options:
<div></div>
Face, fingerprint, PIN, or security key: Use a device to sign in with a passkey (accompanied by a person and key icon).
Sign in with GitHub: Authenticate using a GitHub account (represented by the GitHub logo).
Forgot my username: Assistance for recovering a username (depicted with a question mark icon).
A "Back" button is located at the bottom right.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture2.jpg)

As thousands of people began enrolling and using passkeys every day, we learned a lot. For example, while the term “passkey” was sometimes unfamiliar, the phrase “face, fingerprint, or PIN” was generally well understood, so it was important to connect these two concepts in our user experience (UX).

## Step 2: Experiment

With a good foundation in place, we began to experiment. We didn’t want passkeys to be “just another way to sign in.” We wanted them to be “the best way to sign in.”

To do this, we had to figure out when, where, and how to approach users to enroll a passkey. We developed a hypothesis that a passive approach (requiring users to visit their account settings on their own to enroll a passkey) would never yield the results we wanted, so we needed to proactively invite users to enroll a passkey.

### When and where to nudge users

The most natural enrollment opportunity is when a user initially creates an account. But when and where would be the best time for existing users to create a passkey? Right after they sign in? During a password reset?

While we were cautious with any changes that might prevent our users from quickly accessing their accounts, we discovered that users were very enthusiastic about the invitation to enroll a passkey—even when they weren’t expecting it. About 25% of users who saw a nudge engaged with it—five times our pre-launch expectations. We also learned that the option to create a passkey where users manage their credentials accounted for fewer than 1% of total enrollments. These results confirmed our hypothesis.  

![A comparison chart titled "Active vs Passive" with two sections:
<div></div>
Active:
<div></div>
Three icons representing:
Account Creation (person with a briefcase icon)
After Sign-in (door with an arrow icon)
Password Reset (key icon)
A large "99%" statistic displayed at the bottom.
Passive:
<div></div>
One icon representing Account Settings (gear and document icon).
A statistic displayed as "<1%" at the bottom.
The chart uses a gradient blue-green background with white circles for the icons.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture3-1024x511.jpg)

_Figure 3. Proactive nudges at key points in the UX proved more effective for getting users to enroll a passkey._

### How best to nudge users

As we began to understand where and when to invite users to enroll passkeys, we also explored “how.” We ran multiple user studies and tested every pixel in our nudge screen to answer the question, “What would motivate a user to stop what they’re doing and enroll a passkey?”

First, we wanted to understand which value proposition would resonate most. Surprisingly, an easier sign in didn’t resonate with users as strongly as a faster or more secure sign in. Perhaps less surprising was discovering that security and speed resonated almost equally. Approximately 24% of users shown a message emphasizing security clicked through while approximately 27% of users shown messaging about speed clicked through.

![Two screenshots of a Microsoft sign in page on a mobile device, each promoting the use of a passkey for signing in. The left screenshot highlights security with the text "Sign in more securely with a passkey" and a security icon, showing a 24.07% increase in security. The right screenshot emphasizes speed with the text "Sign in faster with a passkey" and a speed icon, showing a 26.93% increase in speed. Both screenshots include the email address aliceh88@outlook.com and options to "Skip for now" or "Next."](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture4-1024x508.jpg)

_Figure 4. Messaging about “better security” and “faster sign-in” enticed more users to enroll a passkey than “ease of use.”_

If a user sees a nudge and chooses to enroll a passkey, great! But, if they see the nudge and decide that now isn’t the right time, we wanted to frame their decision in a positive way. The button text, “Skip for now,” respects that the user isn’t ready to enroll a passkey yet and lets them continue with what they were doing, but it also sets the expectation that we’re going to ask again. We’re implementing logic that determines how often to show a nudge so as not to overwhelm users, but we don’t let them permanently opt out of passkey invitations. We want users to get comfortable with the idea that passkeys will be the new normal.

![A comparison of different options tested for deferring a prompt on a Microsoft sign in screen. The left side displays various buttons with text options such as "Later," "Not now," "Maybe later," "Skip," "No thanks," and "Skip for now." The right side shows a Microsoft sign in screen with the email address aliceh88@outlook.com and a prompt to "Sign in faster with a passkey." Below the prompt, there is an option to "Skip for now" highlighted with an arrow pointing to it, and to the right of that button, a "Next" button.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture5.jpg)

_Figure 5. We don’t let users permanently opt out of passkey invitations, but we keep the messaging friendly_.

The exciting results of our experiments are helping us craft the best experience possible for our users, and we’re continuing to evolve. We encourage you to run your own experiments as well. Your products and users are different from ours and you might discover different outcomes. However, if you’re looking for a good place to start, messaging about speed and security is probably a safe bet. We also encourage you to reference the fantastic research that the FIDO Alliance has done, along with the UX guidelines they’ve published.

## Step 3: Scale

As our users began to enroll passkeys at scale, our sign-in experience needed to behave more intelligently to encourage passkey use. As we redesigned the experience, we followed these guiding principles:

- **Secure**: A great sign-in experience should prioritize security without sacrificing usability.

- **Low cognitive load**: A great sign-in experience should have low cognitive load. People don’t want to stare at a list of sign-in options to try to decide which one to use. They just want in, and we should make that easy for them.

- **Evolving**: A great sign-in experience should not only prioritize the best available method, but also continuously move users to more secure methods.

With these principles in mind, we came up with a completely reimagined sign-in experience. If the user has a passkey available, it’s always the preferred method. We don’t list all the different ways the user can sign in and ask them to choose one, we just show the passkey sign in user interface (UI) and that’s it. They are safely and quickly signed in.

![A Microsoft sign in screen showing the options to log in with TouchID sign in.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/2024-12-12-Passkey-Momentum-Blog-animated-GIF-SMALLER.gif)

_Figure 6. The sign-in experience defaults to passkey if the user has one available_.

If the user doesn’t have a passkey yet, we determine the next best available credential. Once the user successfully authenticates, we immediately invite them to enroll a passkey. If they do, then the next time they sign in, their passkey will be the best available credential and is set as the new default. **Our initial launch of this new design saw a 10% drop in password use and a 987% increase in passkey use.**

With data to support our design decisions, we’ve started setting defaults and introducing passkeys at a global scale:

- New users are invited to enroll a passkey when they create an account.

- Existing users are invited to enroll passkeys at key pivot points, such as after they sign in or during a password reset.

- Passkeys are set as the default sign-in option for users who have them.

Based on the current adoption rate, we project that hundreds of millions of new users will create and use passkeys over the coming months.

## The passwordless journey

While enrolling passkeys is an important step, it’s just the beginning. Even if we get our more than one billion users to enroll and use passkeys, if a user has both a passkey and a password, and both grant access to an account, the account is still at risk for phishing. Our ultimate goal is to remove passwords completely and have accounts that only support phishing-resistant credentials.

In 2022, we made it possible for users to completely remove their password and sign in with alternative methods. Since then, millions of users have deleted their passwords and protected themselves against password-based attacks. Now with passkeys, we can truly replace passwords with something faster, safer, and easier to use. It’s an ambitious vision, but we firmly believe in a phishing-resistant future for all scenarios, including account recovery and bootstrapping.

![The image depicts a visual representation of the progression towards more secure authentication methods, illustrated as a mountain climb. The path up the mountain is marked with milestones, each representing a step towards eliminating passwords in favor of more secure alternatives. The milestones are:
<div></div>
Passwordless accounts
Support passkeys
Passkeys by default
Passwords not supported
Phishing-resistant credentials only
Each milestone is represented by a yellow dot along a dotted path, indicating the journey towards the ultimate goal of using only phishing-resistant credentials. This image is relevant as it highlights the steps and progression towards enhancing security in digital authentication.](https://www.microsoft.com/en-us/security/blog/wp-content/uploads/2024/12/Picture7-1024x645.webp)

## Learning from our experience

Here are a few suggestions based on our learnings:

1. **Don’t be shy about inviting users to enroll passkeys.** Our experiments show that people love passkeys and are ready for them. If they don’t enroll when you first ask, don’t assume their decision is permanent. Make sure to test a few variations of your designs and copy to determine what’s most effective. We found that messages around sign-in speed and improved security resonate strongly.

4. **Make it as easy** **as possible to enroll and use passkeys.** People want quick and secure access to their accounts. They don’t want to think about signing in. Set defaults to prioritize the best available method when possible.

7. **Raise the floor.** Passkeys are an important step on the path towards a more secure and seamless authentication future. Start planning ahead now to use only phishing-resistant credentials.

Finally, we believe that passkey adoption is a virtuous cycle, and transitioning the world away from passwords is bigger than any one company. As more relying parties prioritize passkey support, passkeys will first become recognized, then familiar, then expected—everywhere you sign in. As people become increasingly familiar with the usability and security benefits of passkeys, they’ll be more likely to enroll and use them on more sites. Together, we can convince billions and billions of users to enroll passkeys for trillions of accounts! We’re proud to be part of this collective effort and hope you will share learnings as well as you progress in your passkey journey.

## Learn more

To learn more about Microsoft Security solutions, visit our website. Bookmark the Security blog to keep up with our expert coverage on security matters. Also, follow us on LinkedIn (Microsoft Security) and X (@MSFTSecurity) for the latest news and updates on cybersecurity.

* * *

1Microsoft Digital Defense Report 2024.

The post Convincing a billion users to love passkeys: UX design insights from Microsoft to boost adoption and security appeared first on Microsoft Security Blog.

Go to Source
