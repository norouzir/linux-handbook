## Order of Shell Expansions

Bash performs shell expansions in the following order:

| Step | Expansion | Example |
|------|-----------|---------|
| 1 | Brace Expansion | `{a,b}.txt` → `a.txt b.txt` |
| 2 | Tilde Expansion | `~/Documents` → `/home/user/Documents` |
| 3 | Parameter/Variable, Arithmetic, and Command Substitution (performed left-to-right) | `$VAR` → `value`; `$((1+2))` → `3`; `$(command)` → `output` |
| 4 | Word Splitting | Separating based on `IFS` |
| 5 | Pathname Expansion (Globbing) | `*.txt` → `a.txt b.txt` |
| 6 | Quote Removal | Removes quoting characters |

> **Note:** On systems that support it, **Process Substitution** (`<(command)`) is performed simultaneously with tilde, parameter/variable, arithmetic, and command substitution. It is not a separate step in the main sequence.