# How this repository was made (Mozambique)

This file is written by the build on 2026-10-30, the day the source data was retrieved.

## Sources

| Source | Address | Retrieved | Licence |
| --- | --- | --- | --- |
| Assembleia da República: Constituição da República (2004, as approved) | <https://www.parlamento.mz/wp-content/uploads/2022/06/Constituicao-da-Republica.pdf> | 2026-10-28 | No licence stated (Assembleia da República) |
| Assembleia da República, Secretariado-Geral: Legislação Básica (2022) | <https://www.parlamento.mz/wp-content/uploads/2022/08/Legislacao_Basica.pdf> | 2026-10-28 | No licence stated (Assembleia da República) |
| Conselho Constitucional: Historial | <https://cconstitucional.org.mz/historial/> | 2026-10-28 | No licence stated |
| WIPO Lex: Constitution of the Republic of Mozambique (details 21389) | <https://www.wipo.int/wipolex/en/legislation/details/21389> | 2026-10-28 | WIPO Lex terms of use |
| Boletim da República, I Série, n.º 165, 2.º suplemento (25 Aug 2023): Lei n.º 13/2023 | <https://faolex.fao.org/docs/pdf/moz221728.pdf> | 2026-10-30 | Official text of the Imprensa Nacional de Moçambique; copy held by FAOLEX (FAO) |
| Boletim da República, I Série, n.º 99 (25 May 2022): Decreto-Lei n.º 1/2022 (Código Comercial) | <https://bancomoc.mz/media/q4kh3wop/decreto-lei-1-2022_25-de-maio_código-comercial.pdf> | 2026-10-30 | Official text of the Imprensa Nacional de Moçambique; copy published by the Banco de Moçambique |

Every word of law in this repository comes from these sources. Lexifina reads them and records each act's dated versions, and the laws that made each change, in one corpus file. This repository is built from that file.

## What is in it

| Act | Title | Versions | Span |
| --- | --- | ---: | --- |
| `constituicao` | Constituição da República de Moçambique | 2 | 2005-01-21 to 2018-06-12 |
| `lei-do-trabalho` | Lei do Trabalho (Lei n.º 13/2023, de 25 de Agosto) | 1 | 2024-02-21 to 2024-02-21 |
| `codigo-comercial` | Código Comercial (Decreto-Lei n.º 1/2022, de 25 de Maio) | 1 | 2022-09-22 to 2022-09-22 |

The build made 4 commits:

| Commits | Kind |
| ---: | --- |
| 3 | first version of an act |
| 1 | one law on its in-force day |
| 0 | changes no amendment note explains, credited to the publisher |

Other counts:
- 0 commits on the branch `futuro`: laws not yet in force on 2026-10-30.
- 0 commits dated 1 January 1970: in force before 1970, with the real day in their `In-force` trailer.
- 4 laws in the record.
- 8 tags.

## Dates

Each commit is dated at noon (Africa/Maputo) on the day the change took effect. The other dates, enacted and published, are trailers.

When a law gives no date of its own, the default rule applies: Lei n.º 6/2006 (Boletim da República): os actos normativos entram em vigor no prazo neles fixado ou, na falta de fixação, 15 dias após a publicação. Each law here states its own commencement, which is used as given. https://www.inm.gov.mz

## How the history is built

`tools/build.py` turns the corpus into this history in one pass: one commit per law, on the day it took effect. The same corpus always gives the same commit hashes. `tools/check_repo.py` reads every version back from git and compares it with the corpus. `tools/on.py` shows the law as it stood on any date. The corpus format is described in `tools/SCHEMA.md`.

- Corpus: sha256 `7be09fd6612d8409ac8284afac4216a1ad5f5e75b6bc9f192e5ef76b7af21cfc` (1123215 bytes).
- Builder blob hashes (`git hash-object`): `build.py` `a4faa25af85facb64b956b372f6b528aafdd7ec1`, `check.py` `3d00541d258745461ab52b1b0cb4e1716a27a95d`, `on.py` `644a6c0857fbcc72562032b58d57aa113fee0641`.
- Python 3.12.10; git version 2.46.0.windows.1.

