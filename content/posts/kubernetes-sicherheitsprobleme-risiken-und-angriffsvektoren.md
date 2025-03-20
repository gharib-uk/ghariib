---
title: "Kubernetes – Sicherheitsprobleme, Risiken und Angriffsvektoren"
date: 2025-03-19
---

The rapid adoption of Kubernetes and containerization poses some real security challenges. We review best practices for keeping your K8s clusters secure.

The post Kubernetes – Sicherheitsprobleme, Risiken und Angriffsvektoren appeared first on SentinelOne DE.

Die IT-Welt verändert sich rasant, und Container und Kubernetes (K8s) erfreuen sich immer größerer Beliebtheit. Der Wechsel von virtuellen Maschinen zu Containern und später zu Container-Koordinierungsplattformen (die erste Version von Docker wurde 2013 veröffentlicht) vollzog sich innerhalb von nur sieben Jahren. Während einige Startups noch Erfahrungen damit sammeln, wie sie von diesen neuen Ressourcen profitieren können, suchen einige etablierte Unternehmen nach Möglichkeiten, ihre veralteten Systeme zu effizienteren Infrastrukturen zu migrieren.

Die schnelle Einführung von Containern und Kubernetes zeigt die disruptive Wirkung dieser Technologien. Sie haben jedoch auch zu neuen Sicherheitsproblemen geführt. Da Container und Kubernetes so beliebt sind und von vielen Unternehmen ohne angemessene Sicherheitsmaßnahmen eingesetzt werden, sind sie perfekte Ziele für Angreifer.

Ein K8s-Cluster besteht aus mehreren Maschinen, die in einem Master-Node (und seinen Kopien) verwaltet werden. Dieser Cluster kann aus tausenden Maschinen und Services bestehen und ist daher ein hervorragender Angriffsvektor. Daher ist es wichtig, strikte Sicherheitspraktiken zu implementieren.

