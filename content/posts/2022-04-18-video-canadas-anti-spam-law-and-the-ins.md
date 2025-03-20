---
title: "Video Canadas Anti-Spam Law and the installation of software"
date: 2022-04-18T10:10:00.004-03:00
draft: false
type: posts
---
# Video Canadas Anti-Spam Law and the installation of software

<br/>

<br/>
Canada’s anti-spam law is about much more than just spam. It also regulates the installation of software. Like the rest of the law, it is complicated and convoluted and has significant penalties. If you’re a software developer or an IT admin, you definitely need to know about this.

So we’re talking about Canada’s anti-spam law. The official title is much longer, and it also includes two sets of regulations to make it more complicated.

Despite the snappy title that most of us use: Canada’s Anti-Spam Law, it is about more than just spam. It has often-overlooked provisions that make it illegal to install software on another person’s computer – or cause it to be installed – without their consent.

It was clearly put into the law to go after bad stuff like malware, viruses, rootkits, trojans, malware bundled with legitimate software and botnets. But it is not just limited to malevolent software. It potentially affects a huge range of software.

So here is the general rule from Section 8 of the Act:

> 8\. (1) A person must not, in the course of a commercial activity, install or cause to be installed a computer program on any other person’s computer system or, having so installed or caused to be installed a computer program, cause an electronic message to be sent from that computer system, unless
> 
> (a) the person has obtained the express consent of the owner or an authorized user of the computer system and complies with subsection 11(5); or
> 
> (b) the person is acting in accordance with a court order.

Let’s break that down. The first part is that it has to be part of a commercial activity. I’m not sure they meant to let people off the hook if they’re doing it for fun and giggles. The “commercial activity” part is likely there so that the government can say this is justified under the federal “general trade and commerce power”.

They could have used the federal criminal law jurisdiction, but then they’d be subject to the full due process and fairness requirements of the Canadian Charter of Rights and Freedoms, and the government did not want to do that. They’d rather it be regulatory and subject to much lower scrutiny.

Then it says you can’t install a computer program on another’s computer without the express consent of the owner or authorized user of the computer. (The definition of “computer system” would include desktops, laptops, smartphones, routers and appliances.) or cause to be installed.

The express consent has to be obtained in the manner set out in the Act, and I’ll discuss that later.

It also additionally prohibits installing a computer program on another’s computer and then causing it to send electronic messages. This makes creation of botnets for sending spam extra bad.

The definition of the term “Computer Program” is taken from the Criminal Code of Canada

> “computer program” means computer data representing instructions or statements that, when executed in a computer system, causes the computer system to perform a function; (programme d’ordinateur)

In addition to defined terms, there are some key ideas and terms in the Act that are not well-understood.

It talks about “installing” a computer program, but what that is has not been defined in the legislation and the CRTC hasn’t provided any helpful guidance.

I wouldn’t think that running malware once on someone’s system for a malevolent purpose would be captured in the definition of “install”, though it likely is the criminal offence of mischief in relation to data.

What about downloading source code that is not yet compiled? Or then compiling it?

It is certainly possible to load up software and have it ready to execute without being conventionally “installed”. Does it have to be permanent? Or show up in your installed applications directory?

I don’t know.

There’s also the question of who is an owner or an authorized user of a computer system.

If it is leased, the leasing company likely owns the computer and we’ve certainly seen reports and investigations of spyware and intrusive software installed on rented and leased laptops.

My internet service provider owns my cable modem, so it’s apparently ok if they install malware on it.

For authorized users, it means any person who is authorized by the owner to use the computer system. Interestingly, it is not limited by the scope of the authorization. It seems to be binary. Either you are authorized or you are not.

There are some scenarios to think about when considering owners and authorized users.

For example, if a company pre-installs software on a device at the factory or before ownership transfers to the end customer, that company is the owner of the device and can install whatever they like on it.

Many companies issue devices like laptops and smartphones to employees. Those employers own the devices and can install any software on them.