## Checks

4 versions of 3 acts read back from git on the day each took effect and on the day before: 0 mismatches. Trailers: 0 commits whose trailers git cannot parse. Health: 0 merge commits on main, 0 commit dates that go backwards, 0 commits dated after the build.

No second build of this corpus has been compared yet.

## Decisions made by hand

- Only born-digital PDFs with a text layer are read; scanned Boletim da República issues and scanned PDFs are left out and named (no OCR).
- Text normalisation: NBSP and runs of spaces collapsed, curly quotes to straight, en and em dashes to "-", page numbers and running footers dropped, lines joined into paragraphs (blank-line separated in the 2004 PDF; in the 2022 compilation a wrapped line ends in a space and a new item starts with its number or letter).
- Units: the preamble and one file per article (art-NNN), the article heading on the first line. Title, chapter and section headings and the closing approval and signature formula are not units.
- CRM v1 from 2005-01-21 (Conselho Constitucional, Historial; WIPO Lex), the 2004 text from the Assembleia da República PDF, credited to the Constitution as approved on 2004-11-16.
- CRM v2 from 2018-06-12 (WIPO Lex: Lei n.º 1/2018 published and in force on 12 June 2018), the revised text from the Assembleia da República compilation "Legislação Básica" (2022). Lei n.º 1/2018 republished the whole text (its art. 304(2) of the new text), so every provision that differs is credited to it, including articles that differ only because the revision renumbered them.
- LEFT OUT, Lei n.º 26/2007 (16 Nov 2007, alters art. 304 of the 2004 text, "Assembleias provinciais"): the only copy found is an image scan (Conselho Constitucional), so its wording is unknown and no version shows it. From 2007 until 2018 art-304 therefore reads as in 2004. The 2018 revision has no counterpart of that article (art. 304 of the 2018 text is "Alterações constitucionais"), so the 2018 commit is right for the text it produces.
- LEFT OUT, the 2023 revision of art. 311(3) (approved by the Assembleia da República on 3 Aug 2023 and promulgated on 4 Aug 2023, per Centro de Integridade Pública): no law number, Boletim da República issue or text-layer copy was found. So the record ends with the 2018 text: art. 311 still reads as before that revision, and the last version here is not today's text.
- The official republication of 2018 (BR I Série n.º 115, 2.º suplemento, 12 Jun 2018) was found only as a scan with an automatic OCR layer (FAOLEX), so it is not used.
- Lei do Trabalho: one version, the law as published (BR I Série n.º 165, 2.º suplemento, 25 Aug 2023), credited to Lei n.º 13/2023 and dated 2024-02-21: art. 274 puts it in force 180 days after publication, and the 180th day counted from the day after publication is 21 Feb 2024. No later amendment is applied: the record holds the text as enacted.
- Lei do Trabalho: the preamble, one file per article (art-NNN) and the glossary annex (anexo); chapter, section and subsection headings, the glossary's letter headings and the closing approval, promulgation and signature formula are not units.
- Código Comercial: one version, Decreto-Lei n.º 1/2022 and its annexed Code as published (BR I Série n.º 99, 25 May 2022), credited to the decree-law and dated 2022-09-22: its art. 8 puts it in force 120 days after publication, and the 120th day counted from the day after publication is 22 Sep 2022. No later amendment is applied: the record holds the text as enacted.
- Código Comercial: the decree-law's preamble and articles (dl-art-NNN) and one file per article of the Code (art-NNN); book, title, chapter and section headings and the approval and signature formula are not units.

## What AI did and did not do

The scripts that read the sources and build this repository were written by Claude Code running Claude Opus 5.5 (`claude-opus-5-5`) under Lexifina's direction.

No model wrote, translated or corrected legal text. No model runs at build time. Every word of law in this repository comes from the official sources above.

The commit messages are written by `build.py` from the source data.

Model use at data-build time:
- Claude Code with Claude Opus 5.5 (claude-opus-5-5) wrote Lexifina's code and chose the sources. No model wrote or altered legal text, and no model runs at build time.
