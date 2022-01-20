# MAP

## Microservice API Patterns

### Atomic Parameter

#### Consequence (Atomic Parameter)

Bei Requests welche zusätzliche Informationen übermitteln sollen ist Atomic Parameter nicht geeignet, da sonst mehrere Requests abgesetzt werden müssten. Dazu greift man besser zu Atomic Parameter List.

#### Consequences

Einige Integrationsplattformen verbieten den Kommunikationspartner mehrer Skalare in einem bestimmten Nachrichtentyp zu senden. Zum Beispiel erlauben viele Programmiersprachen nur einen `out` Parameter oder ein einzelnes Objekt als Rückgabewert.

### Parameter Tree

#### Consequences

Wenn sich die Strukur des Domain Models natürlich als Tree abbilden lässt, ist dier Parameter Tree die optimale Variante.

##### Pros

- Wenn zusätzliche Daten (z.B. Sicherheitsinformationen) mit der Message übertragen werden müssen, kann der Parameter Tree die zusätzlichen Daten strukturell von den Domain-Daten trennen.
- Falls nötig, können zusätzliche Daten (z.B. Security Informationen) mit der Message übermittelt werden.
- Parameter Trees sind komplex und können unnötige Elementen enthalten wodurch Brandbreite verschwendet wird. Ist das Domain Model jedoch eine komplexe Baumstruktur, ist die Verarbeitung und auch die Bandbreitennutzung wesentlich effizienter, als das Verwenden von einfacheren Stukruren, die mehrer Messages brauchen.

##### Cons

- Zu grosse Komplexität: Es kann Sinnvoll sein, die Parameter Trees durch einfachere Strukuren zu ersetzen, wenn diese nicht wesentlich mehr Messages versenden.
- Die Komplexität kann zu Security und Privacy issues führen.
- Die Komplexität kann dazu führen, dass zu viele (strukturelle) Informationen geteilt werden -> Verletzung des _Loose coupeling_ Prinzips.

### Parameter Forest

#### Consequences (Parameter Forest)

##### Pro

- Wenn die Parameter eines Domänenmodells nicht einfacher dargestellt werden können als mit dem Parameter Forest Pattern, dann bietet es sich gegenüber den Alternativen Parameter List oder Parameter Tree mit einem künstlichen Root-Knoten an (wobei das dem Parameter Forest sehr ähnelt). Das sorgt für eine verständliche und schlanke Lösung.
- Wenn zusätzliche Daten mit dem Request mitgesendet werden müssen, bietet der Parameter Forest die Möglichkeit neben den komplexen Datenstrukturen zusätzliche Daten einfach zu übermitteln.
- Bei komplexen Strukturen wie z.B. einem Graphen ist die Übertragung und Verarbeitung sehr effizient.

##### Con

- Aufgrund der Komplexität eignen sich Parameter Forests hinsichtlich der Einfachkeit der Implementierung und Verständlichkeit für die Entwickler nicht immer. Oftmals kann man simplere Strukturen verwenden, wenn sie die Anzahl zu sendenden Requests nicht massgeblich erhöhen und die Komplexität verringern.
- Eine komplexe Verarbeitungslogik kann zu Sicherheitsproblemen führen, falls versehentlich schützenswerte Daten mitgesendet werden.
- Es ist verlockend unnötige Daten in den komplexen Datenstrukturen mitzusenden, was für unnötig aufgeblasene Requests und Responses führt.

### Pagination

#### Consequences (Pagination)

##### Pro

- Pagination verbessert den Verbauch von Ressourcen, sowie die Perfomance allgemein dramatisch. Dies ist dadurch bedingt, dass nur die Daten gesendet werden, welche zu einem bestimmten Zeitpunkt auch absolut nötig sind.
- Aus einem Sicherheitsstandpunkt, verhindert korrekte Pagination Denial-of-Service Attacken, die durch das übermässige Abfragen von Daten entstehen könnte.

##### Con

