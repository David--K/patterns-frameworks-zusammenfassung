## Encapsulate Context (Optional)

Ein Pattern zum vermindern des Framework Dilema

### Problem

- Ein System beinhaltet Daten, welche divergierenden Teilen des Systems zur Verfügung stehen.
- Wir möchten lange Parameter Listen für Funktionen oder globale Daten vermeiden.

Wie kann vermieden werden, dass spezfische Listen von Parametern an Funktionen übergeben werden müssen um die felxibilität der Interfaces zu gewährleisten?

### Lösung

- Einen `Context container` zur verfügung stellen, welcher alle Daten sammelt.
- Der Context agiert als Container für Zustandsdaten des Programms.
- Das Contextobjekt kann erweitert werden ohne bestehenden Code zu brechen.

### Beispiel für eine schlechte Lösung

```java
// Framework Code V1
public interface ProviderFactory {
  Provider create(
  int id,
  String name
  Object parent // V2
  );
}

// Framework User’s Code (based on V1)
public class WebProviderFactory implements ProviderFactory {
  public Provider create(int id,  String name ) {
    return new WebEndPointProvider(id, name);
  }
}
```

Zum Verwenden der Version 2 muss der Benutzer seinen Code anpassen und beim Aufruf von `create` zusätzlich den parent übergeben

### Beispiel für eine korrekte Lösung

```java
// Framework Code V1
public interface ProviderFactory {
  Provider create(FactoryContext context);
}
public interface FactoryContext {
  int getId();
  String getName();
  Object getParent(); // V2
}
// Framework User’s Code (compatible with V1 & V2)
public class WebProviderFactory implements ProviderFactory {
  public Provider create(FactoryContext context) {
    return new WebEndPiontProvider(
      context.getId(),
      context.getName());
  }
}
```

### Weitere Beispiele

schlecht:

```java
// Framework Code V1
public abstract class EndPointBase {
  protected String address; //V2: null is newly allowed for localhost connections
  protected int port;
  private Connection conn;
  protected abstract Connection open();
}

// Framework User’s Code (based on V1)
public class WebEndPoint extends EndPointBase {
  @Override
  protected Connection open() {
    return new WebConnection(address, port);
  }
}
public class WebConnection extends Connection {
  WebConnection(String address, int port) {
    if (address == null) { //Contract of V1 is enforced; breaks in V2
      throw new InvalidArgumentException();
    }
  }
}
```

gut:

```java
// Framework Code V1
public abstract class EndPointBase {
  // no access to fields (avoid slicing)
  protected abstract Connection open(@NotNull ConnectionContext connCtx);
}
// define contract as API part explicitly
public interface ConnectionContext {
  int getId();
  @NotNull String getAddress();
}

// Framework User’s Code (compatible with V1 & V2)
public class WebEndPoint extends EndPointBase {
  @Override
  protected Connection open(@NotNull connCtx) {
    return new WebConnection(connCtx);
  }
}
public class WebConnection extends EndPointBase {
  WebConnection(@NotNull ConnectionContext connCtx) {
  }
}
```

### Fragen

**Q**: Which already discussed patterns could be used to accomplish a similar result?

**A**: Anything Pattern, Property List Pattern

**Q**: Which pattern would you prefer in consideration of interface flexibility? Explain your answer.

**A**:

- Anything: benefits of Property List but hierarchically structured.
- Property List: Passing a property list as a parameter allows a client component to extend the amount of data passed to an operation at will. If you want to keep stable interfaces, shared by different teams, but are still heavily under development, property lists as parameters or attributes can be a real life-safer.

**Q**: Gibt es Gründe Patterns nicht zu verwenden?

**A**:

- Das Pattern muss ich immer für die Funktionalität eignen, es bringt nichts ein Pattern zu verwenden nur damit man es verwendet hat.
- Ein gutes Design hängt nicht von den verwendet Patterns ab sondern ob sich das Design zum Umsetzten der vorgegebenen Anforderungen eignet, Patterns können unterstützend wirken.
- Man stosst auf ein Problem und merkt dann das sich ein Pattern eignen würde, man fängt nicht mit einem Pattern an versucht dann alles dem Pattern anzupassen.

- sonstiges: Patterns sind nicht auf objekt-orientierte Programmierung begrenzt

## Extension Interface (Optional)

### Problem

- Erlauben mehrere Interfaces zur Verfügung zustellen aber gleichzeit Verhindern von `interface bloating`.
- Verhindern das client code bricht, wenn einer Komponente zusätzliche Funktionalität hinzugefügt wird.

Wie kann man Interfaces designen für unbekanntes und "bloated base classes" vermeiden?

### Lösung

![Extension Interface Klassendiagramm](./assets/extension_interface_1.png){ width=60% }

![Extension Interface Dynamiken](./assets/extension_interface_2.png)

### Vorteile

- Extensibility: Adding interfaces is easier (like adding properties)
- Separation of concerns
- Prevents bloated class interfaces, by aggregating extension interfaces
- Different clients can perceive an abstraction differently
- Classes need not be related to have a common interface (facet)
  - loser coupling, no inheritance required

### Nachteile

- Clients become more complex (first obtain facet then use it)
- Subject's original interface does not convey intent; less type-safety
- Run-time overhead
- May split APIs into in-coherent parts

### Fragen

**Q**: Which patterns are incorporated (eingebaut) by the Extension Interface Pattern?

**A**: Simple Factory, Composite