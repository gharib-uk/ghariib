---
title: "Reflecting on 15 Years of Books: A Journey Of DevOps, Organizational Wiring, and So Much More!"
date: 2025-01-25
categories: 
  - "cloud"
  - "cloudnative"
  - "devops"
  - "linux"
---

Last week, I had the delightful experience of reviewing the rough cuts of _The Phoenix Project Graphic Novel_, Volume 2. It resurrected so many memories from working on the original book nearly 15 years ago. (The first working drafts that George Spafford and I worked on together were from 2005, so that’s 20 years ago!)

In Volume 2 of the graphic novel, which comes out later this year, there are many scenes with Bill and his wife and kids. These scenes were originally written around 2011, right after I left Tripwire. Seeing those blissful scenes of Bill and his young children brings back so many memories!

In this post, I want to share some reflections on the books I’ve worked on since _The Phoenix Project_ came out, some surprises I had while rereading them, and how my understanding of the problems has changed after writing Wiring the Winning Organization with Dr. Dr. Steven J. Spear.

## The Phoenix Project (2013)

I’m astounded that the challenges depicted in _The Phoenix Project_ remain just as relevant today. The disconnects between IT and business objectives, the core, chronic conflict between Dev and Ops, the chaos of unplanned work, the struggle to maintain stability while delivering value, and so many more… These remain problems that almost every organization faces.

Here are some reactions I had:

### The Tightly Coupled Nature of the Parts Unlimited Systems

Rereading _The Phoenix Project_, I’m struck by how it highlights the dangers of tightly coupled systems. The story begins with a catastrophic payroll failure triggered by a seemingly innocent security change in the HR employee management system. This is a fantastic example of how small changes in one system can impact distant areas of the system, sometimes with devastating effects.

We see this pattern repeat throughout the book: failures in the Phoenix order entry system cascade into store point-of-sale outages and, in the climactic scenes before Bill threatens to quit, a complete meltdown across invoicing, accounts payable, and inventory management systems. I found myself laughing out loud at the plight of Steve, the CEO, who can no longer give accurate numbers of the most important business health metrics to the board: revenue, cash position, receivables, inventory, etc.  

Because all of the relevant systems of records for those numbers are down.

Not much has changed here. Just recently, a friend told me how her corporate email account was accidentally deleted because she got married but wanted to keep her maiden name professionally. The tight coupling between HR and email systems didn’t handle this case perfectly, and all her work emails were bouncing. 

These kinds of stories reinforce how deeply intertwined our systems often are and why modularity and loose coupling remain such critical architectural principles. These create independence of action for the relevant teams and contain the blast radius of changes so that changes to one area of the system cannot adversely affect another.

### The Need for Linearization in Software Delivery

Of course, not everything can be modularized. Software delivery often requires coordinated work across different functional specialties, such as development, operations, testing, and infosec teams. The Phoenix Project rollout exemplifies what happens when this coordination fails—Bill and the Ops teams learn about all their required deliverables only nine days before launch, setting up the catastrophic launch.

In _Wiring the Winning Organization_, we give a more precise language for this: it was a sequential and interdependent process where the many steps required from many different functional silos to generate the desired output weren’t properly understood or organized. This left everyone in a reactive and firefighting mode. We defined the four critical requirements for successfully linearizing any process, whether it’s an assembly line or software delivery, as such:

- **Sequentialized**: The dependencies and paths through each work center need to be known from start to finish, ideally with dedicated resources, rather than being coupled to shared services (which can often lead to long wait times).

- **Standardized**: Each operation along those paths needs clear, documented (and preferably automated) steps.

- **Stabilized**: Mechanisms must be in place to detect and prevent errors from flowing downstream.

- **Synchronized**: When the above three elements are in place, the process becomes naturally synchronized, eliminating the need for centralized scheduling and expediting.

These properties are important and applicable to drug development, engine design, and software delivery.

### Bill as a Sociotechnical Maestro and Leader

Dr. Ron Westrum introduced me to the term “sociotechnical maestro“—someone who has these five characteristics: high energy, high standards, great in the large, great in the small, and loves walking the floor.

Bill embodies these traits. Throughout the book, he plays the role of the unwilling leader, constantly asked to lead, despite his desire to “stay in the quiet backwaters of the company.” He is plunged into crises, in the beginning without anyone knowing he’s been appointed to be in charge, and constantly goes to where work is performed to understand what is going on. He honors reality and is a consistently principled leader.

