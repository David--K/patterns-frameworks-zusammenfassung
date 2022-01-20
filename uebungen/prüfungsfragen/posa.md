# POSA

## Whole Part Pattern

### Consequences

#### Vorteile

- Austauschbarkeit der Komponenten
  - das ganze Whole kann neu implementiert werden ohne den Client zu verändern
- Aufteilung der Verantwortlichkeiten
  - einzelne Verantwortlichkeiten sind an einzelne Parts delegiert
- Wiederverwendbarkeit
  - einzelne Parts / ganze Wholes können wiederverwendet werden

#### Nachteile

- Weniger effizient aufgrund Indirektion
  - Overhead zur Laufzeit (verglichen mit einem Monoliten)
- Komplexität der Zerlegung von Funktionalität in einzelne Teile

### Wichtige Eigenschaften

- Interface von aussen atomar
  - kein Zugriff auf Komponenten welche darin enthalten sind
- ermöglicht emergente Eigenschaften / Verhaltensweisen

### Prüfungsfragen

1. Beim Whole-Part Pattern ist es wichtig, dass das Whole weiterhin den direkten Zugriff auf die Parts ermöglicht. **Nein**
2. Lists, Sets und Maps sind eine Anwendung des _collection-member_ Whole-Part Pattern. **Ja**

3. Komponenten eines Whole-Part Pattens können nicht in anderen Projekten wiederverwendet werden? **Nein**
4. Eine Komplexität des Whole-Part Patterns ist es, Objekte in einzelne Teile zu zerlegen? **Ja**

5. Shared-Parts sind im reinen Whole-Part Pattern erlaubt? **Nein**
6. Das _Whole_ muss **nicht** dasselbe Interface wie seine _Parts_ implementieren? **Ja**

7. Wodurch unterscheidet sich das Whole-Part Pattern vom Facade Pattern?

- Während beide ein "high-level" Interface für den Zugriff auf eine Aggregation von Softwareelementen definieren ist im Facade Pattern auch der direkte Zugriff auf die einzelnen Elemente erlaubt, im Whole-Part Pattern nur der Indirekte.

2. Nenne 1 jeweils einen Vor und einen Nachteil des Whole-Part Patterns

- Siehe Vor- und Nachteile oben. zB:
  - Vorteil: Austauschbarkeit der Teile
  - Nachteil: Schlechtere Effizienz durch zusätzliche Schicht

1. Ist das Whole-Part Pattern rekursiv anwendbar, z.B. Whole->Whole-Part (Auto->Räder->Felge)? **Ja**
2. Kann man bei der Bottom-Up Variante bereits existierende Objekte immer ohne jegliche Anpassung als Part verwenden? **Nein**

## Forwarder-Receiver Pattern

### Consequences

#### Vorteile

- Effiziente IPC
  - die Kommunikation zwischen Prozessen wird strukturiert
  - `Forwarders` kennen die physikalische Erreichbarkeit der `Receivers`, entsprechend müssen Remotecomputer nicht erst gefunden werden
- IPC relevante OS Abhängigkeiten sind in `Forwarders` und `Receivers` abgekapselt

#### Nachteile

- Flexible Rekonfiguration von Komponenten ist nicht möglich
- Falls die Verteilung von Peers sich zur Laufzeit ändern ist es schwer Anpassungen vorzunehmen
  - _(dies kann mittels Central-Dispatcher [siehe Client-Dispatcher-Server Pattern] gelöst)_

### Wichtige Eigenschaften

- Peers müssen die konkreten Mechanismen zur Kommunikation nicht kennen
- IPC Implementation ist flexibel austauschbar

### Prüfungsfragen

1. Das Forwarder-Receiver Pattern nutzt eine hohe Abstraktionsebene der Netzwerkkommunikation **Nein**
2. Das Forwarder-Receiver Pattern ist für eine peer-to-peer Kommunikation ausgelegt **Ja**

3. Das reine Forwarder-Receiver Pattern setzt voraus, dass ein Peer zur compile-time alle Peers kennt mit denen er kommunizieren muss? **Ja**
4. Ein Peer kann nur ein Server oder Client sein nicht beides? **Nein**

