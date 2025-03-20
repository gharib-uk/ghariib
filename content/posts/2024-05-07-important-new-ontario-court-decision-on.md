---
title: "Important new Ontario court decision on privilege in incident response documentation"
date: 2024-05-07T10:20:00.003-03:00
draft: false
type: posts
categories: 
- cybersecurity,incident,incident response,privilege,video
---
# Important new Ontario court decision on privilege in incident response documentation

<br/>

<br/>
  

The Ontario divisional court has just released a [decision](https://canlii.ca/t/k4bqw), [_LifeLabs LP v. Information and Privacy Commr. (Ontario)_, 2024 ONSC 2194](https://canlii.ca/t/k4bqw), that should grab the attention of Canadian lawyers who work in cyber incident response. I don’t know whether it will be appealed, but the logic of the decision is pretty sound. But I expect this isn’t over. 

In a nutshell, after a significant ransomware incident, LifeLabs was assisted by well-known cybersecurity and forensic consultants for the investigation, remediation and negotiation with the ransomware bad guys. As required by the relevant privacy laws of those provinces, they notified the privacy commissioners of British Columbia and Ontario, and the commissioners started a joint investigation. In connection with their investigation, the commissioners demanded to see the consultants’ reports and LifeLabs claimed they were privileged. 

Not surprisingly, the ransomware incident was followed by a number of class action lawsuits that were still pending at all material times. 

In June 2020, the Commissioners issued a joint decision finding that LifeLabs had provided insufficient evidence to back up the privilege claim. They were also ordered to hand over the consultants’ reports.  So LifeLabs sought judicial review of the order in the Ontario Divisional Court. The Court just released its decision, upholding the IPC’s order. I’m not sure why it took so long to get to a hearing.

According to the IPC’s decision, there were five categories of records at issue:

i.          The investigation report prepared by the cybersecurity firm hired by LifeLabs, which described how the cyberattack occurred.

ii.          The email correspondence between the cyber intelligence firm and the cyber-attackers after the discovery of the attack by LifeLabs.

iii.         An internal data analysis prepared by LifeLabs on April 28, 2020 to describe which individual health information had been affected by the breach and to notify those affected pursuant to ss. 12(1) and 12(2) of the PHIPA.

iv.        A submission from LifeLabs to the Commissioners dated May 15, 2020 in response to certain specific questions, communicated through legal counsel.

v.         The report of Kevvie Fowler, Deloitte LLP dated June 9, 2020 prepared as part of the representations by LifeLabs and submitted to the Commissioners for that purpose.

Other than the internal LifeLabs assessments, the records were created by consultants retained by LifeLabs’ lawyers. The cybersecurity firm was already engaged by LifeLabs to assess the company’s security, and it was actually them who discovered the incident. They were instructed to provide their reports on the incident to legal counsel.  

The court reviewed the IPC’s privilege decision on a standard of correctness and found that it was correct. 

Before getting into the decision, it should be noted that LifeLabs claimed “solicitor client privilege” and “litigation privilege”. They are related and similar, but not the same. 

Solicitor client privilege protects communications that are made in confidence between a lawyer and their client (or third party acting on behalf of their client). In order to be privileged, the communication must be made for the purpose of seeking or giving legal advice, and the parties must have intended the communication to be confidential. Just because there’s a lawyer in the mix doesn’t make it privileged, and a third party’s involvement, like a consultant retained by the client or the lawyer, doesn’t waive that privilege.

Litigation privilege is intended to create a “zone of privacy” within which counsel can prepare draft questions, arguments, strategies or legal theories, in anticipation of litigation and for the purpose of preparing for that litigation. Documents created by others, to assist counsel, in preparing for litigation can also fit into this category. Notably, the privilege only exists while the litigation is anticipated or ongoing.

If you read the IPC’s decision, you’ll see that not much information was provided by LifeLabs (or at least not to the IPC’s satisfaction) to demonstrate that the five categories of records fit into either solicitor client privilege or litigation privilege.  In large measure, the IPC decided that LifeLabs HAD to investigate the incident and HAD an obligation to provide factual information to the IPC. It doesn’t look like the IPC was looking for actual advice given by counsel or anything related to LifeLabs’ trial strategy for their ongoing litigation. 

Ultimately, the decision turned on LifeLabs not providing evidence to the IPC’s satisfaction to back up their privilege claims.

The main conclusions, simplified a bit, are that: 

> 1.         Facts are not privileged, even if they were collected or compiled by a lawyer.
> 
> 2.         If you have a statutory obligation to investigate and provide information to the regulator, the facts that are discovered in that investigation are not privileged.
> 
> 3.         Solicitor client privilege only protects communications that are made for the purpose of seeking or obtaining legal advice.
> 
> 4.         Litigation privilege only protects communications and records that are created for the dominant purpose of preparing for litigation.

This is not earth shattering, but it’s a reminder of how the law of privilege works in Canada. 

The court emphasized that even if certain communications or documents are privileged, the facts referred to or reflected in those communications may not be privileged if they exist independently, outside of the privileged context. Facts that have an independent existence outside of solicitor-client privileged communications are not automatically privileged.

The court quoted and agreed with paragraph 49 of the IPC’s decision:

Even if the communication is privileged, the facts referred to or reflected to in those communications are not privileged if they exist outside the documents and are relevant and otherwise subject to disclosure. Some facts have a life outside the communication between lawyer and client but have also been communicated within the solicitor-client relationship. Facts that have an independent existence outside of solicitor-client privileged communications are not privileged. When deciding if such facts are privileged, one must keep one eye on the need to protect the freedom and trust between solicitor and client and another eye on the potential use of privilege to insulate otherwise discoverable evidence. While privilege is jealously guarded it must be interpreted to protect only what it is intended to protect and nothing more.

The court further clarified that simply depositing a document or providing counsel with a copy of a document does not automatically extend privilege to the original document. The protection of privilege is intended to safeguard the communication between lawyer and client and the adversarial preparation for litigation, not the underlying facts themselves.

Therefore, the court concluded that facts concerning the investigation or remediation, even if communicated within a privileged context, may not be privileged if they have an independent existence outside of privileged documents. 

If an organization has a legal obligation to investigate, remediate and report to the privacy commissioner, interjecting lawyers into the process does not relieve the organization of its obligation to report to the commissioner. This obligation includes cooperating with the commissioner's inquiries and providing information necessary for investigations.

The Court wrote:

\[76\]           Health information custodians, such as LifeLabs, cannot defeat these responsibilities by placing facts about privacy breaches inside privileged documents. Although the claims of privilege here were rejected, even if they had been accepted, this would not have defeated the ON IPC’s duty to inquire into the facts about the data breach within the control and knowledge of LifeLabs. This result flows not only from the ON IPC’s statutory mandate, but also from how litigation privilege and solicitor client privilege function.

…

\[79\]           Thus, the IPC’s statutory duty to inquire, and LifeLabs’ duty to respond, does not permit a claim of litigation privilege over facts obtained through its lawyers, even where those facts might also play a role in defending against parallel civil litigation. As Nordheimer, J. wrote in _R. v. Assessment Direct_, at para. 10, “the privilege does not protect information that would otherwise have to be disclosed”.  LifeLabs did not identify any litigation strategy that would be disclosed in the Investigation Report because of the Privilege Decision.

On this point, the Court agreed with the findings of the IPC:

\[80\]           Similarly, solicitor-client privilege does not extend to protect facts that are required to be produced pursuant to statutory duty. The ON IPC correctly articulated the law when it stated at para. 49:

… Facts that have an independent existence outside of solicitor-client privileged communications are not privileged. … While privilege is jealously guarded it must be interpreted to protect only what it is intended to protect and nothing more.”

Furthermore, the court emphasized that organizations cannot use claims of privilege to shield facts about privacy breaches from the commissioner. Even if privilege is claimed over certain documents or information, it does not absolve the organization from its duty to cooperate with the commissioner's investigation and provide relevant facts. The court noted that placing unpalatable facts within privileged documents to avoid investigative orders would undermine the purpose of regulatory oversight and accountability.

Just saying something is privileged does not make it privileged. Including a lawyer in a conversation does not make it privileged. Having the lawyer hire the consultant does not automatically make it privileged. 

The IPC and the Court noted that the cybersecurity consulting firm had a prior retainer with LifeLabs related to what it was doing before the incident, during the incident and afterwards. Simply having the report related to the incident addressed to counsel didn’t make that report privileged. The IPC referred to a US case called _In re Capital One_, which LifeLabs said was an error. The court disagreed with LifeLabs, and reached the same conclusion as the IPC: 

\[90\]           I disagree. The _In re Capital One_ case affords persuasive authority to support a finding that where a company has a prior retainer with a cybersecurity firm to provide essentially the same services before and after a breach, inserting  counsel’s name into the contract and stating that the deliverables would be made to counsel on behalf of the client, does not render any report prepared subject to the U.S. work product doctrine, which is akin to Canada’s litigation privilege.

Interestingly, the IPC in their [March 2020 decision](https://canlii.ca/t/j64dl) on privilege left the door open for LifeLabs to prove that portions of the records may include information that is subject to solicitor client or litigation privilege. 

I would have liked to have seen a bit more analysis of what is reasonably contemplated litigation and dominant purpose, in the context of the discussion of litigation privilege. The reality is that in the aftermath of an incident like this, litigation is almost certain to follow. Much of the response or even the approach to the incident response is informed by that likelihood. Many records are created in anticipation of defending litigation, but those records are also useful for (or maybe necessary for) dealing with the commissioner’s investigation. Is 50/50 dominant enough? And some of these records would be created because that’s what’s expected of a reasonably prudent company. Is 33/33/33 dominant enough? Should we create different tracks in incident response, assigning certain investigators to the litigation track and others to the commissioner reporting track?

Maybe we should consider amending our privacy laws (or Evidence Acts more generally) to say that the provision of information to a regulator pursuant to a statutory duty does not amount to a waiver of privilege as far as third parties are concerned.

I think lawyers who work in this area will have some interesting discussions about this decision.

It will be interesting to consider how this affects certain activities that take place outside of the context of dealing with an active incident. For example, I may be retained by a client to provide them with my assessment of whether they are complying with their safeguarding obligations under privacy laws. Often, an engagement like that involves working with expert consultants who examine the network security, do penetration testing and benchmark against best practices. New facts are uncovered that will be included in my opinion and advice to the client, and at that stage there is no obligation to assist any privacy regulator in that endeavour. The new facts were “uncovered” or discovered only for the purpose of providing legal advice. I think there are arguments that can be made in both directions regarding whether those new facts can be privileged. That’s a discussion for another day …

I should add this decision doesn’t create any new law about privilege. Nor does it put a dizzying spin on privilege law, but it serves as a reminder that you can’t throw a blanket of privilege over everything associated with incident response. I also don’t think it does away with privilege in connection with incident response. I have provided a lot of advice to a lot of organizations, and I’ve worked with a lot of outside consultants in that context. I remain confident that my communications with my clients, in the context of them seeking my legal advice, is untouched by this decision. 

  

<!-- google\_ad\_client = "pub-2534906746401214"; //728x15, created 12/29/07 google\_ad\_slot = "1518476471"; google\_ad\_width = 728; google\_ad\_height = 15; //-->

#### [Source](http://blog.privacylawyer.ca/feeds/4417217815197419619/comments/default)

<br/>
---
