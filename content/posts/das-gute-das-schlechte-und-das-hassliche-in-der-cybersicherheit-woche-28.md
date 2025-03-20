---
title: "Das Gute, das Schlechte und das Hässliche in der Cybersicherheit – Woche 28"
date: 2025-01-06
---

Researchers crack macOS ransomware encryption, USBs pose increased risk to critical infrastructure, and 15bn stolen credentials are for sale on the DarkNet.

The post Das Gute, das Schlechte und das Hässliche in der Cybersicherheit – Woche 28 appeared first on SentinelOne DE.

## **Das Gute**

In dieser Woche wurde ein großer Business Email Compromise\-Betrugsversuch abgewehrt, der sich gegen Office 365 richtete. BEC oder Email Account Compromises waren im vergangenen Jahr für den größten Anteil der Datenverluste durch Cyberkriminalität verantwortlich. Die Betrüger nutzten die COVID-19-Pandemie als Köder und leiteten den Phishing-Datenverkehr über sechs Internetdomänen, wobei sie über schädliche Webanwendungen an die Anmeldedaten der Office 365-Konten ihrer Opfer gelangten.

Die Nutzung von Webanwendungen ist neu. Anstatt auf eine geklonte und gefälschte Anmeldeseite zu setzen, forderten die Kriminellen ihre Opfer auf, der Webanwendung Zugriff auf ihre Konten zu geben. Sobald die Übernahme des Kontos erfolgreich abgeschlossen wurde, nutzten die Angreifer es für Betrugsversuche. Dabei überzeugten sie Führungskräfte in Unternehmen, Überweisungen an die Angreifer zu genehmigen.

Für die Betrugsmasche, die in mehr als 62 Ländern angewandt wurde, kamen die folgenden schädlichen Domänen zum Einsatz, die jedoch mittlerweile von Microsoft beschlagnahmt wurden.

```
officeinventorys.com
officesuitesoft.com
officehnoc.com
officesuited.com
officemtr.com
mailitdaemon.com

```

Eine weitere gute Nachricht in dieser Woche ist die von der macOS-Sicherheits-Community durchgeführte Analyse einer kombinierten Ransomware/Informationsdieb-Malware, die über eine gecrackte Software in öffentlichen Torrents verbreitet wurde. Bei der als „EvilQuest“ oder „ThiefQuest“ bezeichnete Malware hofften die Autoren augenscheinlich, das ähnlich erfolgreiche Modell aus der Windows-Welt zu kopieren, bei dem Daten unauffällig im Hintergrund gestohlen werden, während sie im Vordergrund Lösegeld für verschlüsselte Dateien verlangen.