5. Ein Peer **braucht zwingend** je einen _Forwarder_ und _Receiver_?
   Falsch, es kann auch One-Way Kommunikation geben und ein Peer daher nur Receiver oder nur Forwarder haben
6. Das Forward-Receiver Pattern eignet sich nicht für Broadcast? **Falsch**
   Falsch, je nach Naming-System, z.b. Gruppen oder das mehrere Peers den gleichen Namen haben, ist Broadcast möglich

7. Ein Peer kann jeweils mehrere Forwarder und Reciever haben? **Falsch**
8. Ein Peer welches während der Laufzeit angehängt wird, wird von anderen Peers erkannt und kann von diesen Nachrichten erhalten? **Falsch**

9. Es wird versucht die Namensauflösung, der Verbindungsaufbau und die Deserialisierung vom Peer zu trennen? **Ja**
10. Peer1 kennt die direkte Adresse des Peer2? **Nein**

## Blackboard Pattern

### Consequences

Das Blackboard pattern kann alle Forces adressieren und diese elegant lösen.

#### Vorteile

- **Experimentieren:** Probleme ohne konkreten Lösungsansatz können ohne alle möglichkeiten auszuprobieren gelöst werden.
- **Maintainability / Austauschbarkeit:** Durch die modulare struktur und unabhängigkeit der Komponenten können einzelne Teile einfach erweitert und ausgetauscht werden.
- **Wiederverwendbarkeit:** Knowledge Sources können für andere Probleme wiederverwendet werden.
- **Robustheit / Fehlertoleranz:** Jegliche Resultate sind hypothesen und verschiedene Pfade werden gegeneinander gewichtet um die optimale Lösung zu finden.

#### Nachteile

- **Testing:** Die Resultate sind nicht reproduzierbar und folgen keinem deterministischen algorithmus.
- **Lösung nicht garantiert:** Nicht alle aufgaben können gelöst werden.
- **Gute Control Strategy schwierig zu finden:** Ein experimenteller ansatz wird jenachdem benötigt um eine Control Strategy zu finden.
- **Effizienz**: Durch zurückgewiesene hypothesen die ausprobiert wurden ist die effizienz eines Blackboard systems tiefer als bei einem deterministischen algorithmus
- **Hoher aufwand**: Die entwicklung eines Blackboard systems ist sehr aufwändig.
- **Paralellisierbarkeit:** Paralellisierung vom pattern nicht angedacht, aber theoretisch möglich.

### Wichtige Eigenschaften

- Undeterministischer Ansatz (Heuristisch)
- Für Komplexe Aufgaben ohne definierten Lösungsweg.
- Löst Teilprobleme und kann diese Lösungsansätze aufeinander aufbauen.
- Kein einheitliches Format der Daten benötigt für die verschiedenen Knowledge Sources.

### Prüfungsfragen

1. Knowledge Sources interagieren direkt miteinander um gemeinsam eine Lösung zu finden. **Nein**
2. Das Blackboard Pattern nutzt einen Deterministischen Ansatz. **Nein**

3. Die Lösungssuche ist erst fertig, sobald eine perfekte Lösung gefunden wurde? **Nein**
4. Das Entwickeln eines Blackboard-Systems kann sehr lange dauern? **Ja**

5. _Knowledge Source's_ kommunizieren untereinander, um Daten abzugleichen und sich über Änderungen zu informieren
   Falsch, <i>Knowledge Source's</i> kommunizieren nie direkt miteinander. Sondern lediglich übers via Blackboard in dem sie Datensätze verarbeiten und neue Datensätze hinzufügen
6. Es kann vorkommen, dass nie eine Lösung gefunden wird oder die Lösungssuche sehr lange dauert.
   Richtig, es ist möglich, dass keine Lösung gefunden wird (auch wenn es eine gibt).

7. Das Blackboard Pattern setzt sich aus einem Blackboard einer Control-Component und einfachen Tasks zusammen? **Falsch** Blackboard, Control Component & Knonwledge Source
8. Alle Knowledge Sourcen können auf die Daten des Blackboard kontrolliert Zugreifen und diese Verändern? **Richtig**

