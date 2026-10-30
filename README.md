# Mozambique: law history in git

The law of Mozambique as a git history: one file per article, one commit per law on the day it took effect, and the law that made each change as the commit's author. `git blame` names the law behind every paragraph; `git log` lists every change to an article; `tools/on.py` gives the text in force on any day.

It follows the shared design of [lexifina/global-law](https://github.com/lexifina/global-law), which places it at `mz/`. The texts are born-digital PDFs of the Assembleia da República and of the Boletim da República (the official gazette); nothing is read from a scan.

**What is in it**

| Folder | Act | Versions | History | Today's text? |
| --- | --- | ---: | --- | --- |
| `constituicao/` | Constituição da República de Moçambique (CRM) | 2 | 2005-01-21 to 2018-06-12 | no: the 2023 revision of art. 311(3) is missing (see below) |
| `codigo-comercial/` | Código Comercial (Decreto-Lei n.º 1/2022, de 25 de Maio) | 1 | 2022-09-22 | the text as enacted; no later amendment is applied |
| `lei-do-trabalho/` | Lei do Trabalho (Lei n.º 13/2023, de 25 de Agosto) | 1 | 2024-02-21 | the text as enacted; no later amendment is applied |

| In force | Author |
| --- | --- |
| 2005-01-21 | Constituição da República (2004), approved 16 Nov 2004: the text as approved |
| 2018-06-12 | Lei n.º 1/2018 (revisão pontual), published and in force 12 Jun 2018 |
| 2022-09-22 | Decreto-Lei n.º 1/2022 (Código Comercial), published 25 May 2022, in force 120 days later |
| 2024-02-21 | Lei n.º 13/2023 (Lei do Trabalho), published 25 Aug 2023, in force 180 days later |

For the Constitution the record ends with the 2018 text: for any later date it shows that text, which is not today's text for art. 311 (see below). The Código Comercial and the Lei do Trabalho are the texts as enacted.

## A lawyer's questions as git commands

| Question | Command |
| --- | --- |
| What did art. 311 say on 2020-01-01? | `python tools/on.py 2020-01-01 constituicao art-311` |
| The same, with raw git (bash) | `git show $(git rev-list -1 --before="2020-01-01T23:59:59Z" HEAD -- constituicao/art-311.md):constituicao/art-311.md` |
| The same, in PowerShell | `$r = git rev-list -1 --before="2020-01-01T23:59:59Z" HEAD -- constituicao/art-311.md; git show "$($r):constituicao/art-311.md"` |
| Which law wrote each line? | `git blame constituicao/art-008.md` |
| Every change, word by word | `git log -p --word-diff constituicao/art-008.md` |
| Every law in the record, in order | `git log --reverse --format="%ad %an" --date=short` |
| Everything one law changed | `git show law/<key>` (tags `law/<key>` mark each law's commit) |
| The whole act on a date | `python tools/on.py 2010-01-01 constituicao` |

After a rebuild, refresh tags with `git fetch --force --tags`: a plain fetch keeps stale tags.

## Dates

- Each commit is dated at **noon, Africa/Maputo, on the day the change took effect**. The day is also in the `In-force:` trailer, which the tools read; approval, promulgation and publication dates are trailers too.
- **2005-01-21:** the CRM entered into force on the day after the 2004 general election results were validated and proclaimed (art. 306); the Conselho Constitucional states the day ("tendo entrado em vigor a 21 de Janeiro de 2005"), and WIPO Lex agrees.
- **2018-06-12:** Lei n.º 1/2018 was promulgated on 11 June 2018 and published and in force on 12 June 2018 (WIPO Lex).
- **2022-09-22:** Decreto-Lei n.º 1/2022, which approves the Código Comercial annexed to it, was published in the Boletim da República, I Série, n.º 99, on 25 May 2022; its art. 8 puts it in force 120 days after publication, which is 22 September 2022 counting from the day after publication.
- **2024-02-21:** Lei n.º 13/2023 was published in the Boletim da República, I Série, n.º 165, 2.º suplemento, on 25 August 2023; its art. 274 puts it in force 180 days after publication. Counting from the day after publication (Código Civil art. 279(b)), the 180th day is 21 February 2024.
- `--before="YYYY-MM-DD"` alone uses your clock's time of day, so a law that took effect that day may be missed before noon. Use `T23:59:59Z` as above, or the tool.

## How it is checked

- Every version is read back from git on the day it took effect and on the day before (`tools/check_repo.py`); both must match `corpus.json`. Results are in `REPRODUCE.md`.
- The two texts were lined up article by article (by heading, since the 2018 revision renumbers most articles from art. 140 on): 128 articles are unchanged in place, 109 kept their words under a new number, and the remaining 70 differ in wording, every one of them reviewed by hand for extraction errors.
- Every paragraph and article heading of the Código Comercial and of the Lei do Trabalho was found, in order, in a second, independent extraction of the gazette PDF (pdftotext); the decree-law's articles 1 to 8, the Code's articles 1 to 554 and the Lei do Trabalho's articles 1 to 274 are all present.
- Every article of both Constitution versions was compared, character for character with spacing ignored, with a second, independent extraction of the same PDFs (pdftotext): every character of every article file is in the PDF in the same place; the only text the PDFs have in addition inside articles is page numbers and running footers.
- The repository is rebuilt from the same data a second time and must give identical hashes.

## What "author" means

The author of a commit is the law that made the change, with an e-mail address that cannot exist, so `git blame` and `git log --author` name laws. The committer is always Lexifina.

- **The first commit** is the Constitution as approved in 2004.
- **Decreto-Lei n.º 1/2022** is the author of the whole `codigo-comercial/` folder: the decree-law (`dl-art-NNN`, its own articles, including the provisions it amends in other laws) and the Code annexed to it (`art-NNN`), as enacted.
- **Lei n.º 13/2023** is the author of the whole `lei-do-trabalho/` folder: the law as enacted.
- **Lei n.º 1/2018** republished the whole revised text with the revision (art. 304(2) of the new text), so every article that differs in 2018 is credited to it, including articles that changed only because the revision renumbered them, and small orthographic differences in the republished text ("juízes", "decretos-lei", "número" for "n.º").
- Blame credits whole lines (one paragraph per line). Use `git log -p --word-diff` to see exactly which words each change made.

## What it does not show

- **Lei n.º 26/2007** (16 Nov 2007) amended art. 304 of the 2004 text (deadline for the provincial assembly elections). The only copy found is a scanned image, so its wording is not here: from 2007 to 2018 `art-304.md` still reads as in 2004. The 2018 revision has no counterpart to that article.
- **The 2023 revision of art. 311(3)** (district elections; approved 3 Aug 2023, promulgated 4 Aug 2023): no law number, Boletim da República issue or text-layer copy was found. `art-311.md` therefore still reads "têm lugar no ano de 2024", which is no longer the law.
- **Headings and formulas.** Title, chapter and section headings, the glossary's letter headings and the closing approval and signature formula are not files; each article file starts with its number and heading.
- **Other statutes.** The Código Penal (Lei n.º 24/2019), Código de Processo Penal and Código Civil are not here yet.
- **No OCR.** The official republication of 2018 (BR I Série n.º 115, 2.º suplemento) and the Conselho Constitucional's PDFs are scans and are not used.

## How it is built

Lexifina reads the sources listed here and records each act's dated versions, and the laws that made each change, in one corpus file. `tools/build.py` writes this repository from that file, and the same corpus always gives the same commit hashes; `REPRODUCE.md` gives the details and the checks. A model (Claude) wrote Lexifina's code; it wrote no legal text, and no model runs when the history is built.

Lexifina's code reads each PDF through its own text layer with PyMuPDF: it joins the PDF lines into paragraphs (blank-line separated in the 2004 PDF; in the 2022 compilation a wrapped line ends in a space), drops page numbers and running footers, normalises spacing, quotes and dashes, splits the text into the preamble and one file per article, and writes `corpus.json`; the shared builder `tools/build.py` turns that into the commits.

## Sources

| Source | What it gives | Used for |
| --- | --- | --- |
| [Assembleia da República](https://www.parlamento.mz): Constituição da República (PDF) | The 2004 text as approved, born-digital | Version 2005-01-21 |
| Assembleia da República, Secretariado-Geral: *Legislação Básica* (2022, PDF) | The CRM as revised by Lei n.º 1/2018, born-digital | Version 2018-06-12 |
| [Conselho Constitucional](https://cconstitucional.org.mz/historial/) | In-force date of the 2004 CRM; scans of the 1990, 2004 and 2018 texts and of Lei n.º 26/2007 | The 2005 date (scans not used) |
| [WIPO Lex](https://www.wipo.int/wipolex/en/legislation/details/21389) | Dates of the CRM and of Lei n.º 1/2018 | The 2018 date |
| Boletim da República, I Série, n.º 99 (25 May 2022), copy published by the [Banco de Moçambique](https://bancomoc.mz/media/q4kh3wop/decreto-lei-1-2022_25-de-maio_código-comercial.pdf) | Decreto-Lei n.º 1/2022 and the Código Comercial, born-digital gazette PDF | Código Comercial |
| Boletim da República, I Série, n.º 165, 2.º suplemento (25 Aug 2023), copy held by [FAOLEX](https://faolex.fao.org/docs/pdf/moz221728.pdf) | Lei n.º 13/2023, born-digital gazette PDF | Lei do Trabalho |
| [Boletim da República](https://www.inm.gov.mz) (Imprensa Nacional) | The official gazette; sold, not open | The issue above only |
| [Centro de Integridade Pública](https://www.cipmoz.org) | Analysis of the 2023 revision (dates of approval and promulgation) | Listing what is missing |

## Licence

See [LICENSE](LICENSE). The texts are official legislative texts, which Mozambique's copyright law (Lei n.º 9/2022, art. 7) leaves unprotected. Lexifina's arrangement, messages, tables and tools are CC0.

This repository is not legal advice.

## Em português

Este repositório guarda a Constituição da República de Moçambique o Código Comercial (Decreto-Lei n.º 1/2022, em vigor desde 22 de Setembro de 2022) e a Lei do Trabalho (Lei n.º 13/2023, em vigor desde 21 de Fevereiro de 2024), estes dois no texto tal como publicado, como um histórico git: um ficheiro por artigo e um commit por lei, datado do dia da sua entrada em vigor (meio-dia, hora de Maputo), com a lei como autora. Contém o texto de 2004 (em vigor desde 21 de Janeiro de 2005) e o texto revisto pela Lei n.º 1/2018 (em vigor desde 12 de Junho de 2018), ambos lidos dos PDF da Assembleia da República, sem OCR. Faltam a Lei n.º 26/2007 (altera o artigo 304; só existe digitalizada) e a revisão de 2023 do n.º 3 do artigo 311 (sem texto oficial digital encontrado), pelo que o último texto aqui não é o texto em vigor hoje. Não constitui aconselhamento jurídico.
