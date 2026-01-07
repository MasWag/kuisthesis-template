# kuisthesis-template

## Recent Changes

### Version 3.01 (2026-01-07)
- **Fixed theorem numbering**: Theorems defined with `\newtheorem{theorem}{定理}[section]` are now correctly numbered as "定理 1.1" instead of "定理 第1章.1". See [THEOREM_NUMBERING_FIX.md](THEOREM_NUMBERING_FIX.md) for details.

## Known issues

- This style file currently cannot be used with `pdflatex`.  Instead, use `platex`, `pbibtex`, and `dvipdfmx`.