- Pagination ist nur dann anwendbar, wenn die Response sich an einem Datenset orientieren und in kleine Stücke (Pages) zerlegt werden können.
- Eine Response welche Pagination verwendet ist komplexer und sicher auch unangenehmer zu Benutzen als eine "einfache" Response.
- Pagination erhöht die Kopplung von Client und Provider. Dies kann aber durch ein konsistente Umsetzung der Pagination minimiert werden.
- Wenn ein Client nicht sequentiell auf Pages zugreiffen möchte, sondern mehr Kontrolle über die Selektion des Resultats haben möchte, müssen zusätzliche Parameter definiert und validiert werden.

### Prüfungsfragen

1. Wenn eine Operation mehrere Informationen benötigt, müssten mit dem Atomic Parameter Pattern mehrere Requests/Calls ausgeführt werden?
   **Ja**, Atomic Parameters würden mehrere API calls benötigen.
2. Kann der Parameter im Atomic Parameter Pattern auch ein Objekt enthalten solange es nicht zu komplex ist?
   **Nein**, es dürfen nur skalare Werte verwendet werden.
3. Ein Parameter Tree Pattern, das nur aus Atomic-Parametern besteht ist äquivalent zum Atomic Parameter List Pattern?
   **Ja**
4. Wird Pagination vor allem dazu verwendet, Datensets besser zu strukturieren?
   **Nein**

5. Das Verwenden von primitiven Datentypen bei "Atomic Parameter" erhöht die Kopplung.
   \_-> **Falsch**. Die Verwendung von primitiven Datentypen verringert die Kopplung.
6. Die "Atomic Parameter List" setzt sich aus mehreren Single Scalar Representations zusammen.
   \_-> **Richtig**.
7. Das unendliche Scroll-Feature von Sozialen Netzwerken ist ein typisches Beispiel für die Anwendung des Pagination Patterns.
   \_-> **Richtig**.
8. Bei einem Parameter Tree werden mehrere Root Elemente definiert, während bei einem Parameter Forrest nur ein Root Element definiert wird.
   \_-> **Falsch**.

9. Ist es eine gute Idee beim Atomic Parameter List-Pattern ein Array als Parameter zu verwenden? **Nein**
10. Im Atomic Parameter Pattern geht es grundsätzlich darum, dass die Parameter atomar verarbeitet werden können? **Nein**
11. Wurden bei der folgender Nachrichtendarstellung alle Vorgaben des Parameter Tree Pattern eingehalten?

```JSON
{
    "claim": {
        "id": "0afeb849-6d63-40b6-b52f-21dee16fdda5",
        "evidence":[{
                "name":"example1"
            }
        ]
    },
    "link":"https://localhost/0afeb849-6d63-40b6-b52f-21dee16fdda5"
}
```

**Nein**, da in der Datenstruktur mehr als ein Root-Element vorhanden ist. 4. Der Parameter Forest Pattern ist die ausgeprägteste Form der vorgestellten Patterns (dieses und letztes Mal) und daher das ratsamste Pattern. Die Anwendung ist unbedenklich und stellt keine Probleme dar.  
 **Nein**, auch dieses Pattern hat seine Problemchen. Nur weil das Pattern die meisten Möglichkeiten bietet, muss man es nicht exzessiv und unbedacht für jeden Fall anwenden. Das Hauptproblem des Patterns besteht darin, dass eher mehr Daten übertragen werden, als wirklich benötigt werden. Tief verschachtelte Datenstrukturen werden schnell komplex und stehen der Verständlichkeit im Weg.

1. What does an API-Provider define and provide Instead of a complex API?
   Solution: a List with Key-Value pair in which the key is a single (or multiple) scalar(s)
2. Does the Atomic Parameter Pattern provide Developer convenience and experience?
   Solution: **yes**, except for security
3. Does the pagination pattern decrease overall performance? -> **No**
4. Does the pagination pattern increase security? -> **Yes**

5. Bilden Interface-Responsibility (Schnittstellenverantwortung), Structure (Struktur) und Delivery-Quality (Lieferqualität) die drei Kernkategorien der sprachlichen Organisation?
   **Ja**