I loved following his journey as he discovered the isomorphic relationship between manufacturing plant floor operations and software delivery. In 2004, my coauthor George Spafford and I both took two graduate-level Theory of Constraints classes that gave us confidence in these claims. Fifteen years later, my work with Dr. Dr. Steven J. Spear completed the validation—the responsibility of the Layer 3 organizational wiring of an organization is the same, regardless of whether it’s performing manufacturing operations or software delivery.

Bill’s education comes through direct observation. At the MRP-8 plant, Erik helps him see how manufacturing principles directly apply to software delivery. He learns that changes waiting to be implemented behave exactly like inventory on the plant floor—they pile up, create bottlenecks, and impact the entire system’s flow. 

The changes were physically manifested as change cards in the change control room. I love Wes’ line about being the “Bates Motel of changes—changes come in, but they never leave,” showing again the dangers of shared services organization, and how easily they can become a bottleneck.

I think what makes Bill particularly effective is his commitment to scientific thinking. He engages in theory building and testing, carefully observing whether new policies and rules actually improve system performance. While Patty does much of the hands-on experimentation, it’s Bill’s constant encouragement and leadership that creates the environment for this learning to happen.

## The DevOps Handbook (2016)

Originally intended for release alongside _The Phoenix Project_ in 2013, The DevOps Handbook took an additional three years to complete. In some ways, that delay was a real blessing, because there were nearly fifty real-world case studies in the book, many drawn from the DevOps Enterprise Summit conferences I’ve been running since 2014 (recently renamed the Enterprise Technology Leadership Summit).

I’m delighted that, like _The Phoenix Project_, _The DevOps Handbook_ seems just as relevant now as it did when it was first released. In fact, when the second edition was released in 2022 (which was completed with the fantastic work of Dr. Nicole Forsgren), I was so delighted that most of the changes were additions—only a few lines had to be rewritten because they seemed too dated.

The principles described in the book have not changed (the three ways of DevOps: flow, feedback, and continuous learning), and the discussions of software architecture, technical practices, and cultural norms are just as relevant now as ever. And with the advent of GenAI, there are now even more functional specialties that need to be integrated into our technology value stream—hooray!

## Accelerate (2018)

From 2013 to 2019, I had the privilege of working with Dr. Nicole Forsgren and Jez Humble on the State of DevOps Research. This remains one of my proudest professional achievements. It was a cross-population study spanning over 36,000 respondents that revealed what high-performing technology organizations looked like and what technical practices, architectural approaches, and cultural norms enabled their success.

The _Accelerate_ book, with Dr. Forsgren as lead author, served two purposes: documenting the rigorous survey design and analysis behind our research and providing leaders with a practical guide to the factors that drive high performance. It occupies a unique space between academic research and a prescriptive manual that I haven’t seen replicated elsewhere.

One of my favorite stories from the early days of this research stems from a spirited debate between Jez and me in 2013 before Dr. Forsgren joined the team. We had just discovered distinct clusters of high, medium, and low-performing organizations, but found ourselves disagreeing on a fundamental question: Who should perform production deployments?

Jez, with his continuous delivery background, represented “Team Development.” He argued that developers, understanding the code’s intricacies, were best positioned to make deployment decisions. I was on “Team Operations,” influenced by my work in ITIL in the early 2000s. I believed that only Ops people understood the intricacies of the environment and were, therefore, best suited to make deployment decisions.

We added a survey question asking who performs production deployments—Development, Operations, or Other—expecting to find that one group would show lower change failure rates.

The results surprised us both. It didn’t matter who pressed the deploy button. Change failure rates showed no meaningful difference between developer-led or operations-led deployments. Instead, the data revealed that technical practices were the real differentiator. What actually mattered were the following practices:

- Version control for all artifacts (code and configurations)

- Automated testing to validate changes

- Automated environment creation

- Standardized deployment automation

Better outcomes did not depend on who performed the deployment but on the presence of technical practices that enabled deployments to be safe.

## The Unicorn Project (2019)

The Unicorn Project allowed me to revisit Parts Unlimited from a different angle—through the eyes of Maxine, our senior developer protagonist. Writing it allowed me to explore themes that had become increasingly important since the original release of _The Phoenix Project_: the importance of loosely coupled architectures that enable independence of action, the need for developer productivity, and the importance of platforms (which is another form of modularity).

