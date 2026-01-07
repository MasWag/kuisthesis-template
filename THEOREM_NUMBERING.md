# Theorem Numbering Example

This example demonstrates the use of theorem environments numbered within sections.

## Usage

After the fix to `kuisthesis.sty`, you can now define theorem environments that are numbered within sections using the standard LaTeX syntax:

```latex
\newtheorem{thm}{定理}[section]
```

## Expected Output

With this definition, theorems will be numbered as:
- 定理 1.1 (Theorem 1.1 in the first section)
- 定理 1.2 (Theorem 1.2 in the first section)
- 定理 2.1 (Theorem 2.1 in the second section)
- 定理 2.2 (Theorem 2.2 in the second section)
- etc.

## Test File

To test this functionality, compile `test_theorem_numbering.tex`:

```bash
uplatex test_theorem_numbering.tex
uplatex test_theorem_numbering.tex  # Run twice for references
dvipdfmx test_theorem_numbering.dvi
```

Or using latexmk:

```bash
latexmk test_theorem_numbering.tex
```

## Other Theorem Numbering Options

### Independent Numbering

For theorems with independent numbering (not tied to sections):

```latex
\newtheorem{thm}{定理}
```

This will produce: 定理 1, 定理 2, 定理 3, etc.

### Shared Counter

To have multiple theorem-like environments share the same counter:

```latex
\newtheorem{thm}{定理}[section]
\newtheorem{lem}[thm]{補題}
\newtheorem{prop}[thm]{命題}
```

This will number theorems, lemmas, and propositions in a single sequence within each section:
- 定理 1.1
- 補題 1.2
- 命題 1.3
- 定理 1.4
- etc.