9. Das Pattern ist geeignet, wenn man den Lösungsweg genau kennt? **Nein**
10. Der Controller entscheidet welche Knowledge Source auf das Blackboard schreiben darf. **Ja**

## Lazy Acquisition Pattern

### Consequences

#### Vorteile

- Verfügbarkeit: Es werden nicht alle Ressourcen von Beginn angefordert, dadurch wird die Möglichkeit verringert, dass Ressourcen geladen werden die nicht gebraucht werden.
- Stabilität: Es wird sichergestellt das Ressourcen nur geladen werden, wenn sie benötigt werden.
- Optimaler Systemstart: Ressourcen die nicht sofort benötigt werden, werden erst zu einem späteren Zeitpunkt geladen, dadurch wird die Start zeit optimiert.
- Transparenz: Lazy Acquisiotion ist transparent gegenüber dem Benutzer der Ressource.

#### Nachteile

- Space overhead: Es wird wegen der Indirektion des Proxys zusätzlicher Speicher benötigt.
- Time overhead: Es kann zu einer Verzögerung kommen beim Anfordern von Ressourcen und die Indirektion verursacht ebenfalls eine Verzögerung beim Ausführen des Programms.
- Vorhersagbarkeit: Wenn mehrere Teile des Systems den Zugriff auf eine Ressource hinausschieben und alle zur selben Zeit den Zugriff machen ist die Zugriffszeit nicht vorhersagbar.

### Wichtige Eigenschaften

- Zugriff auf eine Ressource wird nur gemacht wenn sie benötigt wird.
- Performance und Stabilität eines Systems können besser gewährleistet werden.
- Vorhersagbarkeit der Zugriffszeit kann nicht mehr gewährleistet werden.

### Prüfungsfragen

1. Wenn alle Ressourcen zu Beginn geladen werden damit die optimale Performance gewährleistet werden kann, redet man von Lazy Acquisition? **Nein**
2. Lazy Acquisition führt dazu, dass die Startdauer eines Systems sehr viel grösser wird. **Nein**

3. Lazy Aquisition verhindert lange Wartezeiten bei der Nutzung von Software? **Nein**
4. Ziel ist es teure Ressourcen, so spät wie möglich zu laden? **Ja**

5. Das Lazy Pattern ist in jedem Fall die bessere Variante gegenüber "Beim Start laden" (Eager)
   **Nein**, es ist ein klassischer Tradeoff. Es muss je nach Situation entschieden werden was besser ist.
6. Die echte Ressource und der Proxy müssen das gleiche Interface für den User bieten
   **Ja**

7. Die Lazy Aquistion lädt alle Daten früh genug, sodass wenn der User auf diese Zugreifen möchte der Provider nichts mehr tun muss. **Falsch**
8. Das Pattern der Lazy Aquisition besteht aus den vier Klassen: Resource User, Resource Proxy, Resource Provider und Resource? **Richtig**

9. Besitzt das Proxy Objekt ein subset der Funktionen von der echten Ressource? **Nein**
10. Wird beim erstellen und zurückgeben des Proxys auch gleich die Ressource geladen? **Nein**

## Coordinator Pattern

### Consequences

#### Vorteile

- **Atomarität**: Es wird sichergestellt, dass alle Aufgaben erfolgreich abgeschlossen werden oder sonst nichts gemacht wird. "All or nothing" Prinzip.

- **Konsistenz**: Die konsistenz des Systems wird gewährleistet, da ein System von einem validen Zustand in den nächsten wechselt. Und falls das nicht möglich ist, wird das System im alten konsistenten Zustand belassen.

- **Skalierbarkeit**: Es spielt keine Rolle wie viele "Participants" existieren. Denn der Client interagiert nur über den Coordinator mit ihnen.

- **Transparenz**: Der Client kennt nur die Tasks und hat keine Kenntnis der 2-Phasen. Die Konsistenz und Atomarität ist transparent umgesetzt gegenüber dem Client.

