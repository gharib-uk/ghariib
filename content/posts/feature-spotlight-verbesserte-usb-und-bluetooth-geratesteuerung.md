---
title: "Feature Spotlight – Verbesserte USB- und Bluetooth-Gerätesteuerung"
date: 2025-03-19
---

Enterprise needs full visibility and control over USB & Bluetooth devices connecting to the network. Learn all about SentinelOne's enhanced Device Control.

The post Feature Spotlight – Verbesserte USB- und Bluetooth-Gerätesteuerung appeared first on SentinelOne DE.

Im Jahr 2018 verkündeten wir die Einführung einer Gerätesteuerung auf unserer Plattform. Sie bietet Administratoren und Sicherheitsteams die Möglichkeit, die Verwendung von USB- und anderen Peripheriegeräten im gesamten Netzwerk zu verwalten. Heute können wir die neuesten Aktualisierungen für diese Funktion bekanntgeben, die nun die individuelle Verwaltung von USB-, Bluetooth- und Bluetooth Low Energy-Geräten ermöglicht. Dank der aktualisierten Gerätesteuerungsfunktion können IT- und SOC-Teams den unterbrechungsfreien Geschäftsbetrieb für alle Benutzer sicherstellen, die auf externe Geräte angewiesen sind, dabei jedoch gleichzeitig die Angriffsoberfläche auf das absolute Minimum reduzieren.

![](https://de.sentinelone.com/wp-content/uploads/sites/3/2020/12/de_Feature-Spotlight-Enhanced-USB-Bluetooth-Device-Control-6.png)

## **Welche Sicherheitsrisiken entstehen durch USB- und andere Peripheriegeräte?**

Peripheriegeräte, die über einen USB-Anschluss oder Bluetooth verbunden werden, sind überall verbreitet. Sie sind immer noch ein wichtiges Merkmal von geschäftlich genutzten Endgeräten, ob es sich nun um Laptops, Arbeitsplatzrechner oder gar IoT-Smart-Geräte handelt. Auch Bedrohungsakteure haben erkannt, dass Peripheriegeräte mit Verbindung zu Endgeräten in Unternehmen intensiv genutzt werden. In einem kürzlich veröffentlichten Bericht wurde beispielsweise deutlich, dass sich die Cyberbedrohungen für OT-Systeme über USB-Wechseldatenträger in den vergangenen zwölf Monaten fast verdoppelt haben. Mit Malware, die über Wechseldatenträger übertragen wird, öffnen die Angreifer beispielsweise Hintertüren, richten einen persistenten Remote-Zugang ein und liefern weitere schädliche Payloads aus.

Sie denken sich kreative Möglichkeiten aus, um Benutzer dazu zu bringen, fremde USB-Sticks mit ihren Unternehmensgeräten zu verbinden. Bei einem Fall aus dem Hotel- und Gastgewerbe erhielten die Opfer einen Briefumschlag, der eine gefälschte Gutscheinkarte sowie einen USB-Stick enthielt, auf dem sich die Malware befand. Darüber hinaus sind USB-Laufwerke ein Hauptvektor für die Exfiltration vertraulicher und geschäftskritischer Daten. Die jüngste Umstellung auf die Arbeit im Homeoffice (oder an einem anderen Ort außerhalb des eigentlichen Büros) vergrößert das Risiko, dass Mitarbeiter nicht vom Unternehmen genehmigte Peripheriegeräte verbinden, um in der neuen Umgebung arbeiten zu können.

Bei der Entwicklung dieser Funktion haben wir Anforderungen wie Systemstabilität, Interoperabilität und plattformübergreifende Unterstützung (Windows und macOS) berücksichtigt.

![](https://www.sentinelone.com/wp-content/uploads/2018/11/Device-Control-Block.gif)

## **Gerätesteuerung: Einfache Richtlinienverwaltung zum Hinzufügen, Blockieren oder Beschränken von Geräten**

Um die Implementierung zu vereinfachen, bietet diese Funktion maximale Granularität _und_ Flexibilität bei der Definition von Gerätesteuerungsrichtlinien.

Sie können solche Richtlinien für das gesamte Unternehmen, einen bestimmten Standort oder sogar eine Gerätegruppe festlegen. Eine Richtlinie besteht dabei aus mehreren Gerätesteuerungsregeln.

Die Regeldefinition beginnt mit der Auswahl des Schnittstellentyps (USB oder Bluetooth), dann des Regeltyps und der Aktion. Wir können zum Beispiel USB-Geräte auf Grundlage der folgenden Merkmale steuern:

- Anbieterkennung
- Klasse
- Serienkennung
- Produktkennung

Dann kommt die gewünschte Aktion:

- Lese- und Schreibzugriff zulassen
- Nur Lesezugriff zulassen
- Blockieren

Der Administrator kann auf diese Weise fein abgestimmte Richtlinien festlegen. So besteht etwa die Möglichkeit, eine Regel aufzustellen, die einzelnen Benutzern den Zugriff auf bestimmte Arten von USB-Geräten gewährt, anderen ausschließlich den Lesezugriff auf USB-Wechseldatenträger gestattet und für alle anderen Benutzer die Verwendung externer USB-Geräte komplett verbietet.

![](https://www.sentinelone.com/wp-content/uploads/2020/07/1-policy.jpg)

## **Bluetooth-Sicherheit – Lücken schließen**

Das Bluetooth-Protokoll strotzt vor Schwachstellen – vor allem in älteren Versionen. Sicherheitsbewusste Unternehmen sollten es ihren Benutzern nicht gestatten, diese Geräte mit firmeneigenen Endgeräten (und damit mit den Netzwerken) zu verbinden.

Mit der Gerätesteuerung von SentinelOne können Sie die Nutzung _aller_ Bluetooth-Geräte, von Geräten eines bestimmten Typs (z. B. Tastatur, Maus, Headset) oder den Betrieb von Geräten auf Grundlage der von ihnen unterstützten Bluetooth-Protokollversion zulassen oder beschränken. Mit der letzteren Option wird das mit älteren Bluetooth-Versionen verbundene Risiko reduziert.

![](https://www.sentinelone.com/wp-content/uploads/2020/07/2-allow-bluetooth.jpg)

## **Flexibilität und Kontrolle über jedes Gerät**

Die Richtliniendefinition ist für Administratoren mit der SentinelOne-Gerätesteuerung also ganz einfach. Wir wissen jedoch auch, dass jeden Tag neue Geräte hinzukommen können. Deshalb müssen Administratoren flexibel und spontan entscheiden können, wie sie neue USB-Geräte zulassen wollen, sobald sie im System auftauchen (und blockiert werden).

Zu diesem Zweck kann sich der Administrator im Aktivitätenprotokoll der Verwaltungskonsole alle Fälle von blockierten Geräten ansehen und das jeweils blockierte Gerät auch direkt von dort aus genehmigen.

![](https://www.sentinelone.com/wp-content/uploads/2020/07/3-block-event.jpg)

## **Fazit**

Mit der Gerätesteuerung und der Firewall-Steuerung von SentinelOne sind nun alle Bestandteile vorhanden, um alte Virenschutz\-Lösungen durch ein Produkt der nächsten Generation zu ersetzen. Ihre plattformübergreifende Bereitstellung erfolgt wie bei anderen Funktionen der Plattform über den SentinelOne-Agenten und die gleiche Verwaltungskonsole.

The post Feature Spotlight – Verbesserte USB- und Bluetooth-Gerätesteuerung appeared first on SentinelOne DE.

Go to Source