![](https://www.sentinelone.com/wp-content/uploads/2020/07/Screenshot-2020-07-10-at-21.15.35.jpg)

SentinelLabs knackte die symmetrische Verschlüsselung der EvilQuest/ThiefQuest-Malware und veröffentlichte ein kostenloses Entschlüsselungsprogramm. Beruhigend ist auch, dass die von den Bedrohungsakteuren eingerichtete Bitcoin-Adresse zur Sammlung von Lösegeld bislang noch keine einzige Transaktion verzeichnet hat. Die Malware bereitet den Opfern jedoch weiterhin Sorgen, da die separaten Datendiebstahl- und Backdoor-Komponenten möglicherweise vertrauliche Daten gestohlen haben und immer noch aktiv sein könnten, wenn das Gerät nicht ordnungsgemäß bereinigt wurde.

## **Das Schlechte**

Wie ein in dieser Woche erschienener Bericht zeigt, hat sich die Zahl von Cyberbedrohungen, die sich per USB-Wechselmedien gegen OT-Systeme (operative Technologie) richten, in den letzten zwölf Monaten fast verdoppelt. Fast die Hälfte aller industriellen Standorte, die im Bericht untersucht wurden, hatte mindestens eine Bedrohung entdeckt, die gegen ihre Industriesteuerungsnetzwerke gerichtet war. Der Bericht betont die unveränderte Vielzahl von USB-Geräten sowie ihre Nutzung als Angriffsvektor, wobei 20 % der gemeldeten Angriffe über Wechselspeichergeräte erfolgen. Bei den Zielen waren die Angreifer am stärksten am Öffnen von Backdoors interessiert, die dauerhaften Remote-Zugriff ermöglichen und weiteren Payload übertragen.

![](https://www.sentinelone.com/wp-content/uploads/2020/07/Screenshot-2020-07-10-at-21.22.24.jpg)

Der Grund für die Zunahme USB-gestützter Bedrohungen ist nicht Malware, die versehentlich von einem zum anderen Gerät übertragen wird, heißt es in dem Bericht, sondern das Ergebnis „bewusster und gezielter“ Angriffe wie Disttrack, Duqu, Ekans, Industroyer und USBCulprit, die USB-Geräte für Attacken gegen OT-Systeme nutzen. Der Bericht dient als dringende Warnung an alle unternehmenseigenen Sicherheitsteams und als Erinnerung daran, wie wichtig die Kontrolle von Wechselmedien wie softwarebasierten USB-Geräten ist.

## **Das Hässliche**

Nach der letzten Zählung leben rund 7,8 Milliarden auf unserem kleinen Planeten. Laut einem neuen Audit des Darknets zirkulieren mindestens doppelt so viele gestohlene Anmeldedaten in Hackerforen, wobei etwa 5 Milliarden davon einzigartig sind. Der enorme Bestand an kompromittierten Daten ist das Ergebnis von mehr als 100.000 Datenschutzverstößen – eine erschreckend hohe Zahl von Sicherheitsverletzungen.

Der Bestand umfasst Anmeldeinformationen für soziale Netzwerke, Streaming, VPN und Spiele-Websites sowie Online-Banking, Finanzdienstleister und sogar Domänenadministratoren. Kriminelle, die auf das Online-Banking-Konto einer anderen Person zugreifen möchten, bezahlen beispielsweise im Darknet etwa 500 US-Dollar oder weniger. Konten von Domänenadministratoren werden je nach Konto meist zu Preisen zwischen wenigen tausend bis zu mehr als 100.000 US-Dollar versteigert.

![](https://www.sentinelone.com/wp-content/uploads/2020/07/Screenshot-2020-07-10-at-21.26.51.jpg)

Der Diebstahl von Anmeldedaten sowie Kontoübernahmen sind eine Wachstumsbrache, da Computerkriminelle massenhafte Phishing-Kampagnen mit Botnets durchführen, Malware für Anmeldedaten-Diebstahl verteilen und Techniken wie Credential Stuffing und Brute-Forcing für den Diebstahl von Kennwörtern nutzen. Wie der Bericht zeigt, sammeln und verkaufen Kriminelle jetzt Zugriff auf digitale Fingerabdruckdaten wie Cookies, IP-Adressen und Zeitzonen, sodass gestohlene Anmeldedaten missbraucht werden können, ohne dass der betroffene Service eine Warnung wegen verdächtiger Anmeldungen ausgibt. Einige Darknet-Märkte wie Genesis Market, UnderWorld Market und Tenebris sind dafür bekannt, dass sie zeitlich begrenzte Zugriffe auf kompromittierte Konten an andere Cyberkriminelle vermieten. Für diese ergeben sich verschiedenste Möglichkeiten, beispielsweise Geldwäsche, Empfang von E-Mails oder Kauf von Produkten.

Laut den Sicherheitsforschern nutzt der Durchschnittsnutzer fast 200 Online-Dienste, die alle Kennwörter erfordern. Da viele Benutzer keine Ahnung von grundlegender Kennwortsicherheit haben und viele Unternehmen beim Schließen von Datenlecks versagen, können sich die heutigen 15 Milliarden in einigen Jahren wie Kleingeld ausnehmen.

* * *

**Like this article? Follow us on LinkedIn, Twitter, YouTube or Facebook to see the content we post.**

### Read more about Cyber Security

- „EvilQuest“ Rolls Ransomware, Spyware & Data Theft Into One
- How Do Attackers Use LOLBins In Fileless Attacks?
- How a New macOS Malware Dropper Delivers VindInstaller Adware
- Ransomware – A Complex Attack Needs a Sophisticated Defense
- macOS Big Sur | 9 Big Surprises for Enterprise Security
- The CISO Side: A Certifiable Journey
- Next-Gen AV and The Challenge of Optimizing Big Data at Scale
- Email Reply Chain Attacks | What Are They & How Can You Stay Safe?

The post Das Gute, das Schlechte und das Hässliche in der Cybersicherheit – Woche 28 appeared first on SentinelOne DE.

Go to Source
