---
title: "Die 10 besten Methoden zum Schutz des Active Directory"
date: 2025-01-06
---

While protecting AD is a challenge, it is far from impossible—it just requires the right tools and tactics.

The post Die 10 besten Methoden zum Schutz des Active Directory appeared first on SentinelOne DE.

Für Cyberkriminelle stellt das Active Directory (AD) ein wertvolles Ziel dar. Sie nehmen es regelmäßig ins Visier, um ihre Berechtigungen auszuweiten und den Zugriff zu erweitern. Leider ist das AD für den Geschäftsbetrieb unverzichtbar und muss daher für alle Benutzer eines Unternehmens leicht zugänglich sein. Das erschwert Schutzmaßnahmen zusätzlich. Laut Microsoft werden täglich mehr als 95 Millionen AD-Konten angegriffen. Dies macht das Ausmaß des Problems deutlich.

Schutzmaßnahmen für das AD sind zwar eine Herausforderung, lassen sich mit den richtigen Tools und Taktiken aber durchaus bewerkstelligen. Nachfolgend finden Sie zehn Tipps, mit denen Unternehmen ihr Active Directory effektiver vor häufigen aktuellen Angriffstaktiken schützen können.![](https://de.sentinelone.com/wp-content/uploads/sites/3/2022/06/Die-10-besten-Methoden-zum-Schutz-des-Active-Directory.png)

## 1\. Erkennen und verhindern Sie die Auflistung von Berechtigungen, delegierten Administratoren, Services und Netzwerksitzungen

Sobald die Angreifer den Perimeterschutz überwunden und sich im Netzwerk festgesetzt haben, führen sie Erkundungen durch, um potenziell wertvolle Ressourcen zu identifizieren und festzustellen, wie sie an diese gelangen können. Am besten eignen sich dafür Angriffe auf das AD, weil sich diese als normale Geschäftsaktivitäten tarnen lassen und dadurch nur selten erkannt werden.

Eine Lösung, die die Auflistung von Berechtigungen, delegierter Administratoren und Dienstkonten erkennen und verhindern kann, ermöglicht es dem Sicherheitsteam, Angreifer in den ersten Phasen eines Angriffs aufzuspüren. Zudem können Angreifer mit gefälschten Domänenkonten und Anmeldedaten auf Endpunkten getäuscht und anschließend auf Köderobjekte umgeleitet werden.

## 2\. Identifizieren und korrigieren Sie gefährdete privilegierte Konten

Häufig speichern Benutzer ihre Anmeldedaten in ihren Workstations. Meist geschieht dies versehentlich, mitunter aber auch absichtlich – in der Regel aus Bequemlichkeit. Angreifer wissen das und nehmen diese gespeicherten Anmeldedaten ins Visier, um sich damit Zugriff auf die Netzwerkumgebung zu verschaffen. Mit den richtigen Anmeldedaten können sie viel Schaden anrichten. Zudem suchen sie immer nach einer Möglichkeit, ihre Zugriffsrechte auszuweiten.

Unternehmen können Angreifern den Weg ins Netzwerk erschweren, indem sie gefährdete privilegierte Konten identifizieren, Konfigurationsfehler beheben und gespeicherte Anmeldedaten, freigegebene Ordner sowie andere Schwachstellen entfernen.

## 3\. Verhindern und erkennen Sie Golden Ticket- und Silver Ticket-Angriffe

Pass-the-Ticket\-Angriffe (PTT) sind äußerst effektiv, da sie Angreifern laterale Bewegungen im Netzwerk und die Erweiterung von Berechtigungen ermöglichen. Da Kerberos ein zustandsloses Protokoll ist, lässt es sich leicht missbrauchen, sodass Angreifer innerhalb des Systems problemlos Tickets fälschen können. Golden Ticket- und Silver Ticket-Angriffe sind die gravierendste Form von PTT-Angriffen, mit denen Domänen kompromittiert und Persistenz erzielt werden soll.

Um sich davor zu schützen, müssen Unternehmen anfällige Kerberos-TGTs (Ticket Granting Tickets) und -Dienstkonten erkennen können, um Konfigurationsfehler, die PTT-Angriffe ermöglichen könnten, zu identifizieren und davor zu warnen. Zudem kann eine Lösung wie Singularity Identity die Nutzung gefälschter Tickets auf Endpunkten verhindern.

## 4\. Implementieren Sie Schutz vor Kerberoasting-, DCSync- und DCShadow-Angriffen

Mit Kerberoasting-Angriffen können Angreifer auf einfache Weise privilegierten Zugriff erlangen, während DCSync- und DCShadow-Angriffe dazu dienen, Persistenz in der Domäne eines Unternehmens zu erzielen.

Das Sicherheitsteam muss in der Lage sein, das AD kontinuierlich zu überwachen und AD-Angriffe in Echtzeit zu analysieren. Zudem muss es vor Konfigurationsfehlern gewarnt werden, die zu solchen Angriffen führen könnten. Außerdem muss eine Lösung Cyberkriminelle auf Endpunktebene davon abhalten, Konten auszukundschaften, und das Risiko für diese Angriffe somit minimieren.

## 5\. Verhindern Sie die Erfassung von Anmeldedaten aus Domänenfreigaben

Häufig nehmen Angreifer in Skripten gespeicherte Kennwörter ins Visier, die im Klartext oder in umkehrbarer Verschlüsselung vorliegen. Oder sie nutzen Gruppenrichtliniendateien aus, die sich in Domänenfreigaben wie Sysvol oder Netlogon befinden.

Eine Lösung wie Ranger AD hilft beim Auffinden dieser Kennwörter, damit das Sicherheitsteam Risiken beseitigen kann, bevor Angreifer sie ausnutzen. Zudem bieten Lösungen wie Singularity Identity Mechanismen, mit denen falsche Sysvol-Gruppenrichtlinienobjekte im Produktions-AD bereitgestellt werden können, die Angreifer täuschen und von Produktionsressourcen ablenken.

## 6\. Identifizieren Sie Konten mit verborgener privilegierter SID

Angreifer können das SID-Verlaufsattribut (Windows Sicherheits-ID) mithilfe der SID-Injektionstechnik ausnutzen und sich dadurch lateral in der AD-Umgebung bewegen sowie ihre Zugriffsrechte erweitern.

Um dies zu verhindern, müssen Sie Konten identifizieren, für die im SID-Verlaufsattribut und dazugehörigen Berichten bekannte privilegierte SID-Werten festgelegt sind.

## 7\. Erkennen Sie gefährliche Delegierungen von Zugriffsrechten für kritische Objekte

Delegierung ist eine AD-Funktion, mit der Benutzer oder Konten ein anderes Konto imitieren können. Wenn beispielsweise ein Benutzer eine Web-Anwendung aufruft, die auf einem Web-Server gehostet ist, kann die Anwendung die Anmeldedaten des Benutzers imitieren, um damit auf Ressourcen zuzugreifen, die auf einem anderen Server gehostet sind. Jeder Domänenrechner mit uneingeschränkter Delegierung kann sich mit den entsprechenden Anmeldedaten bei jedem anderen Service in der Domäne als ein Benutzer ausgeben. Leider missbrauchen Angreifer diese Funktion, um sich damit Zugang zu anderen Netzwerkbereichen zu verschaffen.

Durch die kontinuierliche Überwachung von AD-Schwachstellen und Delegierungsrisiken kann das Sicherheitsteam diese Schwachstellen identifizieren und beheben, bevor sie ausgenutzt werden können.

## 8\. Identifizieren Sie privilegierte Konten mit aktivierter Delegierung

Apropos Delegierung: Privilegierte Konten mit uneingeschränkter Delegierung können unmittelbar zu Kerberoasting- und Silver Ticket-Angriffen führen. Daher müssen Unternehmen privilegierte Konten mit aktiver Delegierung erkennen und deren Aktivitäten protokollieren können.

Mit einer umfassenden Liste privilegierter Benutzer, delegierter Administratoren und Dienstkonten kann sich das Sicherheitsteam einen Überblick über potenzielle Schwachstellen verschaffen. In diesem Fall ist Delegierung nicht automatisch schlecht, denn die Funktion wird häufig für bestimmte Abläufe benötigt. Das Sicherheitsteam kann Angreifer jedoch mittels Tools wie Singularity Identity davon abhalten, diese Konten aufzuspüren.

## 9\. Identifizieren Sie nicht privilegierte Benutzer in der AdminSDHolder-Zugriffssteuerungsliste

Die Active Directory-Domänendienste (Active Directory Domain Services, AD DS) nutzen das AdminSDHolder\-Objekt und den SDProp-Prozess (Security Descriptor Propagator, Propagierung der Verzeichnisdienst-Sicherheit), um privilegierte Benutzer und Gruppen zu schützen. Das AdminSDHolder-Objekt besitzt eine besondere Zugriffssteuerungsliste (Access Control List, ACL), die die Berechtigungen der Sicherheitsprinzipale regelt, die zu den integrierten privilegierten AD-Gruppen gehören. Um sich lateral zu bewegen, fügen Angreifer Konten zum AdminSDHolder hinzu und erhalten somit den gleichen privilegierten Zugriff wie andere geschützte Konten.

Tools wie Ranger AD schützen Unternehmen vor diesen Aktivitäten, da sie ungewöhnliche Konten in der AdminSDHolder-Zugriffssteuerungsliste erkennen und davor warnen.

## 10\. Identifizieren Sie neue Änderungen an der Standarddomänenrichtlinie oder Standarddomänencontroller-Richtlinie

Im AD nutzen Unternehmen Gruppenrichtlinien zur Verwaltung mehrerer operativer Konfigurationen. Dabei werden Sicherheitseinstellungen für die jeweilige Umgebung festgelegt und häufig auch Administratorgruppen konfiguriert. Zudem umfassen die Richtlinien Skripte zum Starten und Herunterfahren. Administratoren konfigurieren sie, um auf jeder Ebene unternehmensspezifische Sicherheitsanforderungen einzurichten, Software zu installieren sowie Datei- und Registrierungsberechtigungen festzulegen. Leider können Angreifer diese Richtlinien verändern und damit Persistenz im Netzwerk erzielen.

Wenn Änderungen an den Standardgruppenrichtlinien überwacht werden, kann das Sicherheitsteam Angreifer schnell erkennen und auf diese Weise Sicherheitsrisiken minimieren sowie privilegierten Zugriff auf das AD verhindern.

## Setzen Sie die richtigen Tools ein

Unternehmen, die die gängigen Taktiken von AD-Angriffen kennen, können sich besser davor schützen. Bei der Entwicklung von Tools wie Ranger AD und Singularity Identity haben wir viele Angriffsvektoren berücksichtigt und zudem ermittelt, wie diese am besten erkannt und abgewehrt werden können.

Mit diesen Tools können Unternehmen effektiv Schwachstellen identifizieren, böswillige Aktivitäten frühzeitig erkennen und Sicherheitszwischenfälle beheben, bevor Eindringlinge ihre Zugriffsrechte erweitern und ein kleiner Angriff zu einer weitreichenden Kompromittierung wird. Der Schutz des Active Directory ist eine Herausforderung, die sich jedoch dank moderner AD-Schutzlösungen bewältigen lässt.

The post Die 10 besten Methoden zum Schutz des Active Directory appeared first on SentinelOne DE.

Go to Source