6. Können mit dem Atomic Parameter (List) Daten strukturiert werden?
   **Nein**. Für strukturierte Daten müssen die Pattern Parameter Tree oder Parameter Forest verwendet werden.
7. Kann ein Tupel oder eine Liste von Zahlen als Parameter-Forest aufgefasst werden?
   **Nein**. Zahlen sind atomatmoare Typen. Aus ihnen können aber Parameter-Tree oder Parameter-Forest zusammengesetzt werden.
8. Sind verschachtelte Objekte eine Sonderform von Parameter-Collections?
   **Nein**. Parameter-Collections sind eine Sonderform von verschachtelten Objekten.

## Interface Evolution Patterns

### Consequences

#### Pros

- Gute Planbarkeit aufgrund der bereits bekannten Supportdauer

#### Cons

- Limitiert die Möglichkeit auf dringende Changes zu reagieren
- Zwingt Clients zu einem Upgrade an einem gewissen Zeitpunkt, welcher eventuell in Konflikt zu der Roadmap des Clients steht
- Keine Möglichkeit mit Clients umzugehen, die nicht mehr weiterentwickelt werden

### Prüfungsfragen

1. Muss die Rückwärtskompatibilität gewährleistet werden wenn die Major-Version inkrementiert wird?
   **Nein**
2. Bei Limited Lifetime Guarantee ist ein wesentlicher Vorteil, dass keine Koordination mit den Clients nötig ist. Korrekt?
   **Ja**

3. Eine beliebige API Versionierung erlaubt es dem Client die Kompatibilität zu erkennen?
   \_-> **Falsch**.
4. Beim Pattern "Two in Production" weiss der Client von Anfang an, bis wann er auf eine neuere Version der API upgraden muss?
   \_-> **Richtig**.

5. Dürfen für eine Anfrage mehrere Version-Identifiers verwendet werden? Z.B. ein GET v1/customers/1234 mit Accept: text/json; version=3.1
   **Ja**. Die Implementation kann sich unabhängig von der Schnittstelle weiterentwickeln. Was zu zu mehreren Versions-Identifiers führt.
6. Ein API Provider darf nach Limited Lifetime Guarantee Pattern keinerlei Änderungen in der API vornehmen.
   **Nein**.
   Die API darf vom Provider verändert werden, insofern sie Rückwärtskompatibel für bestehende Clients bleibt. Sobald das Ablaufdatum der API eintritt, steht es dem Provider frei, den Support der API einzustellen.

7. Can this Pattern also be used outside of APIs? => **Yes**

8. Wenn ich meiner Software eine zusätzliche Funktion hinzufüge, welche andere Funktionen nicht beeinflusst, ändere ich die Version wie folgt x.y.z -> x.(y+1).z?
   **Ja**. Die Anpassung der Nebenversion ist in diesem Fall notwendig.
9. Wenn ein API-Anbieter seine Clients über die Laufzeit einer angebotenen Version informieren möchte, genügt es, wenn er für einen fixen Zeitraum keine Unverträglichkeiten bei der publizierten Schnittstelle verspricht (Garantie durch Anbieter).
   **Nein**. Die Garantie alleine reicht nicht. Alle publizierten Schnittstellen müssen mit einem Ablaufdatum ergänzt werden.

## Game Loop

### Run, run as fast as you can (Fixed time step with no synchronization)

#### Benefits

- Einfache Implementierung
- User Inputs werden nicht blockierend verarbeitet
- Funktioniert gut, wenn spezifische Hardware vorausgesetzt ist und Stromverbrauch nicht von Relevanz ist

#### Problems

- Keine Kontrolle wie schnell das Spiel läuft
- Auf schneller Hardware würde man nicht sehen was passiert
- Content-heavy oder Teile mit mehr AI oder Physics würden auf minderer Hardware real langsamer laufen

### Take a little nap (Fixed time step with synchronization)

#### Benefits

- Immer noch sehr einfache Implementierung
- Schonend für Stromverbrauch
- Spiel läuft nicht zu schnell

#### Problems

- Spiel kann zu langsam laufen
- Wenn `update` und `render` zu lange brauchen, wird gameplay verlangsamt

