# Theorem Numbering Fix (Version 3.01)

## Problem

When using `\newtheorem` with `[section]` option to create theorems numbered by section, the theorem numbers included the full section representation ("第N章" or "Chapter N") instead of just the section number.

For example:
```latex
\newtheorem{theorem}{定理}[section]
\section{サンプル}
\begin{theorem}
...
\end{theorem}
```

Would produce "定理 第1章.1" instead of the expected "定理 1.1".

## Previous Workaround

Users had to manually redefine `\thesection` around each section:

```latex
\renewcommand{\thesection}{第\arabic{section}章}
\section{サンプル}
\renewcommand{\thesection}{\arabic{section}}
```

## Solution (Version 3.01)

The fix separates the internal representation of `\thesection` from its display format:

1. `\thesection` now returns just `\arabic{section}` (e.g., "1", "2", "3")
2. A new `\@seccntformat` command handles the display formatting for section headings
3. Section headings still display as "第N章" or "Chapter N"
4. Theorems, figures, equations, and other counters that reference sections use just the number

## Benefits

- No manual workarounds needed
- Cleaner, more maintainable code
- Consistent behavior across all numbered environments
- Backward compatible with existing documents (section headings look the same)

## Impact on Cross-References

With this change, `\ref{section-label}` will return just the section number (e.g., "1") instead of the full chapter representation (e.g., "第1章"). This is the standard LaTeX behavior and is consistent with how references work for subsections and other numbered elements.

If you prefer to have references show the full chapter format, you can add this to your document preamble:

```latex
% For Japanese documents
\renewcommand{\p@section}{第}
\renewcommand{\thesection}{\arabic{section}章}

% For English documents  
\renewcommand{\p@section}{Chapter~}
```

However, this will bring back the original theorem numbering issue. The recommended approach is to use the section number in references and add contextual text in your writing (e.g., "第1章参照" or "see Chapter 1").

## Technical Details

The key changes in `kuisthesis.sty`:

```latex
% Old definition (ver 3.00 and earlier)
\ifDS@english
\def\thesection{Chapter~\arabic{section}}
\else	
\def\thesection{第\arabic{section}章}
\fi

% New definition (ver 3.01)
\def\thesection{\arabic{section}}

% New \@seccntformat to format section headings
\ifDS@english
\def\@seccntformat#1{%
  \def\@tempa{section}\def\@tempb{#1}%
  \ifx\@tempa\@tempb
    Chapter~\csname the#1\endcsname\quad
  \else
    \csname the#1\endcsname\quad
  \fi}
\else	
\def\@seccntformat#1{%
  \def\@tempa{section}\def\@tempb{#1}%
  \ifx\@tempa\@tempb
    第\csname the#1\endcsname 章\quad
  \else
    \csname the#1\endcsname\quad
  \fi}
\fi
```

## Impact on Existing Documents

This change is **mostly backward compatible**. Existing documents will continue to work without modification:
- Section headings look exactly the same
- Table of contents entries are unchanged  
- Theorem/figure/equation numbering that references sections is improved
- Cross-references to sections (`\ref`) now return just the number (e.g., "1") instead of the full format (e.g., "第1章")
  - This is standard LaTeX behavior and is generally preferred for consistency with subsection references
  - If needed, you can restore the old reference format using `\p@section` (see above)