#### Nachteile

- **Overhead**: Jeder Task wird in zwei Phasen unterteilt. Die Überprüfung in der ersten Phase und die Ausführung danach stellen einen Overhead dar.

- **Zusätzliche Verantwortung**: Die registrierung der "Participants" beim "Coordinator" stellt eine zusätzliche Verantwortung dar welche die Participants erfüllen müssen.

### Wichtige Eigenschaften

- Das Pattern umfasst 2-Phasen welche nacheinander aufgerufen werden. Nur wenn eine problemlose Ausführung möglich ist, wird diese auch umgesetzt.
- Das System befindet sich immer in einem Validen zustand.
- Der Client hat muss sich nicht um die Konsistenz und Atomarität kümmern.

### Prüfungsfragen

1. Der Client gibt Tasks direkt an einen Participant, welcher diesen dann für ihn atomar ausführt. **Nein**
2. Das Ziel des Coordinator Patterns ist "All or nothing" **Ja**

3. Es ist unmöglich, dass beim two-phase-commit in der commit-Phase ein Fehler auftritt? **Nein**
4. Wenn auf einem Participant ein Task nicht machbar ist, werden die anderen Tasks trotzdem auf allen anderen Participants durchgeführt? **Nein**

5. Das Pattern garantiert, das ein Task in jeden Fall ausgeführt wird
   **Nein**, es garantiert nur, dass es entweder bei allen oder bei keinem Participant ausgeführt wird.
6. Das Pattern wird oft bei Datenbanken eingesetzt
   **Ja**

7. In der Prepare Phase werden noch keine permanenten Änderungen an den Teilnehmer vorgenommen **Richtig**
8. Ein Nachteil des Coordinator Patterns ist, dass die Laufzeit bei Erhöhung der Teilnehmer quadratisch zunimmt. **Falsch**

9. Ist die Konsistenz sichergestellt bei einem Fehler in der Commit-Phase beim Three-Phase Commit? **Ja**
10. Werden die Participants vom Coordinator ausgewählt und registiert? **Nein**

## Resource Lifecycle Manager

### Consequences

#### Vorteile

- Effizienz
- Skalierbarkeit
- Leistung
- Transparenz
- Stabilität
- Kontrolle

#### Nachteile

- Single point of failure: Ein Bug kann dazu führen, dass das grosse Teile des RLM nicht mehr funktionieren.
- Flexibilität: Falls Ressourcen eine sehr spezifische Behandlung brauchen ist der RLM vielleicht zu unflexibel um das umzusetzen.

### Wichtige Eigenschaften

- Transparenz: RLM ist gegenüber Ressourcenbenutzer transparent
- Anpassungsfähigkeit: RLM kann für verschiedene Bedürfnisse entsprechende Strategien zum Management der Ressourcen anwenden

## Prüfungsfragen

1. Der Resource Lifecycle Manager verwendet einen Resource Provider zum Akquirieren der Ressource. **Richtig**
2. Abhängigkeiten von Resourcen haben einen Einfluss auf ihren Lifecycle. **Richtig**

3. Die Idee hinter dem Resource Lifecycle Manager Pattern ist es, die Verwendung und Verwaltung von Ressourcen zu trennen? **Ja**
4. Das Resource Lifecycle Manager Pattern eignet sich besonders gut für «large-scale» Systeme? **Ja**

5. Es darf in einem System mehrere RLMs geben
   **Ja** (siehe Solution)
6. Das Resource Lifecycle Manager Pattern verhindert Single Point of Failures
   **Nein**, im Gegenteil (siehe Nachteile)

7. Die Ressourcen einer Applikation können nur von einem Resource Lifecycle Manager verwaltet werden. **Falsch**
8. Durch den Resource Lifecycle Manager können Ressourcen wieder freigegeben werden sobald sie nicht mehr benötigt werden. **Richtig**

9. Kann der Resource Lifecycle Manager Optimierungen wie Caching / Lazy-Loading implementieren? **Ja**
10. Wird eine resource immer an den resource user zurückgegeben? **Nein** Wenn es zu wenig hat nicht.