But increasingly, employees are using devices that they own for work-related purposes, and employers may have a legitimate interest in installing mobile device management and security software on those devices. Unless there’s a clear agreement that the employer gets to do so, they may find themselves to be offside the law.

So, in short, it is prohibited to do any of these things without the express consent of the owner or authorized user:

-   (a) install a computer program of any kind;
-   (b) cause a computer program of any kind to be installed, such as hiding or bundling additional software in an installer that the owner or authorized user has installed. We sometimes see this when downloading freeware or shareware, and the installer includes other software that the user didn’t ask for;
-   (c) or cause such a program that has been installed to send electronic messages after installation.

Of course, someone who is the owner or authorized user of the particular device can put whatever software they want on the device. This only covers installation by people who are not the owner or the authorized user of the device.

There are some exceptions that people should be aware of.

It is common to install software and to have it automatically update. This is ok if the user consents to the auto updates. But that probably doesn't apply if the update results in software that does very different things compared to when it was first installed.

There are some cases where consent is deemed or implied.

CASL deems users to consent to the installation of the following list of computer programs if the user’s conduct shows it is reasonable to believe they consented to it. It is a weird list.

At the top of the list are “cookies”. To start with, anyone who knows what cookies are knows they are not computer programs. They are text files, and including them on this list tells me that the people who wrote this law may not know as much as you may hope about this subject.

It then includes HTML code. HTML is hypertext markup language. I suppose it is data that represents instructions to a computer on how to display text and other elements. I guess the next question is whether this includes the variations of HTML like XHTML? I don’t know. But if HTML is a computer program, then so are fonts and Unicode character codes.

Next it refers to “Java Scripts”. Yup. That’s what it says. We are told by industry Canada that this is meant to refer to JavaScript, which is different from a Java script. Not only could have have maybe not made such a stupid mistake, but maybe they could have been clear about whether they were referring to JavaScript run in a browser (with its attendant sandbox) or something else.

Next on the list are “operating systems”, which seems very perverse to include. The operating system is the mostly invisible layer that lies between the computer hardware and the software that runs on top of it. Changes to the operating system can have a huge impact on the security and privacy of the user, and much of it happens below the system. And there is no clarity about whether an “operating system” on this list includes the software that often comes bundled with it. When I replace the operating system on my Windows PC, I get a new version of a whole bunch of standard software that comes with it like the file explorer and notepad. It would make sense that a user who upgrades from one version of MacOS or Windows to another. But I can make an open source operating system distro that’s full of appalling stuff, in addition to the operating system.

Finally, it says any program executable only through use of another computer program for which the user has already consented to installation. Does this include macros embedded in word documents? Not sure.

It makes sense to have deemed consent situations or implied consent, but we could have used a LOT more clarity.

There are some exceptions to the general rule of getting consent, two of which are exclusively reserved to telecommunications service providers, and a final one that related to programs that exclusively correct failures in a computer system or a computer program.

This is understandable, but this would mean that a telco can install software on my computer without my knowledge or consent if it’s to upgrade their network.

So how do you get express consent. It’s like the cumbersome express consent for commercial electronic messages, but with more.

When seeking express consent, the installer has to identify

-   the reason;
-   Their full business name;
-   Their mailing address, and one of: telephone number, email address, or web address;
-   if consent is sought on behalf of another person, a statement indicating who is seeking consent and on whose behalf consent is being sought;
-   a statement that the user may withdraw consent for the computer program’s installation at any time; and
-   a clear and simple description, in general terms, of the computer program’s function and purposes.

But if an installer “knows and intends” that a computer program will cause a computer system to operate in a way its owner doesn’t reasonably expect, the installer must provide a higher level of disclosure and acknowledgement to get the user’s express consent.

This specifically includes the following functions, all of which largely make sense:

-   collecting personal information stored on the computer system;
-   interfering with the user’s control of the computer system;
-   changing or interfering with settings, preferences, or commands already installed or stored on the computer system without the user’s knowledge;
-   changing or interfering with data stored on the computer system in a way that obstructs, interrupts or interferes with lawful access to or use of that data by the user;
-   causing the computer system to communicate with another computer system, or other device, without the user’s authorization;
-   installing a computer program that may be activated by a third party without the user’s knowledge; and
-   performing any other function CASL specifies (there are none as yet).

Like the unsubscribe for commercial electronic messages, anyone who installs software that meets this higher threshold has to include an electronic address that is valid for at least one year to the user can ask the installer to remove or disable the program.

A user can make this request if she believes the installer didn’t accurately describe the “function, purpose, or impact” of the computer program when the installer requested consent to install it. If the installer gets a removal request within one year of installation, and consent was based on an inaccurate description of the program’s material elements, then the installer must assist the user in removing or disabling the program as soon as feasible – and at no cost to the user.

So how is this enforced? CASL is largely overseen by the enforcement team at the Canadian Radio-television and Telecommunications Commission.

Overall, I see them at least making more noise about their enforcement activities in the software arena than the spam arena.

In doing this work, the CRTC has some pretty gnarly enforcement tools.

First of all, they can issue “notices to produce” which are essentially similar to Criminal Code production orders except they do not require judicial authorization. These can require the recipient of the order to hand over just about any records or information, and unlike Criminal Code production orders, they can be issued without any suspicion of unlawful conduct. They can be issued just to check compliance. I should do a whole episode on these things, since they really are something else in the whole panoply of law enforcement tools.

They can also seek and obtain search warrants, which at least are overseen and have to be approved by a judge.

Before CASL, I imagine the CRTC was entirely populated by guys in suits and now they get to put on raid jackets, tactical boots and a badge.

I mentioned before that there can be some significant penalties for infractions of CASL’s software rules.

It needs to be noted that contraventions involve “administrative monetary penalties” - not a “punishment” but intended to ensure compliance. These are not fines per se and are not criminal penalties. That’s because if they were criminal or truly quasi-criminal, they’d have to follow the Charter’s much stricter standards for criminal offences.

The maximum for these administrative monetary penalties are steep. Up to $1M for an individual offender and $10M for a corporation.

The legislation sets out a bunch of factors to be considered in determining the amount of penalty, including the ability of the offender to pay.

There is a mechanism similar to a US consent decree where the offender can give an “undertaking” that halts enforcement, but likely imposes a whole bunch of conditions that will last for a while.

Officers and directors of companies need to know they may be personally liable for penalties and of course the CRTC can name and shame violators.

There is a due diligence defence, but this is a pretty high bar to reach.

We have seen at least three reported enforcement actions under the software provisions of CASL.

The first was involving two companies called Datablocks and Sunlight Media in 2018. They were found by the CRTC to be providing a service to others to inject exploits onto users’ computers through online ads. They were hit with penalties amount to $100K and $150K, respectively.

The second was in 2019 and involved a company called Orcus Technologies, which was said to be marketing a remote access trojan. They marketed it as a legitimate tool, but the CRTC concluded this was to give a veneer of respectability to a shady undertaking. They were hit with a penalty of $115K.

The most recent one, in 2020, involved a company called Notesolution Inc. doing business as OneClass. They were involved in a shady installation of a Chrome extension that collected personal information on users’ systems without their knowledge or expectation. They entered into an undertaking, and agreed to pay $100K.

I hope this has been of interest. The discussion was obviously at a pretty high level, and there is a lot that it unknown about how some of the key terms and concepts are being interpreted by the regulator.

If you have any questions or comments, please feel free to leave them below. I read them all and try to reply to them all as well. If your company needs help in this area, please reach out. My contact info is in the notes, as well.

<!-- google\_ad\_client = "pub-2534906746401214"; //728x15, created 12/29/07 google\_ad\_slot = "1518476471"; google\_ad\_width = 728; google\_ad\_height = 15; //-->

#### [Source](http://blog.privacylawyer.ca/feeds/3220600997419116460/comments/default)

<br/>
---
