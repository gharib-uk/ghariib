---
title: "How Cloudflare is using automation to tackle phishing head on"
date: 2025-03-19
---

Phishing attacks have grown both in volume and in sophistication over recent years. Today’s threat isn’t just about sending out generic emails — bad actors are using advanced phishing techniques like 2 factor monster in the middle (MitM) attacks, QR codes to bypass detection rules, and using artificial intelligence (AI) to craft personalized and targeted phishing messages at scale. Industry organizations such as the Anti-Phishing Working Group (APWG) have shown that phishing incidents continue to climb year over year.

To combat both the increase in phishing attacks and the growing complexity, we have built advanced automation tooling to both detect and take action. 

In the first half of 2024, Cloudflare resolved 37% of phishing reports using automated means, and the median time to take action on hosted phishing reports was 3.4 days. In the second half of 2024, after deployment of our new tooling, we were able to expand our automated systems to resolve 78% of phishing reports with a median time to take action on hosted phishing reports of under an hour.

In this post we dig into some of the details of how we implemented these improvements.

### The phishing site problem

Cloudflare has observed a similar increase in the volume of phishing activity throughout 2023 and 2024. We receive abuse reports from anyone on the Internet that may have seen potentially abusive behaviors from websites using Cloudflare services. Our Trust & Safety investigators and engineers have been tasked with responding to these complaints, and more recently have been using the data from these reports to improve our threat intelligence, brand protection, and email security product offerings.

Cloudflare has always believed in using the vast amounts of traffic that flows through our network to improve threat detection and customer security. This has been at the core of how we protect our customers from DoS attacks and other cybersecurity threats. We've been applying the same concepts our internal teams use to mitigate phishing to improve detection of phishing on our network and our ability to detect and notify our customers about potential risks to their brand.

Prior to last year, phishing abuse reported to Cloudflare relied on manual, human review and intervention to remediate. Trust & Safety (T&S) investigators would have to look at each complaint, the allegations made by the reporter, and the content on the reported websites to make assessments as quickly as possible about whether the website was phishing or malware.

Given the growing scale of our customer base and phishing across the Internet, this became unsustainable. By collecting a group of internal experts on abuse, we were able to tackle this problem by using insights across our network, internal data from our Email Security product, external feeds from trusted sources, and years of abuse report processing data to automatically assess risk of likely phishing and recommend appropriate action.

### Turning our intelligence inward

We built our automated phishing identification on the Cloudflare Developer Platform so that we could meet our scanning demand without concern for how we might scale. This allowed us to focus more on creating a great phishing detection engine and less on the infrastructure required to meet that demand. 

Each URL submitted to our phishing detection Worker begins with an initial scan by the Cloudflare URL Scanner. The scan provides us with the rendered HTML, network requests, and attributes of the site. After scanning, we collect reputational information about the site by submitting the HTML and page resources to our in-house machine learning classifiers; meanwhile, the indicators of compromise (IOCs) are sent to our suite of threat feeds and domain categorization tools to highlight any known malicious sites or site categorizations.

Once we have all of this information collected, we expose it to a set of rules and heuristics that identify the URL as phishing or not based on how T&S investigators have traditionally responded to similar abuse reports and patterns of bad behaviors we’ve observed. Rules will suggest decisions to make against the reports, and remediations to make against harmful content. It is through this process that we were able to convert the manual reviews by T&S investigators into an automated flow of phishing identification. We also recognize that reporters make mistakes or even deliberately try to weaponize abuse processes. Our rules must therefore consider the possibility of false positives, in which reports are created against legitimate websites (intentionally or unintentionally). False positives can erode the trust of our customers and create incidents, so automation must include processes to disregard erroneous reports.

The magic of all of this was the powerful suite of tools on the Cloudflare Developer Platform. Whether it was using KV to store report summaries that could scale indefinitely or Durable Objects to keep running counters of an unlimited number of attributes that could be tracked or leveraged over time, we were able to integrate the solutions quickly allowing us easily add or remove new enrichments with little effort. We also made use of Hyperdrive to access the internal Postgres database that stores our abuse reports, Queues to manage the scanning jobs, Workers AI to run machine learning classifiers, and D1 to store detection logs for efficacy and evaluation review. To tie it all together, the team also deployed a Remix Pages UI to present all the phishing detection engine’s analysis to T&S investigators for follow-on investigations and evaluations of inconclusive results.

![](https://cf-assets.www.cloudflare.com/zkvhlag99gkb/7MQYa4u71uKm9J6AaNxQNy/0cce686f51988ece4a1a46d87dae6df9/image1.png)

_Architecture of Trust & Safety’s phishing automation detection pipeline_

### Moving forward

The same intelligence we’re gathering to expedite and refine abuse report processing isn’t just for abuse response; it’s also used to empower our customers. By analyzing patterns and trends of abusive behaviors — such as identifying common phrases used in phishing attempts, recognizing infrastructure used by malicious actors or spotting coordinated campaigns across multiple domains — we enhance the efficacy of our application security, email security, and threat intelligence products.

For our Brand Protection customers, this translates into a significant advantage: the ability to easily report suspected abuse directly from the Cloudflare dashboard. This feature ensures that potential phishing sites are addressed rapidly, minimizing the risk to your customers and brand reputation. Furthermore, the Trust and Safety team can use this information to take action on similar threats across the Cloudflare network, protecting all customers, even those who aren't Brand Protection users.

Alongside our network-wide efforts, we’ve also been partnering with our customers, as well as experts outside of Cloudflare, to understand trends they are seeing in their own phishing mitigation efforts. By soliciting intelligence regarding the abuse issues that affect the attack’s targets, we can better identify and prevent abuse of Cloudflare products. We’ve been able to use these partnerships and discussions with external organizations to craft highly targeted rules that head off emerging patterns of phishing activity. 

### It takes a village: if you see something, say something

If you believe you’ve identified phishing activity that is passing through Cloudflare’s network, please report it via our abuse reporting form. For technical users who might be interested in a programmatic way to report to us, please review our abuse reporting API documentation.

We invite all of our customers to join us in helping make the Internet safer:

1. Enterprise customers should speak with their Customer Success Manager about enabling Brand Protection, included by default for all enterprise customers. 
    
2. For existing users of the Brand Protection product, update your brand's assets, so we can better identify the legitimate websites and logos of our customers vs. possible phishing activity.
    
3. As a Cloudflare customer, make sure your abuse contact is up-to-date in the Cloudflare dashboard.
    

Go to Source