### One small step, one giant step (Variable time step)

#### Benefits

- Das Spiel läuft auf unterschiedlicher Hardware mit gleichbleibender Geschwindigkeit
- Spieler mit schnelleren Rechnern werden mit einem flüssigeren Spielablauf belohnt

#### Problems

- Nicht mehr Deterministisch
  - Auf unterschiedlichen Hardware werden Berechnungen unterschiedlich oft durchgeführt (floating point operations)

### Play catch up (Fixed update time step, variable rendering)

#### Benefits

- Gameplay speed ist Konsistent
- Updates in fixen Zeitintervallen
- Entkopplung von `update` und `render`

#### Problems

- Komplexität
- Update time step so klein wie möglich für high-end und genug gross für low-end

### Prüfungsfragen

1. Mit der Variante variable time step läuft das Spiel auf unterschiedlicher Hardware mit gleichbleibender Geschwindigkeit und eignet sich somit für multiplayer Spiele?  
   **Nein**
2. Wird ein einfaches `sleep` in den Gaming Loop eingebaut, so wird Gameplayqualität von langsamen Maschinen beeinflusst?  
   **Nein**

3. Ein Game Loop mit einem Synchronisierungsschritt am Ende kann nicht verhindern, dass ein Spiel sich auf alter Hardware verlangsamt.
   -> Korrekt, wenn der update Schritt länger geht als die Framelänge hilft die Synchronisierung nichts.
4. Ein klassischer Game Loop wartet auf User Input bevor der Loop weiter ausgeführt wird.
   -> **Falsch**. Wenn auf User Inputs gewartet werden würde, würde das Bild einfrieren und der Game State würde sich nicht weiter anpassen bis der nächste Input getätigt würde.

5. Beim Game Loop können nur Probleme entstehen, wenn die Hardware zu langsam ist?
   **Falsch**, auch wenn die Hardware zu schnell ist, kann es zu Problemen führen.
6. Es gibt 2 wichtige Bestandsteile beim Game Loop. Nicht auf Benutzer Eingaben warten und das Spiel muss mit einer konstanten Geschwindigkeit laufen
   **Richtig**, siehe Key Parts

7. A variable time step is a fine solution if you are planning to run your app on multiple different devices
   **False**
8. It is recommended to always take the platform event loop
   **False** (It depends)

9. Das Game-Loop-Pattern schreibt vor wie Verzögerungen behandelt werden?
   **Nein**, das Game-Loop-Pattern bietet mehrere Subpatterns, welche verschiedene Varianten aufzeigen.
10. Das Subpattern «Play catch up» berücksichtigt bei unterschiedlichen Geräten einen gleichen Spielverlauf?
    **Ja**, sowie das «Stuck in the middle» Subpattern.

## Component Game Pattern

## Consequences

### Pros

- Bessere Wiederverwendbarkeit von Logik der einzelnen _Domains_
- Reduziert Kopplung der einzelnen _Domains_ massiv
- Austauschbarkeit der Komponenten

### Cons

- Je nach Granularität der Komponenten nimmt die Komplexität enorm zu
- Korrekte Kommunikation zwischen den Komponenten ist anspruchsvoll
- Golden Hammer

### Prüfungsfragen

1. Sollte man für die Kommunikation zwischen Komponenten nur eine Variante verwenden?
   **Nein**
2. Mit Hilfe dieses Patterns kann ein Auto-Pilot für einen Spieler auf einfache Art und Weise implementiert werden?
   **Ja**

3. Die erschwerte Kommunikation zwischen Komponenten im Komponentenpattern ist eine negative Konsequenz.
   -> **Richtig**
4. Die Komponenten im Komponentenpattern können weniger einfach wiederverwendet werden als bei klassischen Vererbunghierarchien
   -> **Falsch**

5. Erhöht das Component Pattern die Kopplung zwischen verschiedenen Klassen?
   **Nein**, es sorgt für eine lose Kopplung.
