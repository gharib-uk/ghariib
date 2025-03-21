---
title: "Securing Cryptocurrency Organizations"
date: Tue, 21 Jan 2025 14:00:00 +0000
draft: false
type: posts
---
# Securing Cryptocurrency Organizations





Written by: Joshua Goddard The Rise of Crypto Heists and the Challenges in Preventing Them Cryptocurrency crime encompasses a wide range of illegal activities, from theft and hacking to fraud, money laundering, and even terrorist financing, all exploiting the unique characteristics of digital currencies. Cryptocurrency heists, specifically, refer to the large-scale theft

Written by: Joshua Goddard

* * *

The Rise of Crypto Heists and the Challenges in Preventing Them
---------------------------------------------------------------

Cryptocurrency crime encompasses a wide range of illegal activities, from theft and hacking to fraud, money laundering, and even terrorist financing, all exploiting the unique characteristics of digital currencies. Cryptocurrency heists, specifically, refer to the large-scale theft of cryptocurrencies or digital assets through unauthorized access, exploitation, or deception.

Cryptocurrency heists are on the rise due to the lucrative nature of their rewards, the challenges associated with attribution to malicious actors, and the opportunities presented by nascent familiarity with cryptocurrency and Web3 technologies among many organizations. Cofense highlighted that phishing activity targeting Web3 platforms increased by 482% in 2022, [Chainalysis reported](https://go.chainalysis.com/crypto-crime-2024.html) that $24.2 billion USD was received by illicit addresses in 2023, and [Immunefi reported](https://immunefi.com/research/) that in Q2 2024, compromises of Web3 organizations resulted in losses of approximately $572 million USD.

When threat actors gain access to cryptocurrency organizations, the potential for rapid, high-value financial losses due to unauthorized access is significantly elevated. A single malicious command executed on a vulnerable system can lead to the theft of millions of dollars worth of assets. This starkly contrasts with traditional organizations, where achieving financial gain or extracting value from stolen data often requires prolonged social engineering campaigns, all while facing the risk of detection and apprehension by law enforcement or financial institutions. The prospect of swift and substantial financial gains presents a compelling motivation for threat actors to target cryptocurrency organizations.

Cryptocurrency organizations are those whose core operations revolve around the use, management, or exchange of digital currencies, including:

-   Cryptocurrency exchanges that facilitate the buying and selling of cryptocurrencies
    
-   Financial institutions or payment gateway platforms that provide on or off ramp buying and selling of cryptocurrencies
    
-   Financial institutions that hold cryptocurrency assets as investment products for their customers
    
-   DeFi protocol providers that provide financial solutions for interacting with cryptocurrency assets
    
-   Web3 game creators that use blockchains for their in-game economics
    
-   Providers of hardware or software cryptocurrency wallets, wallet custodians, and wallet smart contract providers, which facilitate storage solutions for cryptocurrency assets 
    
-   Cryptocurrency mining organizations, which validate transactions to generate new cryptocurrency tokens
    

The threats posed to these organizations are significant. Mandiant has observed cryptocurrency organizations employing heightened security controls driven by pressures from widespread reporting of impactful heists, however, many still remain unprepared for the threats they face. 

Across its Incident Response engagements conducted at cryptocurrency organizations, Mandiant has observed common challenges relatively unique to these types of organizations. Mandiant has observed that these challenges introduce significant technical security debt, complexity, and widened attack surfaces, which make preventing, detecting, and responding to intrusions increasingly challenging.

-   **Hyperfocus on wallet infrastructure:** Many organizations focus on the security of wallet infrastructure but fall short on fundamental enterprise security practices.
    
-   **Rapid development lifecycles:** Cryptocurrency organizations, especially startups, often need to develop platforms fast, driven by aggressive market competition and investor pressure.
    
-   **Unmanaged workforces:** Given the demand for cryptocurrency platform developers, many organizations employ contractors or freelancers, who work for multiple organizations at the same time from their own devices. These devices are generally not monitored or policy-enforced by the organizations the user works for, and compromise of one of these devices may lead to intrusions at multiple organizations.
    
-   **Unmanaged or disparate infrastructure:** Given how rapidly many organizations have grown, infrastructure can be relatively chaotic, with disparate systems across multiple environments or cloud providers, with ad-hoc inventory or change management practices.
    

![Challenges](https://storage.googleapis.com/gweb-cloudblog-publish/images/securing-crypto-orgs-no-challenges-hd.max-1000x1000.png)

Mandiant has observed similarities in how threat actors were able to compromise and steal cryptocurrencies from intrusions it has investigated. In Securing Cryptocurrency Organizations, Mandiant has collated recommendations and insight from its frontline work, designed to specifically assist organizations whose core business involves interacting with cryptocurrencies. The recommendations provide an overview of security controls that cryptocurrency organizations should implement to prevent intrusions, detect them earlier in the [Attack Lifecycle](https://cloud.google.com/security/resources/insights/targeted-attack-lifecycle), and respond to them more effectively. While these recommendations are based on specific security failings observed in intrusions Mandiant has investigated, alternative frameworks outlining wider security controls for Cryptocurrency Organizations are available within the industry. Notably, the [Security Frameworks by Security Alliance (SEAL)](https://frameworks.securityalliance.org/intro/introduction.html) provides an excellent overview of key controls to consider when approaching security in a range of cryptocurrency organizations.

![Prevention, detection, response](https://storage.googleapis.com/gweb-cloudblog-publish/images/security-crypto-orgs-1-2-3.max-1000x1000.png)

Prevention
----------

### Securing Cryptocurrency and IT Infrastructure to Prevent Compromise

#### Cryptocurrency Infrastructure and Wallet Management

Cryptocurrency infrastructure is the network of systems, protocols, and technologies that facilitate the creation, storage, transfer, and management of digital assets. At the core of this infrastructure are cryptocurrency wallets, which serve as containers for storing and managing cryptographic keys that provide ownership and control over cryptocurrencies. The highest priority for every cryptocurrency organization is the security of this infrastructure, since compromise of these keys can lead to significant financial loss.

##### Standards for Crypto Infrastructure

Traditional cryptographic security controls are commonplace in most organizations' security strategy. Many of the same controls and use cases can be applied to the protection of cryptocurrency keys and wallets.

The [CryptoCurrency Security Standard (CCSS)](https://cryptoconsortium.org/standards-2/) from the CryptoCurrency Certification Consortium (C4) is a widely accepted standard for the technical security of systems that store or interact with cryptocurrencies. While not formally required by any financial regulator in any market, the controls it introduces are a strong starting point for organizations looking to secure their cryptocurrency infrastructure, specifically, and complement other local and international IT security standards. Mandiant recommends that organizations maintaining cryptocurrency infrastructure should adhere to these standards, both on custom implementations and with third-party solutions.

##### Custodian Solutions

Many cryptocurrency organizations choose to use commercial cryptocurrency custodian solutions to manage their wallet infrastructure. These solutions offer several advantages, especially for green-field or start-up organizations that may lack the skills, budget, or requirements to build or commission custom systems. The primary advantages of custodian solutions include simplified authentication and permissions management, streamlined logging and monitoring, and the provision of API access for easy integration with other systems.

Commercial custodian platforms can introduce new risks over custom-built solutions, especially when those platforms are not self-hosted. These include the potential compromise of the custodian platform itself, vulnerabilities in platform dependencies or content delivery networks (CDNs), and reliance on internal vendor security testing and monitoring for closed-source commercial platforms. Organizations should implement and validate strict controls around any custodian solution.

-   **Perform internal security testing:** When using self-hosted solutions, organizations should conduct internal security testing for every new implementation or significant change. This may be performed internally or by using multiple third-party vendors. This helps identify misconfigurations and vulnerabilities and provides extra assurance above and beyond the testing performed by the custodian vendor.
    
-   **Implement strict access controls:** Implement strict authentication and access controls, including principles of least privilege and multifactor authentication. Any account with permission to withdraw cryptocurrency assets or make platform or wallet policy changes should be considered a highly privileged account.
    
-   **Implement strict scopes:** Implement limits on spending and access at user, wallet, and API key levels. Implement transaction allow lists for destination send addresses and regularly audit the scopes.
    
-   **Manage credentials:** Protect API keys and credentials by using just-in-time keys, avoiding local storage, and allowing developer access to only developer custodian instances or environments. Integrate the credentials for custodian solutions into privileged access and identity management solutions to enhance auditing and management of credentials.
    
-   **Secure access:** When using on-premises or cloud-hosted custodian web platforms, ensure that users visit the platform using saved bookmarks, and always verify the website they are visiting is legitimate before transacting.
    
-   **Limit transaction types:** When managing or interacting with smart contracts through a custodian platform, the platform should support limiting which functions can be called. This would typically include only allowing token transfers instead of risky operations like smart contract updates. The exact functions callable by the custodian will depend on the smart contract deployed.
    
-   **Configure multi-signature (multi-sig) requirements:** If the platform is managing multi-sig smart wallets, ensure that it is one of the required signatures and it only provides that signature if the transaction meets all of the configured criteria, including destination addresses and called functions. The custodian must have context around the exact functions being called on any smart contract to configure these criteria.
    

#### Smart Contracts and Multi-Sig Wallets

Organizations should employ smart contracts for cold wallets that necessitate multiple hardware signatures to authorize significant send transactions. While the specific capabilities of these contracts can vary across blockchains, Mandiant recommends that organizations should consider several key controls.

-   **Use audited contract code:** For multi-sig wallets, opt for open-source, publicly audited contract code without any modifications. Where custom contracts are required, their code should be audited and tested by experts familiar with the blockchain and coding language used.
-   **Configure signature requirements:** Enforce a minimum requirement of at least four signatures to authorize any send transaction. These signatures should originate from clear-signing capable hardware wallet devices of multiple vendors assigned to and in the possession of different trusted employees at the organization. A strict process around signing transactions should be implemented (see Management and Protection of Cold Wallets and Their Transactions). Additionally, ensure the passphrases or recovery seeds for these devices are stored in secure geographically dispersed locations to mitigate the risk of a single point of failure or physical compromise.
-   **Secure private keys and contract upgrades:** As with standard wallets, private keys for smart contracts that allow the upgrading of contract code or implementation contracts should be secured on cold devices. Multiple keys or signatures should be configured for any contract upgrade.
-   **Distribute assets across multiple wallets:** Distribute assets across multiple multi-sig wallets with multiple cold signing wallets to a level where loss of assets from one multi-sig wallet would not be significant.
-   **Enforce a strict multi-sig signing process**: Implement and enforce a multi-sig signing process that requires signers to be in geographically dispersed locations with different internet connections, and enforce the use of signing systems only for any multi-sig transaction. Signing systems should be audited to confirm they meet a minimum set of security requirements prior to being used for signing. Signers must also verify the transaction hashes for any signed transaction, on-device if using cold wallets, and verify raw transaction data in a third-party system prior to signing.
-   **Scrutinize transactions where signing errors occur:** Wherever signing on a multi-sig transaction fails, implement a process to scrutinize the transaction and communicate the concern to all signers to prevent further signing activity. In cases of multi-sig compromise, Mandiant has frequently observed victims reporting multiple errors in the signing process before malicious transactions are successfully approved.

**Case Study: Abusing Smart Contracts**

The March 2023 Euler Finance exploit, resulting in a $200 million USD loss, exposed the dangers of flash loan manipulation within DeFi protocols.  Attackers exploited vulnerabilities in Euler's code to manipulate the protocol's lending and collateralization mechanisms, allowing them to obtain and drain massive amounts of cryptocurrency through a series of flash loans. This incident highlighted the need for secure coding practices and thorough testing, especially when dealing with complex financial instruments like flash loans. It highlighted the importance of anticipating and mitigating potential attack vectors, including those that exploit the unique characteristics of DeFi protocols and smart contracts.

#### Management and Protection of Cold Wallets and Their Transactions

The standard practice for cryptocurrency organizations is to store most of their assets in multi-sig smart wallets that require cold wallets for signing send transactions. Hot wallets are topped up from these multi-sig wallets as needed when they are near depletion. Since the majority of cryptocurrency organizations’ assets are stored on these cold wallets, their security is important.

##### Cold Wallet Security

Cold wallets may hold private keys for cryptocurrency wallets directly or may be used for signing transactions in a multi-sig smart wallet setup. Security of these wallets is therefore critical in preventing unauthorized transactions or contract upgrades.

-   **Use cold wallet devices from multiple vendors:** Use cold signing wallet devices from multiple vendors to mitigate risks associated with vulnerabilities in specific vendor platforms or hardware.
-   **Use cold wallet devices that support clear signing:** Ensure all signing devices support clear signing of transactions, allowing signers to accurately match the transaction hash and payload to the intended transaction. Blind signing introduces significant risks of transaction data interception between IT systems and the signing device.
-   **Store passphrases and recovery codes securely offline:** Ensure that hard copies of passphrases or backup codes for wallets are stored securely. In multi-sig wallets, these should be stored in separate secure geographical locations.

##### Dedicated Signing Systems

Mandiant recommends using dedicated, hardened signing systems for transfer or contract update transactions on multi-sig smart wallets.

-   **Enforce dedicated use:** The signing systems should be solely used for signing transactions and no other business activities.
-   **Apply security updates frequently:** The signing systems and any wallet software must be kept up-to-date with security patches. Patches should be applied much more frequently than for other systems. Verified updates should be checked and applied before signing as part of the signing process.
-   **Use separate internet connections:** The signing systems should connect to the internet from separate internet lines (mobile hotspots or locations), not through corporate exit nodes.
-   **Limit execution of applications:** The signing systems should have extremely strict controls around program execution. Typically, only a managed web browser, security software, hardware wallet software, and update software should be allowed.
-   **Limit network traffic:** Network traffic should only be allowed to specified destination addresses in line with the individual signing process, such as any cryptocurrency custodian platforms and their dependent systems or only to self-managed infrastructure. Updates may be fetched from internal staging servers or introduced through removable media after verification.
-   **Verify applications:** Signers should use the latest verified version of official hardware vendor software. This should generally be used in place of web USB solutions to mitigate the risk of compromise of third-party libraries, CDNs, or the platforms themselves.

**Case Study: Risks of Blind Signing**

In one case Mandiant investigated, a cryptocurrency exchange inadvertently signed a malicious transaction due to their hardware wallet devices not supporting clear-signing. The signers had no way to verify the transaction, which led to significant financial loss.

#### General Infrastructure Security

Cryptocurrency organizations, like any other technology-dependent organization, should adhere to best practices in enterprise security. Organizations in this industry, as in the wider financial services industry, are expected to have a higher level of capability or maturity than other industries due to the specific risks and targeting they face.

-   **Manage third-party technologies:** The security of technologies and dependencies of custodian and web platforms are of particular importance. The security of the upstream suppliers of these technologies has a direct impact on the security of the organization using them. It is critical for cryptocurrency organizations to effectively manage, monitor, and enforce security within their supply chain. Where third-party technologies are implemented, regular audits of their security should be conducted to verify the integrity of any subsystems (scripts, libraries) against known-good distributions. For web services, the content hosted on third-party servers or provided through content delivery networks should also be regularly audited.
-   **Manage vulnerabilities:** Regularly perform vulnerability scans against deployed technologies to ensure public vulnerabilities are identified, prioritized, and mitigated effectively. Implementing a bug bounty program that rewards reporters is also advisable.
-   **Manage misconfigurations:** Implement an Attack Surface Management solution to continually audit the configuration of external infrastructure, and compare the results to change management records.
-   **Validate security controls:** Implement a continuous security validation program to verify the effectiveness of prevention and detection controls as infrastructure changes.
-   **Manage identities and access:** Implement hardware-token multifactor authentication and manage permissions and identities across all production and development environments.
-   **Manage remote access:** Where remote access to environments is required, ensure that authentication is hardened and access is restricted to only managed and hardened devices through host integrity checking and device certificate-based authentication.
-   **Provide employee awareness training:** All employees of cryptocurrency organizations should be aware of the significant security threats their organization faces. Signers and developers should have an even greater understanding. Regular training or communications on the threats and how to manage them should be conducted, especially related to common tactics attackers use to compromise victims, including unsolicited job offers or freelance development work.
-   **Conduct regular targeted threat hunting activities:** Threat hunting can identify evidence of compromise that has been missed by existing detection capability, identify gaps in visibility, and assist in the development of new use cases for detection engineering. Mandiant generally recommends engaging a third party for such activities to bring new hunting hypotheses and intelligence to those already proposed by internal security teams.

#### Secrets Management

Effective and secure management of secrets within cryptocurrency organizations is critical, even more so than for organizations in other industries. The theft of secrets used to secure wallets in particular, directly or indirectly, can lead to immediate and significant financial loss.

-   **Rotate secrets:** Rotate secrets related to critical wallets and wallet infrastructure on a frequent basis, ideally monthly. This reduction of lifespan in secrets limits the potential impact of an attacker gaining access to them. Frequent secret rotation can introduce its own set of challenges, including the secure distribution and communication of new secrets across systems and personnel. Implementing a robust privileged access management (PAM) or secrets management system can make this easier. A PAM solution can streamline and secure the process of secret rotation, reducing the risk of exposure or compromise.
-   **Manage secret storage:** Organizations should consider implementing an enterprise password or secrets storage and management solution with enhanced monitoring and regular auditing. Privileged secrets should never be stored locally on user systems. Regular auditing of systems used by privileged users, such as developers or signers, should be performed to identify any improperly stored secrets.

#### Securing Developer Workstations

Developers and their workstations are particularly attractive targets for cyberattacks, especially when they interact with cryptocurrency infrastructure. Mandiant has frequently observed initial access in cryptocurrency organization intrusions being through developer workstations. 

-   **Restrict direct access:** Minimize or eliminate direct access to production cryptocurrency infrastructure from developer workstations. Instead, implement secure deployment pipelines and staging environments for testing and code integration.
-   **Manage secrets:** Employ robust secret management solutions to protect sensitive keys and credentials. Avoid storing secrets directly on developer workstations or within code repositories.
-   **Enforce principles of least privilege:** Enforce the principle of least privilege, granting developers only the minimum necessary access required to perform their tasks.
-   **Regularly conduct security audits:** Conduct regular security audits of developer workstations and their access privileges to identify and address potential vulnerabilities.
-   **Conduct security awareness training:** Provide comprehensive security awareness training to developers, emphasizing the importance of protecting sensitive information and recognizing potential threats.
-   **Enforce and manage security software:** Endpoint detection and response tooling complements built-in security mechanisms and anti-virus to alert on threats quicker. Detections should be engineered for any anomalous or unauthorized behavior consistent with the end user's job role.

**Case Studies: Targeting Developers and Their Workstations**

Direct access to production cryptocurrency infrastructure from developer workstations is especially risky. Mandiant has observed threat actors leveraging footholds on developer systems to interact with such infrastructure:

-   **Targeted phishing via fake job opportunities:** State-sponsored threat actors leveraged fake job postings on LinkedIn to deliver malware to developers and their highly privileged systems, leading to privileged access to cryptocurrency infrastructure.
-   **AWS CLI abuse:** Attackers have used the AWS CLI to interact with and extract secrets from wallet infrastructure hosted in Elastic Kubernetes Service (EKS).
-   **API key compromise:** In some cases, attackers leveraged their foothold to make API requests to the cryptocurrency custodian platform using keys stored within scripts and API testing applications.

#### Protecting Customers

The security of a cryptocurrency platform's customers is an important part of a comprehensive security strategy. In any business-to-consumer organization, some level of customer account compromise is inevitable. However, cryptocurrency platforms can implement targeted controls to limit the chance of success and level of impact for such breaches.

-   **Enforce multifactor authentication for all users:** Enforce strong multifactor authentication, avoiding SMS-based tokens due to their vulnerability to SIM swapping attacks. Consider offering hardware-based tokens or app-based authenticators for enhanced security.
-   **Provide users with tools to manage their own security:** Empower users with greater control over their security by providing features like hardware multifactor authentication support, session management (including termination), login notifications, and login history visibility. This not only fosters user confidence, but also reduces the burden on the organization when investigating individual account compromises.
-   **Enforce withdrawal controls:** Implement withdrawal limits, delays, or cool-down periods for large transactions, coupled with secondary verification or notifications. These measures can significantly restrict the amount of assets an attacker can transfer if an account is compromised.
-   **Back all assets 1-to-1:** Organizations holding user assets should maintain their full value in cold storage completely segregated from the core infrastructure. This ensures that in the event of a significant cryptocurrency theft caused by the organization, users can be reimbursed. Many financial regulators already mandate this practice for cryptocurrency exchanges.
-   **Provide user awareness communications:** It is the responsibility of all cryptocurrency organizations to educate their user base about threats to their assets. Regular communication and awareness campaigns can empower users to protect themselves and recognize potential threats before security incidents occur.

Detection
---------

### Maintaining Visibility and Engineering Detections to Catch Attackers Early

Most organizations can benefit from security monitoring. By monitoring activity across host, network, and application data sources, organizations can leverage threat intelligence and identify suspicious activities related to generic IT behavior and cryptocurrency transactions, wallet interactions, and privileged access. Effective security monitoring allows organizations to detect and respond to threats earlier in the Attack Lifecycle and ultimately limit their impact.

#### Monitoring User Transactions and Activity

For cryptocurrency exchanges in particular, monitoring transactions among its user base can provide insight into fraudulent activities in the early stages of a heist. Transaction monitoring should be implemented from logs generated by the cryptocurrency applications or the custodian platforms if they are used. From these log sources, detections should be engineered relative to normal business processes and customer activity.

-   **High-volume or -velocity transactions:** Sudden surges or unusually high frequencies of withdrawals might indicate individual user accounts have been compromised or that an attacker is attempting to trigger hot wallet replenishment from cold storage. If user account compromise is suspected, organizations should investigate commonalities among affected users, particularly their platform access points, to implement targeted blocks, heightened monitoring, and trigger account credential resets. If potential attacker-induced top-up activity is detected, organizations should exercise extreme caution before initiating any replenishment procedures from cold wallets.
-   **Unusual transaction patterns:** Deposits and withdrawals involving unusual or unexpected wallets should be flagged and scrutinized. A wallet might be defined as unusual if it is linked to previous malicious behavior internally or from threat intelligence sources. Transaction behavior may also be monitored, such as typical times of day for each user and normalized frequencies of transactions. Advanced user behavior profiles may be built around this data and engineered into detections, such as identifying which users rarely make withdrawals and which users trade frequently. These detections should be integrated with transaction delay or cool-down controls.
-   **Anomalous behavior:** Detect deviations from typical user behavior, such as login attempts from unusual IP addresses or geolocations. Wherever possible, platform application security logs should be linked with transaction logs to provide context into user cryptocurrency activity.

#### Monitoring Internal User and Wallet Interaction

Monitoring internal user activity within a cryptocurrency organization is arguably more important than monitoring customer activity. Effective internal monitoring can identify compromised internal accounts and secrets early on and significantly reduce the impact of a compromise.

-   **Secrets usage:** Monitor and audit the use of API keys and other credentials when interacting with internal systems, particularly custodian platforms and wallet infrastructure. Use context from secrets management platforms or records to identify suspicious activities, such as unusual source systems, failed access attempts, or unusual API calls (especially those that might indicate reconnaissance, for example, checking policies within custodian solutions).
-   **User activity:** Monitor the specific actions performed with each request against wallet infrastructure systems. This includes monitoring the creation, modification, updating, or deletion of information or configurations. Attackers typically conduct internal reconnaissance before executing a heist. This involves actions like listing wallets, retrieving balances, or checking configured limits and allow lists through custodian platforms. Monitoring these activities provides valuable opportunities for early detection.
-   **Access to knowledge and code repositories:** Implement detections for unusual access to knowledge bases and code repositories., for example, high-velocity or high-volume requests, which might indicate bulk downloading, or searches for keywords such as "passwords", "keys", or "infrastructure." Mandiant has frequently observed threat actors performing internal reconnaissance through knowledge and code repositories prior to performing a heist.

#### Heightened Monitoring on Developer and Signing Systems

No matter how many security controls are implemented on developer and signing systems, there is still a chance they can be compromised. For this reason, heightened monitoring should be configured on these and other sensitive systems, including any that interact with wallet infrastructure.

-   **Monitor host-based activity:** Monitor process creation and software installations to promptly detect any compromise and verify the effectiveness of preventative security controls. For signing systems, deploy detections to alert on any software not associated with approved hardware wallet vendors or managed web browsers.
-   **Monitor network activity:** Engineer network detections tailored to the permitted use cases of each system. On signing systems, implement strict alerts for any network calls to destinations beyond those necessary for software updates and transaction signing, even if these should be blocked at the network level.
-   **Perform threat hunting:** Conduct regular threat hunting and auditing on developer and signing systems to ensure secrets are not stored on disk and that security controls remain effective.

Response
--------

### Taking Action to Thoroughly Investigate and Remediate Compromise

When compromise is detected in a cryptocurrency organization, it is essential to tactically harden the environment, investigate to identify root cause, and perform remediation swiftly once the attack path is understood.

#### Tactical Hardening and Positioning

When compromise is identified within any part of a cryptocurrency organization, time is of the essence to harden the environment as much as possible and configure security technologies to give organizations the best chance of thoroughly investigating, effectively remediating, and securing the environment going forward.

-   **Enhance logging:** If the attacker is still active within the environment, immediately increase logging verbosity for critical areas such as secret use against custodian platforms, wallet infrastructure, and identity providers. This enhanced visibility helps understand attacker actions and enables real-time tactical responses.
-   **Isolate wallet infrastructure:** At the first sign of access or malicious attempted access to wallet infrastructure, including hot wallets or secrets, temporarily isolate the affected systems or custodian platforms. This containment measure prevents further attacker access and potential asset loss. Such actions should be linked to agreed processes for customer or stakeholder communication, if necessary.
-   **Implement restrictions on withdrawals:** Where an attacker may be active or have opportunities to complete their mission, implement cool-down periods or other restrictions on withdrawals. This should include limiting withdrawals to new addresses or introducing limits on the volume or velocity of withdrawals.
-   **Rotate wallet keys:** If there are any suspicions that the attacker possesses wallet keys or passphrases, rotate them immediately or transfer funds to a new, secure wallet as quickly as possible. For multi-sig wallets, rotate all signers to invalidate any compromised keys, and issue new hardware devices if necessary.
-   **Isolate compromised systems:** Immediately disconnect any systems exhibiting signs of malware or attacker access, especially those involved in signing transactions or interacting with wallet infrastructure services. Preserve these systems for forensic investigation.
-   **Carefully plan remediation:** Generally, avoid widespread remediation actions like enterprise-wide password resets during the initial response phase. Without a complete understanding of the attacker's access, such actions could alert them, prompting them to accelerate their malicious activities and target accessible wallets. While caution is advised against widespread remediation actions during the initial response phase of a typical compromise, the unique nature of cryptocurrency heists demands a more nuanced approach. Unlike other breaches where organizations might be able to monitor attacker activity, the theft of a single file or key in a cryptocurrency environment can lead to significant financial loss, so a different approach may be needed. 
-   **Engage incident responders:** Organizations should engage an incident response service provider immediately to begin scoping and investigating the intrusion and advising on the best tactical options to take at each phase of the investigation.

#### Investigating

Investigation of compromises within cryptocurrency organizations varies significantly between intrusion types, initial information identified from detection, and infrastructure architecture. However, in the majority of cases Mandiant has investigated, similar tactics were observed, which can provide a starting point for investigators.

-   **Malware on developer or signing systems:** Analyze systems that interact with wallet or custodian infrastructure for signs of malware, including developer and signing systems. Given their prevalence in cryptocurrency organizations, macOS systems are particularly attractive targets for cryptocurrency threat actors. Both built-in and third-party security mechanism logs and artifacts should be carefully reviewed.
-   **Initial access by phishing:** Investigate opportunities threat actors may have had to introduce malware or steal credentials in the environment. Mandiant frequently observes threat actors targeting developers with deceptive job offers containing malicious coding or debugging challenges, most frequently delivered via Slack, Telegram, or LinkedIn.
-   **Malicious webpages:** Analyze web history on hosts and the network perimeter to uncover any visits to malicious websites, especially by signers in the case of a multi-sig heist. Be vigilant for websites mimicking custodian platforms or wallet management applications that trick users into connecting hardware wallets and inadvertently signing malicious transactions.
-   **Malicious transaction requests:** Review available logs that may have recorded interactions with wallet infrastructure, especially send transactions. These are typically available within the custodian platform, if deployed, or in API gateways.

#### On-Chain Analysis

Investigation can also be performed on the blockchain and can provide valuable information for attribution of a threat actor and opportunities for recovery of funds. 

-   **Follow the money:** Trace the movement of funds through the blockchain to identify the origin, destination, and intermediaries involved in transactions relating to stolen funds.
-   **Identify key entities:** Analyze transaction patterns and metadata to uncover addresses and entities associated with the movement of funds.
-   **Use blockchain explorers:** Leverage blockchain explorers to gather information on addresses, transactions, and associated metadata.
-   **Cluster analysis:** Group addresses controlled by the same entity based on transaction patterns and behaviors.
-   **Consult with experts:** Engage with blockchain analytics experts or law enforcement agencies for advanced tracing and identification techniques.
-   **On-chain adversary tactics, techniques, and procedures (TTPs)**: TTPs also apply post-intrusion, where an adversary has developed their own methodology to obscure the money flow and launder the funds, for example, using specific tokens such as Monero (XMR), which is a cryptocurrency and blockchain with enhanced privacy features, or using obfuscation protocols such as mixers like Tornado Cash.

#### Remediating and Moving Forward

Effective remediation after a thorough investigation is important in order to secure the infrastructure going forward. The precise steps an organization takes will depend on the specific architecture and the attacker's tactics, but several key actions are generally recommended by Mandiant.

-   **Rotate all secrets:** As in tactical hardening, reset all wallet secrets or move funds to newly created, secure wallets. Implement enterprise-wide password resets, revoke compromised certificates, and reset API keys for custodian platforms and any access points to wallet infrastructure.
-   **Remove footholds:** Identify and eliminate any access points the attacker may still have in the environment. If malware was deployed, rebuild affected systems from known-good configurations to ensure a clean environment.
-   **Implement system hardening:** Apply all preventive measures detailed in Prevention to enhance the overall security posture and make the environment more resistant to future attacks.
-   **Implement enhanced security monitoring:** Implement enhanced security monitoring tailored to the specific indicators of compromise and attacker methodologies identified during the investigation. This will provide a way to verify that remedial actions have been effective.
-   **Plan for effective remediation:** Organizations must strategically time their remedial actions, ensuring they align with the investigation's findings. Action should be initiated when sufficient knowledge about the attacker and their activities is gained and sufficient monitoring is in place to ensure the effectiveness of these actions and provide timely alerts if any gaps remain.

Conclusion
----------

Cryptocurrency organizations operate in a challenging threat landscape where the risk of immediate and substantial financial loss is high. The convergence of high-value digital assets, complex and rapidly evolving technologies, and the pressures of a rapidly evolving market, creates an environment where organizations should expect to be targeted. The recommendations in Securing Cryptocurrency Organizations provide a starting point for bolstering defenses, however security is not a point-in-time achievement, and organizations should aim for continuous improvement.

Mandiant’s recommendations highlight the importance of a multi-faceted enterprise security approach for every size of cryptocurrency organization. This includes the secure management of wallet infrastructure, robust access controls, and securing systems which interact with infrastructure, such as developer and signing workstations. Mandiant has also emphasized the importance of proactive threat detection to identify malicious activity earlier within wallet infrastructure, wider platform infrastructure, and on employee workstations. Finally, organizations should be prepared to respond to an intrusion with suitable data available to support an investigation, and appropriate containment and hardening controls to limit the impact.

The task of securing a cryptocurrency organization is complex. By embracing a culture of proactive security, continuous improvement, and implementing the recommendations in Securing Cryptocurrency Organizations as a baseline, Mandiant hopes that the number of successful intrusions decreases across the industry.

### Call to Action for Security Leaders

Security leaders in cryptocurrency organizations should take these steps to make the most of Securing Cryptocurrency Organizations:

-   Conduct a comprehensive security assessment which benchmarks your current security posture against industry best practices and the recommendations provided in Securing Cryptocurrency Organizations. Identify gaps in your controls and prioritize remediation efforts through risk analysis.
    
-   Develop and implement a security roadmap based on the assessment. Focus on technical and strategic improvements with clear timelines and resource allocations across prevention, detection, and response capabilities.
    
-   Strive to be the most trusted organization within your industry by fostering a security-conscious culture. Empower your organization to see security as a shared responsibility supported by your investment.
    

The future of cryptocurrency organizations depends on the security and trust within them. By proactively addressing the challenges and implementing effective security controls, cryptocurrency organizations can safeguard their assets, protect their customers, and contribute to a more secure and resilient industry.

Mandiant provides cryptocurrency organizations with a range of services including [security assessments](https://cloud.google.com/security/consulting/mandiant-technical-assurance), [compromise assessments](https://cloud.google.com/security/resources/datasheets/compromise-assessment), [threat hunts](https://cloud.google.com/security/products/mandiant-managed-threat-hunting), [security monitoring](https://cloud.google.com/security/products/managed-defense), [threat intelligence](https://cloud.google.com/security/products/threat-intelligence), and [incident response](https://cloud.google.com/security/consulting/mandiant-incident-response-services). To learn more about our experience and how we can help, [get in touch](https://cloud.google.com/contact).

Acknowledgement
---------------

Special thanks to the reviewers: Adrian Hernandez, Robert Wallace, Joseph Dobson, Mohamed El-Banna, and Jigisha Patel.

#### [Source](https://cloud.google.com/blog/topics/threat-intelligence/securing-cryptocurrency-organizations/)

