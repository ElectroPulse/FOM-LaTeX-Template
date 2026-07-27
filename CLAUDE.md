# CLAUDE.md

Working notes for this repository. Read before editing `.tex`/`.bib` files.

## What this is

A fork of the [FOM-LaTeX-Template](https://github.com/andygrunwald/FOM-LaTeX-Template)
(scientific papers for the FOM Hochschule, German, follows the *Leitfaden zur
formalen Gestaltung von Seminar-/Abschlussarbeiten*, 2018 edition).

Branch `WA-Seminararbeit-KI-Codegeneratoren` carries a real paper:
**"Potenziale und Risiken des Einsatzes von KI-gestützten Code-Generatoren in der
agilen Softwareentwicklung"** (Seminararbeit, Wissenschaftliches Arbeiten).
`master` still holds the pristine upstream template — do not merge paper content there.

The prose source of truth for the current paper is
`todo/Seminararbeit_final_fuer_LaTeX_noAI.md`; the `.tex` files under `kapitel/`
are its transferred form. `todo/` is untracked scratch input, not part of the build.

## Build

```bash
./compile.sh          # German (default) -> thesis_main.pdf
./compile.sh en       # English          -> thesis_englisch.pdf
docker compose up     # same, containerised
```

`compile.sh` runs `lualatex` → `biber` → `lualatex` → `lualatex` and cleans aux
files before and after. Three passes are **required**: `printonlyused` acronyms,
the glossary and the TOC all need a previous `.aux`.

**No TeX toolchain is installed in this environment** (`lualatex`, `biber`,
`latexmk`, `docker` all absent). Any edit here is unverified until the user
compiles. Prefer conservative LaTeX constructs and verify structure with grep/
scripts instead (see *Sanity checks* below).

`countwords.sh` counts words; `%TC:ignore` / `%TC:endignore` in `thesis_main.tex`
fence off the front and back matter.

## Layout

| Path | Purpose |
|---|---|
| `thesis_main.tex` | Master file: full preamble, document skeleton, verzeichnis toggles |
| `skripte/meta.tex` | **Author, title, Betreuer, Matrikelnr., Studiengang…** — edit here, never in titelseite |
| `skripte/kapitelUebersicht.tex` | The ordered `\input` list of chapters. Add/remove chapters here |
| `skripte/textcommands.tex` | Text macros: `\vglf` (Vgl.), `\pagef` (S. ), `\zb`, `\dah`, heading names |
| `skripte/modsBiblatex2018.tex` | The FOM-2018 citation style. Owns nearly all bibliography formatting |
| `skripte/modsBiblatex.tex` | Same for the old (`fom_alt`) style — unused |
| `skripte/symbolDef.tex` | Formula symbols (`\newsym`) — currently disabled |
| `skripte/weitereEbene.tex` | Adds a 4th outline level |
| `kapitel/titelseite.tex` | Title page; pulls everything from `meta.tex` |
| `kapitel/<name>/<name>.tex` | One directory per chapter, file named like the directory |
| `kapitel/anhang/anhang.tex` | Appendix body (currently the KI-Hilfsmittelverzeichnis) |
| `kapitel/anhang/erklaerung.tex` | Eigenständigkeitserklärung — always last, uses `abbildungen/unterschrift.png` |
| `kapitel/anhang/sperrvermerk.tex` | Optional confidentiality notice, disabled in `thesis_main.tex` |
| `abkuerzungen/acronyms.tex` | Abkürzungsverzeichnis (`acronym` package) |
| `abkuerzungen/glossar.tex` | Glossar of Fachbegriffe/Fremdwörter (`glossaries` package) |
| `literatur/literatur.bib` | All sources |
| `abbildungen/` | Images. Only `fomLogo.pdf` and `unterschrift.png` remain |
| `Quellcode/Beispiel.html` | Orphaned demo asset for the deleted template chapter — safe to delete |

`scrartcl`: `\section` = chapter, `\subsection` = second level.

Current chapters: `einleitung`, `grundlagen`, `potenziale`, `risiken`,
`wuerdigung`, `fazit`. Labels follow `sec:<name>` and `subsec:<name>`; cross
references use `Kapitel~\ref{...}`, never hard-coded numbers.

## Citations — the part that bites

Style is selected by `\newcommand{\citationstyle}{fom_2018}` in `thesis_main.tex`
(alternatives: `ieee`, `fom_alt`). `fom_2018` = biblatex `ext-authoryear-ibid`
plus the heavy patching in `skripte/modsBiblatex2018.tex`.

**`usera` is mandatory on every bib entry.** It is the Stichwort/Kurzbeleg that
the style prints in parentheses after the author, in both footnote and
bibliography: `Neumann, M. et al., GenAI Adoption, 2026, S. 297 f.`

Footnote syntax used throughout:

```latex
% single source
Satz.\footcite[\vglf][\pagef 297 f.]{Neumann.2026}
% no page number
Satz.\footcite[\vglf][o.\,S.]{Beck.2001}
% two sources, one shared "Vgl." — note the two empty paren groups
Satz.\footcites(\vglf)()[][\pagef 2]{Peng.2023}[][\pagef 2]{Becker.2025}
```

Behaviours that follow from the config — don't fight them:

- **"Ebd." is automatic.** Always cite the explicit key; `ext-authoryear-ibid`
  substitutes *Ebd.* when the previous citation is identical, and correctly
  prints the full entry again after a page break.
- **`maxcitenames=3`, `mincitenames=1`** → any work with >3 authors cites as
  `Nachname, V. et al.`; `maxbibnames=999` → the bibliography lists all authors.
- **`@online` entries go into the separate *Internetquellen* list**, everything
  else into the Literaturverzeichnis. This is the entry type, not a choice.
- **URLs are stripped** from `book`, `collection`, `incollection`, `article`,
  `inproceedings` by `\AtEveryBibitem`. They survive only on `@online`,
  `@misc`, `@report`.
- **`\DeclareSourcemap` injects `o. O.`** as location for every non-`@online`
  entry that lacks one. Expected, not a bug.
- **Journals always render `Band (Jahr), Nr. X`** — the style hard-codes "Nr.";
  it will not produce "23. Jg. … Heft 3" without editing `modsBiblatex2018.tex`.
- **`shortauthor`** overrides the name in *footnotes only* — used for
  `DORA.2024` so footnotes read "DORA" while the bibliography keeps
  "DORA / Google Cloud".
- **`mincrossrefs=1`**: an `@incollection` with `crossref` to an `@collection`
  makes the parent appear as its own bibliography entry and inherits
  `title`→`booktitle`, editor, series, number, publisher, location, date.
  Always give the child its own `usera`, otherwise it inherits the parent's.
- **arXiv preprints** are modelled as `@misc` with `howpublished = {arXiv:XXXX}`
  so they stay out of *Internetquellen*. URL + access date go in `addendum` as
  `<\url{…}> [Zugriff: …]` — the style only auto-formats that for `@online`.

Key naming convention: `Nachname.Jahr` (e.g. `Neumann.2026`, `XP.2024.Workshops`).

## Acronyms

`\usepackage[printonlyused]{acronym}` — an acronym only appears in the
Abkürzungsverzeichnis if the text actually touches it. The `[DORA]` argument to
the `acronym` environment must be the **longest** abbreviation in the list, it
sets the column width.

**`\acused{}` does not satisfy `printonlyused`.** It only tells `\ac` to use the
short form from now on; it does not register the acronym for the list. An
acronym reaches the Verzeichnis only via a command that actually typesets it —
`\ac`, `\acs`, `\acl`, `\acf` and their plural forms. Verified the hard way: a
build in which KI, DORA and HTTP were marked with `\acused{}` printed only LLM.

German compounds break `\ac{}`: `\ac{KI}-Adoption` expands to
"Künstliche Intelligenz (KI)-Adoption", and `zugunsten von \ac{KI}` produces a
wrong case. Use **`\acs{}`** there instead — it typesets exactly the short form,
so the rendered text is identical to writing the letters by hand, and it
registers correctly: `des \acs{DORA}-Reports`, `eines \acs{HTTP}-Servers`,
`zugunsten von \acs{KI}`. Introduce the long form inline only where it reads
naturally, e.g. `(\aclp{LLM}, \acs{LLM})`.

Never put `\ac{}` in a `\section`/`\subsection` title — it would expand inside
the TOC, which is typeset before the body.

Check that every defined acronym is reachable:

```bash
grep -rnoP '\\ac(s|l|f|p|lp|sp|fp)?\{[A-Z]+\}' kapitel/*/*.tex   # registered uses
grep -oP '\\acro\{\K[A-Z]+' abkuerzungen/acronyms.tex            # defined
```

## Glossary

`\makenoidxglossaries` + `\printnoidxglossaries`. Only entries referenced from
the text are printed. Reference them with:

- `\gls{key}` / `\glspl{key}` — renders `name` / `plural` verbatim, so only use
  where that exact surface form fits the sentence
- `\glslink{key}{beliebiger Text}` — links *and* indexes, use for declined or
  lower-case forms (`\glslink{debugging}{debuggen}`)
- `\glsadd{key}` — index only, no output

Give every entry an explicit `plural=` when the German plural is not `name + s`.

Only the **first** occurrence of a term per document is marked; later mentions
stay plain text. Keep it that way unless asked otherwise.

`nonumberlist` is set on the package, which suppresses the location list (the
page numbers `glossaries` otherwise prints behind every entry). Without it the
Glossar reads like a Stichwortverzeichnis, and `\glsadd{devops}` in
`einleitung.tex` would point at a page where the word "DevOps" is not visible.

**The FOM Leitfaden does not provide for a Glossar at all** (checked against
`todo/Leitfaden…pdf`, Stand Januar 2024 — zero occurrences of "Glossar").
Section 1.1 *Elemente der Arbeit* lists the permitted parts exhaustively and
omits it; 2.7 *Sonstige Verzeichnisse* covers only Rechtsprechungs- and
Quellenverzeichnis. Instead, 1.5.2 requires central terms to be **defined in the
running text**, which Kapitel 2 already does. The Glossar is therefore a
tolerated extra, not a requirement — it was kept by explicit decision. If a
Betreuer objects, removing it means: empty `abkuerzungen/glossar.tex`, drop the
`glossaries` block from `thesis_main.tex`, and strip all `\gls*` macros from
`kapitel/`.

## Verzeichnisse and the TOC

Every Verzeichnis that is built from `\section*` (Abkürzungs- und
Symbolverzeichnis) needs **`\phantomsection` immediately before its
`\addcontentsline`** in `thesis_main.tex`. `\section*` creates no hyperref
anchor, so without it the TOC entry silently links to the title page. The
Glossar (`\printnoidxglossaries`) and the bibliography handle their own anchors.

## Disabled by design

In `thesis_main.tex`, commented out with a note on how to restore:
`\listoffigures`, `\listoftables`, the Symbolverzeichnis block
(`skripte/symbolDef.tex` + `\listofsymbols`), `kapitel/vorwort/vorwort.tex`
(deleted) and `kapitel/anhang/sperrvermerk.tex`. The current paper has no
figures, tables or symbols — re-enable them the moment one is added, otherwise
the PDF gains empty verzeichnis pages.

## Typography conventions in this paper

- Percent: `55,8\,\%` (thin space, escaped sign)
- Negative values: `($-$7,2\,\%)`
- Dashes: `--` for the German Gedankenstrich, `298--300` for ranges
- Page hints: `\pagef 297 f.`, `\pagef 39 f.`, `\pagef 5, \pagef 9`, `o.\,S.`
- Files are UTF-8 with literal umlauts, in `.tex` **and** `.bib` (luainputenc)

## Sanity checks

Since nothing compiles here, verify structurally after edits:

```bash
# citation keys used in text but missing from the bib
comm -23 <(grep -ohP '(?<=\{)[A-Za-z][\w.]*\.\d{4}(?=\})' kapitel/*/*.tex | sort -u) \
         <(grep -oP '(?<=^@)\w+\{\K[^,]+' literatur/literatur.bib | sort -u)

# glossary keys used but not defined
comm -23 <(grep -ohP '\\gls(pl|link|add)?\{\K[a-z]+' kapitel/*/*.tex | sort -u) \
         <(grep -oP '\\newglossaryentry\{\K[a-z]+' abkuerzungen/glossar.tex | sort -u)

# \ref targets without a \label
comm -23 <(grep -ohP '\\ref\{\K[^}]+' kapitel/*/*.tex | sort -u) \
         <(grep -ohP '\\label\{\K[^}]+' kapitel/*/*.tex | sort -u)

# every \includegraphics target must exist in abbildungen/
grep -rn 'includegraphics' kapitel/ skripte/ thesis_main.tex
```

Two bib entries are intentionally uncited: the `@collection` parents
`XP.2026` and `XP.2024.Workshops`, pulled in via `crossref`.

When transferring prose from a Markdown source, diff it word-by-word against the
`.tex` after expanding `\gls*`, `\ac*` and `\ref` and stripping `\footcite*` —
and remember that a naive `%`-comment stripper will eat everything after an
escaped `\%`.

## Housekeeping

- Build artefacts (`*.aux`, `*.bcf`, `*.log`, `*.toc`, `*.sym`, …) are
  gitignored; some stale ones at the repo root are owned by `root` from a
  container run. `compile.sh` deletes them on start.
- `thesis_main.pdf` and `thesis_englisch.pdf` are committed on purpose.
- Do not commit or push unless asked.