![](https://de.sentinelone.com/wp-content/uploads/sites/3/2020/12/de_k8s-security-challenges-risks-attack-vectors.png)

# **Absicherung Ihres Clusters**

Der Kubernetes-Cluster enthält zahlreiche dynamische Elemente, die angemessen geschützt werden müssen. Die Absicherung des Clusters ist jedoch keine einmalige Angelegenheit, sondern erfordert Best Practices und ein kompetentes Sicherheitsteam.

Wir erläutern verschiedene Kubernetes-Angriffsvektoren sowie Best Practices zum Schutz Ihres K8s-Clusters.

## **Gewährleisten, dass Kubernetes und die Nodes auf dem neuesten Stand sind**

K8s ist ein Open-Source-System, das kontinuierlich aktualisiert wird. Das GitHub-Repository gehört zu den aktivsten Repositorys der Plattform. Kontinuierlich werden neue Funktionen, Verbesserungen und Sicherheits-Updates hinzugefügt.

Alle vier Monate wird eine neue Hauptversion von Kubernetes mit neuen Funktionen zur Verbesserung des Services veröffentlicht. Gleichzeitig kommen wie bei jeder Software – und ganz besonders bei häufig aktualisierten Programmen – neue Sicherheitsprobleme und Fehler hinzu.

Doch auch in älteren Versionen werden Sicherheitsverletzungen gefunden. Es ist daher wichtig zu verstehen, wie das Kubernetes-Team in diesen Fällen mit Sicherheitsupdates umgeht. Im Gegensatz zu Linux-Distributionen und anderen Plattformen verfügt Kubernetes nicht über eine LTS-Version. Stattdessen versucht Kubernetes ein Backporting von Sicherheitsproblemen für die drei zuletzt veröffentlichten Hauptversionen.

Daher ist es wichtig, dass Ihr Cluster eine der drei zuletzt veröffentlichten Hauptversionen verwendet und Sicherheitspatches zügig implementiert werden. Außerdem sollten Sie die Aktualisierung auf die neueste Hauptversion zumindest einmal innerhalb von zwölf Monaten durchführen.

Zusätzlich zu den Hauptkomponenten verwendet Kubernetes auch Nodes, auf denen die Workloads ausgeführt werden, die dem Cluster zugewiesen sind. Bei diesen Nodes kann es sich um physische oder virtuelle Maschinen handeln, auf denen ein Betriebssystem ausgeführt wird. Dabei ist jeder Node ein potenzieller Angriffsvektor, der zur Vermeidung von Sicherheitsproblemen aktualisiert werden muss. Zur Verringerung der Angriffsfläche müssen die Nodes daher so sauber wie möglich sein.

## **Einschränkung der Zugriffsrechte**

Rollenbasierte Zugangskontrolle (Role-based Access Control, RBAC) ist einer der besten Ansätze, um zu kontrollieren, welche Benutzer wie auf den Cluster zugreifen können. Er bietet die Möglichkeit, die Zugriffsrechte für jeden einzelnen Benutzer detailliert festzulegen. Die Regeln gelten jeweils additiv, sodass jede Berechtigung explizit gewährt werden muss. Mithilfe von RBAC können Sie die Zugriffsrechte (Anzeigen, Lesen oder Schreiben) für jedes Kubernetes-Objekt – von Pods (der kleinsten K8s-Recheneinheit) bis zu Namespaces – beschränken.

RBAC kann auch per OpenID Connect-Token auf einen weiteren Verzeichnisdienst angewendet werden, sodass die Benutzer- und Gruppenverwaltung zentral erfolgen und im Unternehmen auf breiterer Ebene umgesetzt werden kann.

Die Zugriffsrechte sind nicht nur auf Kubernetes beschränkt. Wenn Benutzer zum Beispiel Zugriff auf einen Cluster-Node benötigen, um Probleme identifizieren zu können, ist es sinnvoll, temporäre Benutzer zu erstellen und nach der Behebung der Probleme wieder zu löschen.

## **Best Practices für Container**

Docker, die am häufigsten eingesetzte Container-Technologie, besteht aus Layern: Das innerste Layer ist die einfachste Struktur und das äußere Layer ist die spezifischste. Daher beginnen alle Docker-Images mit einer Form von Distributions- oder Sprach-Support, wobei jedes neue Layer eine Funktion hinzufügt oder die vorherige Funktion modifiziert. Der Container enthält alles, was zum Hochfahren der Anwendung erforderlich ist.

Diese Layer (auch als Images bezeichnet) können öffentlich im Docker Hub oder privat in einer anderen Image-Registry verfügbar sein. Das Image kann zum Zeitpunkt der Image-Erstellung in zwei Formen ausgedrückt werden: als Name plus Bezeichnung (z. B. Node:aktuell) oder mit einer unveränderlichen SHA-ID (z. B. sha256:`d64072a554283e64e1bfeb1bb457b7b293b6cd5bb61504afaa3bdd5da2a7bc4b`).

Das Image, das der Bezeichnung zugeordnet ist, kann vom Repository-Inhaber jederzeit geändert werden, d. h. das Tag _aktuell_ weist darauf hin, dass es sich um die neueste verfügbare Version handelt. Das heißt aber auch, dass sich das innere Layer bei der Erstellung eines neuen Images oder beim Ausführen eines Images mit diesem Tag plötzlich und ohne Benachrichtigung ändern kann.

Diese Vorgehensweise verursacht einige Probleme: (1) Sie verlieren die Kontrolle darüber, was in Ihrer Kubernetes-Instanz ausgeführt wird, wenn ein äußeres Layer aktualisiert wird und einen Konflikt verursacht, oder (2) das Image wird absichtlich modifiziert, um eine Sicherheitsschwachstelle hinzuzufügen.

Um das erste Problem zu umgehen, vermeiden Sie das Tag _aktuell_ und wählen ein konkretes Tag, das die Version beschreibt (z. B. Node:14.5.0). Und um das zweite Problem zu vermeiden, wählen Sie offizielle Images und klonen Sie das Image in Ihr privates Repository oder verwenden Sie den SHA-Wert.

Ein weiterer Ansatz besteht darin, ein Tool zur Schwachstellenerkennung einzusetzen und die Images kontinuierlich zu prüfen. Diese Tools können mit kontinuierlichen Integrations-Pipelines parallel ausgeführt werden und das Image-Repository überwachen, um zuvor unerkannte Probleme zu identifizieren.

Bei der Erstellung eines neuen Images müssen Sie beachten, dass jedes Image immer nur einen Dienst enthalten sollte. Außerdem sollte die Zahl der Abhängigkeiten auf ein Minimum beschränkt werden, um die Angriffsfläche auf die Komponenten zu reduzieren, die für den Dienst unerlässlich sind. Wenn jedes Image nur eine Anwendung enthält, lässt es sich zudem leichter für neuere Versionen aktualisieren. Es wird auch einfacher, Ressourcen zuzuweisen.

## **Netzwerksicherheit**

Im vorherigen Abschnitt ging es um die Reduzierung der Angriffsfläche. Das gilt auch für die Netzwerkfunktionen. Kubernetes enthält virtuelle Netzwerke innerhalb des Clusters, die den Zugriff zwischen Pods beschränken und externe Zugriffe erlauben können, sodass nur der Zugriff auf genehmigte Dienste möglich ist. Das ist eine primitive Lösung, die nur in kleinen Clustern funktioniert.

Größere Cluster, die mehrere, von verschiedenen Teams entwickelte Dienste enthalten, sind jedoch deutlich komplexer. In diesen Fällen ist ein zentraler Ansatz eventuell nicht umsetzbar, stattdessen gelten Service Meshes als bestmögliche Methode. Das Service Mesh erstellt eine Netzwerkverschlüsselungsebene, über die Dienste sicher miteinander kommunizieren können. Sie fungieren meist als _Sidecar_\-Agent, d. h. sie werden wie ein Beiwagen an jeden Pod angefügt und ermöglichen die Kommunikation zwischen den Diensten. Service-Meshes übernehmen nicht nur Sicherheitsfunktionen, sondern ermöglichen zudem Erkennung, Überwachung/Nachverfolgung/Protokollierung und vermeiden Dienstunterbrechungen, indem sie zum Beispiel ein als Circuit Breaking (Sicherung) bezeichnetes Entwurfsmuster verwenden.

## **Festlegen von Ressourcenkontingenten**

Da Anwendungen quasi permanent aktualisiert werden, genügt es nicht, für die Absicherung Ihres Clusters einfach die oben beschriebenen Methoden zu implementieren. Das Risiko einer Sicherheitsverletzung bleibt weiterhin bestehen.

Ein weiterer wichtiger Schritt ist die Verwendung von Ressourcenkontingenten, bei denen Kubernetes die Ausfallabdeckung auf die festgelegten Grenzen beschränkt. Wenn diese Grenzen gut definiert sind, wird verhindert, dass im Falle einer Ressourcenerschöpfung alle Cluster-Dienste ausfallen.

Außerdem wird verhindert, dass zum Ende des Monats massive Kosten für die Cloud-Bereitstellung anfallen.

## **Überwachung und Protokollierung**

Die Überwachung des Clusters (vom Cluster bis zu den Pods) ist für die Erkennung von Ausfällen und Identifizierung von Ursachen unerlässlich. Dabei geht es vor allem um die Erkennung von ungewöhnlichen Verhaltensweisen. Wenn der Netzwerkverkehr zugenommen hat oder sich die CPU der Nodes anders verhält, sind weitere Untersuchungen zur Problemsuche erforderlich. Während es bei der Überwachung mehr um Kennzahlen wie CPU, Arbeitsspeicher und Netzwerkleistung geht, kann die Protokollierung zusätzliche (historische) Informationen liefern, mit denen sich ungewöhnliche Muster oder die Quelle des Problems schnell identifizieren lassen.

Die Tools Prometheus und Graphana sind in Kombination für die effektive Überwachung von Kubernetes geeignet. Prometheus ist eine hochleistungsfähige Zeitreihen-Datenbank, während Graphana die Daten aus Prometheus lesen und in übersichtlichen grafischen Dashboards darstellen kann.

ElasticSearch ist ein weiteres nützliches Tool und gehört zu den beliebtesten Tools für die zentrale Protokollierung von Anwendungen, Nodes und Kubernetes selbst fast in Echtzeit.

# **Cloud oder lokal – die Sicherheitsaspekte**

Eine Kubernetes-Installation kann lokal oder über einen Cloud-Management-Dienst erfolgen. Bei einer lokalen Bereitstellung muss jede Konfiguration (Hochfahren neuer Maschinen, Einrichtung des Netzwerks und Absicherung der Anwendung) _manuell_ durchgeführt werden. Bei Cloud-basierten Diensten wie Google GKE, AWS EKS oder Azure AKS kann K8s mit minimalem Konfigurationsaufwand installiert werden. Sie sind dann automatisch mit anderen Diensten dieses Dienstanbieters kompatibel.

In Bezug auf die Sicherheit benötigen lokale Lösungen erheblich mehr Aufmerksamkeit. Wie bereits erwähnt, muss jedes neue Update heruntergeladen und vom System konfiguriert werden, und die Nodes müssen ebenfalls aktualisiert werden. Es wird daher empfohlen, dass lokale Kubernetes-Umgebungen nur von einem erfahrenen Team bereitgestellt werden.

Bei Diensten, die über die Cloud verwaltet werden, ist der Prozess hingegen deutlich einfacher: Kubernetes ist bereits installiert und der Cloud-Anbieter sorgt dafür, dass alle Nodes auf dem aktuellen Stand sind und über die neuesten Sicherheitsfunktionen verfügen. In Bezug auf die Cluster bieten die meisten Cloud-Anbieter Benutzern die Option, eine von mehreren K8s-Versionen zu wählen. Außerdem stellen sie Möglichkeiten zur Aktualisierung auf eine neue Version zur Verfügung. Somit ist diese Vorgehensweise einfacher, aber weniger flexibel.

# **Abschließende Anmerkungen**

In Anbetracht ständiger Updates und der Flut an Tools, die auf den Markt gelangen, bedeuten Aktualisierungen und das Schließen von Sicherheitslücken einen erheblichen Aufwand. Dadurch sind Breaches praktisch unvermeidbar. Bei Kubernetes ist die Herausforderung noch größer, da es sich nicht nur um ein Tool handelt. Vielmehr besteht Kubernetes aus mehreren Tools, die wiederum weitere Tools, Maschinen und Netzwerke verwalten. Die Sicherheit spielt daher eine zentrale Rolle.

Angesichts dieser dynamischen Umgebung ist die Absicherung von Kubernetes alles andere als einfach. Berücksichtigen Sie daher diese Tipps:

- Überprüfen Sie Anwendungen, die auf K8s laufen, auf Sicherheitsprobleme.
- Beschränken und kontrollieren Sie den Zugriff.
- Stellen Sie sicher, dass alle Komponenten die neuesten Sicherheitsupdates enthalten, und überwachen Sie den Cluster kontinuierlich, um Ausfälle sofort beheben und so Schaden zu vermeiden.

Die Herausforderung ist bei lokalen Bereitstellungen noch größer, da hier echte Hardware verwaltet, Automatisierungen eingerichtet und mehr Software-Programme aktualisiert werden müssen. Wenn Sie die hier beschriebenen Best Practices befolgen, profitieren Sie von einem großen Sicherheitsvorteil und können den sicheren und zuverlässigen Betrieb Ihrer Kubernetes-Umgebung gewährleisten.

_Die_ _SentinelOne-Plattform_ _unterstützt physische und virtuelle Maschinen, Docker, selbstverwaltete Kubernetes-Instanzen und über Cloud-Dienstanbieter verwaltete Kubernetes-Implementierungen wie AWS EKS. Wenn Sie mehr erfahren möchten, fordern Sie noch heute eine_ _kostenlose Demo_ _an._

The post Kubernetes – Sicherheitsprobleme, Risiken und Angriffsvektoren appeared first on SentinelOne DE.

Go to Source
