# Generate pdf

```bash
pandoc --defaults .\pandoc-settings.yaml
```

# Notes

Vorlesung:

- Pattern Zusammenfassung
  - wie erkennt man es?
    - Pseudoimplementation
    - Klassendiagramm
  - wie unterscheidet es sich zu ähnlichen Patterns? Wie kann ich sie eindeutig auseinanderhalten?
  - Für welche Art von Problem eignet sich das Pattern?
    - Für welche nicht?
    - Warum könnte ein anderes Pattern nützlicher sein?
    - Beispiele für klassische Probleme, z.B. Chain of Responsibility
- Framework
  - ?

# Flashcards on Card2Brain

All patterns learned through presentations are available to learn as flashcards on card2brain under the following links:

- [POSA Patterns](https://card2brain.ch/folder/pf_posa)
- [MAP Patterns](https://card2brain.ch/folder/pf_map)
- [Game Programming Patterns](https://card2brain.ch/folder/pf_game_programming)

# Common issues

Generating the pdf fails if tab and whitespace characters are mixed:

```bash
! LaTeX Error: Too deeply nested.
```
