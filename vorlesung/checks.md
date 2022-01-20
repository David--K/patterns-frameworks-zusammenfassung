# CHECKS Patterns

Das Ziel ist es Validen und nicht validen Input zu unterscheiden.
Jede input-based Applikation benötigt solche integrity checks. Die Checks sollten durchgeführt werden, ohne die Komplexität des Programms zu erhöhen.

![CHECKS Übersicht](./assets/checks_overview.png)

## Meaningful Quantities

### Exceptional Behaviour

#### Problem

- Es ist unmöglich invalide oder fehlende Werte in einem Domänen Model zu vermeiden.
- Die Domänen Logik sollte in der Lage sein, diese Art von fehlende Daten zu behandeln.

Wie kann man Ausnahmefälle ausgelöst durch einen invaliden Input handhaben, ohne einen Fehler zu werfen?

#### Lösung

- Invalide Werte führen dazu das `Exceptional Values` erstellt werden.
  - Ein `Enumeration Value` als `Exceptional Value` kann Aussagen was falsch ist.
- Domänen Logik akzeptiert das `Excpetional Value` als legalen Input zumindest temporär.
- For main success scenario, it should not be required to test for exceptional values
  - Only expected value type is used for computation, Exceptional Values are ignored
- Produces meaningful behavior

#### Beispiel

```TypeScript
export enum CalculationError {
  DivByZero = "div/0",
  NumeratorIsNaN = "NaN(numerator)",
  DivisorIsNaN = "NaN(divisor)"
}
export class Calculator {
  public static divide(numerator: number, divisor: number): number | CalculationError {
    if (divisor === 0) { return CalculationError.DivByZero; }
    if (isNaN(numerator)) { return CalculationError.NumeratorIsNaN; }
    if (isNaN(divisor)) { return CalculationError.DivisorIsNaN; }
    return numerator / divisor;
  }
}
```

### Meaningless Behavior

#### Problem

Wegen des Error Handling wird die Domänen Logik komplexer als ursprünglich geplant.

Wie kann man Ausnahmefälle ausgelöst durch einen invaliden Input handhaben, ohne einen Fehler zu werfen?

#### Lösung

- Initiate computation
  - If it fails
    1. recover from failure and continue processing
    2. ensure the error is logged/visualized on surface
- Choose meaningless behavior unless a condition has domain meaning
- As opposed to “operational” meaning (e.g. not-yet-filled-out)
- Represents an alternative implementation of Exceptional Value
  - But provides potentially the lower error information granularity
    - Is it an API usage fault?
    - Is it a user input fault?
    - Is it an internal system fault?

#### Beispiel

```java
public class Calculator {
  public static double divide(double numerator, double divisor) {
    return numerator / divisor; // may result in Double.NaN if divisor is 0.0
    // CAUTION: x / 0 (integer) will throw an ArithmeticException
  }
  public static int divide(int numerator, int divisor) throws ArithmeticException {
    return numerator / divisor; // CAUTION: x / 0 (integer) will throw an ArithmeticException
  }
}

public class CalculatorUi {
  public void onDivideClick(double leftInput, double rightInput) {
    try {
      double result = Calculator.divide(leftInput, rightInput);

      if (!Double.isNaN(result)) {
        // action when calculation completed and a result is available
      } else {
        // action when calculation failed, visualize error on UI
      }
    } catch (ArithmeticException e) {
      // calculation failed, visualize error on UI
    }
  }
}

```