6. Macht es in jedem Fall Sinn das Component Pattern anzuwenden?
   **Nein**, nicht in jedem Fall. Bei einer grossen Codebasis macht es oft durchaus Sinn. In kleinen Projekten kann es dazu kommen, dass das Pattern ein Problem löst, welches gar nicht bestanden hat.

7. Goal of this Pattern is High Coupling and Low Cohesion? => **False**
8. The Component Pattern is only useful when programming a game. => **False**

9. Das Component Pattern löst das Problem von zu gekoppelten Klassen?
   **Ja**. Durch die Anwendung des Component Patterns können Klassen nach Domänen geteilt werden.
10. Die Basisklasse, welche alle Komponenten zusammenbindet, kennt die genauen Implementationen der Komponenten und kann dadurch einfach in das Verhalten der Komponenten eingreifen?
    **Nein**. Die Basisklasse kennt die genauen Implementationen nicht und sollte in das Verhalten der Komponenten nicht eingreifen. Das Prinzip des Component Pattern ist die Trennung nach Domänen.

## Event Queue

## Consequences

### Pro

- Der Code für das Empfangen einer Nachricht ist vom Code für das versenden einer Nachricht entkoppelt.
- Wir haben eine Queue für die Koordination zwischen den Sender und Empfänger.
- Die Queue ist entkoppelt vom rest des Programmes.

Somit muss nur noch die "Thread-Safty" im Programm garantiert werden, welcher im Higher-Level gemacht werden kann.

### Cons

- Komplexität eher hoch -> Observer- oder Command-Pattern eventuell ausreichend
- Notwendige Information über den aktuellen Context muss immer in der Datenstruktur (`PlayMessage`) übergeben werden, da alles asynchron ist.

### Prüfungsfragen

1. Das Event Queue Pattern dient primär zur "statischen" Entkopplung von Sender und Empfänger?
   **Nein**
2. Versendet eine Queue Nachrichten immer an mehrere Empfänger?
   **Nein**

3. In einer einfachen Message Queue kann "gedrängelt" werden.
   -> **Falsch**. Es gibt in einfachen Message Queues keine Priorisierungen.
4. Message Queues werden oft mit Circular Arrays (a.k.a Ring Buffers) implementiert.
   -> **Richtig**.

5. Globale Queues bergen Performanceprobleme?
   **Nein**, nicht per se. Wenn man es richtig macht, schon. Soll die Event-Queue threadsafe sein, geht aufgrund von synchronisationsmechanismen Performance verloren.
   Dies kommt aber nicht gratis daher, das muss bewusst gemacht werden.
   Ist eine Queue nicht threadsafe, so birgt sie andere Gefahren, schmälert aber nicht die Performance an sich.
   Hier eignet sich eventuell der Einsatz mehrerer Event-/Message-Queues.
6. Dieses Pattern entkoppelt zeitliche Ausführung und die Komponenten voneinander
   **Ja**, bei diesem Pattern handelt es sich und die Entkoppelung der Komponenten an sich und die Ausführung ist ebenso zeitlich voneinander entkoppelt.
   Gerade bei Events ist nicht garantiert, dass diese auch direkt abgearbeitet werden.

7. Events use interrupts
   **No**
8. If I only want to decouple Elements which receive messages from its sender, I could also use an Observer- or Command-Pattern.
   **Yes**

9. Nicht jede Event-Queue braucht eine listenartige Struktur, in welcher die Events gehalten werden?
   **Nein**. Jede Event-Queue benötigt irgend eine Art Liste, in welcher die Events abgespeichert werden.
10. Klassische synchrone Events (auf Systemen ohne Event-Queue) sind datenlastiger als Events in einer Event-Queue (auf Systemen mit Event-Queue)?
    **Nein**. Events in einer Event-Queue (oder auf einem System mit Event-Queue) sind prinzipiell datenlastiger. Auf Systemen ohne Event-Queue reicht eine Art Eventtyp aus, da in den Umsystemen direkt nachgeschaut werden kann, was sich geändert hat. Auf Systemen mit Event-Queue können sich die Umsysteme bereits verändert haben, oder existieren bereits nicht mehr, diese Daten müssen zusätzlich im Event gespeichert werden.