When reviewing an early draft, Dr. Mik Kersten described it as depicting “the absolute worst-case architecture that nearly prevents all aspects of daily work being done.” This was precisely the intent—to create a grand thought experiment showcasing how an organization could systematically impede even the most talented developers through layered technical debt, burdensome processes, and endless committees.

The story powerfully demonstrates how even a “10x engineer” like Maxine can be completely neutralized when placed in a dysfunctional sociotechnical system. Her journey illustrates that extraordinary outcomes don’t emerge from individual capability (Layer 1), but rather from effective organizational wiring (Layer 3), as we illustrate in _Wiring the Winning Organization_. 

The contrasts between high-performing and struggling teams within Parts Unlimited highlighted how architectural choices and organizational structures directly impact developer productivity and business outcomes. Writing this book was the perfect preparation for working on _Wiring the Winning Organization_.

## Wiring the Winning Organization (2023)

Working on Wiring the Winning Organization felt like the capstone of my entire career. I got to collaborate with Dr. Dr. Steven J. Spear, who wrote the most downloaded _Harvard Business Review_ article of all time, “Decoding the DNA of the Toyota Production System” (1999). Looking through my _Phoenix Project_ repository, I found the PDF in our research files. I remember reading it with _Phoenix_ coauthor Kevin Behr in 2002, and how we were both amazed by Spear’s description of Toyota as a community of scientists, where thousands of people learned and experimented daily.

_Wiring the Winning Organization_ was one of the most intellectually challenging and rewarding I’ve worked on. 

In it, we wanted to answer the question of what was common between agile and DevOps (my world) and Lean, manufacturing, engine design, and safety culture (Steve’s world), along with resilience engineering, psychological safety, and so much more. We concluded that each of these were incomplete expressions of a far greater whole.  

We identified three mechanisms that enable high performance regardless of operational domain:

1. **Slowification**: Moving our most dangerous and consequential work from production to planning and practice.

3. **Simplification**: Breaking down large, complex, intertwined problems into simpler, smaller, more manageable pieces through incrementalization, modularization, and linearization.

5. **Amplification**: Creating the conditions where problems can be seen and solved quickly.

What fascinates me is how concepts from my previous books can be expressed by these three mechanisms. For instance, Dr. Westrum’s organizational typology model exemplifies Amplification—leaders signal what matters and amplify important signals so that potential problems can be detected, corrected, and prevented.

I feel like the rest of my career will be viewing everything through the lens of what we discovered in this book.

## Coming Full Circle: _The Phoenix Project Graphic Novels_ (2024 and 2025!)

I had mentioned that reviewing the rough cuts of The Phoenix Project Graphic Novel, Volume 2 has been a delightful journey back in time. The scenes of Bill with his wife and kids, written around 2011 right after I left Tripwire, resurrect vivid memories of writing the original book. Looking at those earliest drafts from 2005 with George Spafford, I’m struck by how much has changed—and how much hasn’t.

I was telling my kids recently that _The Phoenix Project_ is a book I’m not sure I could write again. What amazes me is how vividly the business problems manifested—from the potential failure of the SOX audit to the impacts on operations and financial reporting to the consequences of PCI Data Security Standard non-compliance. These were problems I worked with more intensely earlier in my career, and I wonder if I could write about them as authentically today, not experiencing them daily.

The book also revealed how many things we initially had hunches about. Take the isomorphic relationship between manufacturing plant operations and software development. We had good reasons for believing in this parallel, but it wasn’t until working on _Wiring the Winning Organization_ that I knew with certainty these connections were correct and had even broader implications.

Seeing _The Phoenix Project_ adapted into a graphic novel is like how novelists must feel watching their work become a successful film. It’s remarkable to see another artist’s interpretation, especially when done so well. The attention to detail delights me—Brent’s desk area crammed with decades of dated equipment (including a PET?!?), the emotionally charged scenes between Steve and Bill that lead to his resignation, the interactions between Wes and Patty. These visual interpretations bring new dimensions to the story that I never imagined while writing it.

## A Small Surprise

I’m ridiculously excited to be working on a new book! I’ll share details of it very soon!

The post Reflecting on 15 Years of Books: A Journey Of DevOps, Organizational Wiring, and So Much More! appeared first on IT Revolution.

Go to Source
