# Mozambique: law history in git

The law of Mozambique as a git history: one file per article, one commit per law on the day it took effect, and the law that made each change as the commit's author. `git blame` names the law behind every paragraph; `git log` lists every change to an article; `tools/on.py` gives the text in force on any day.

It follows the shared design of [lexifina/global-law](https://github.com/lexifina/global-law), which places it at `mz/`. The texts are the Assembleia da República's own born-digital PDFs; nothing is read from a scan.

**What is in it**

| Folder | Act | Versions | History | Today's text? |
| --- | --- | ---: | --- | --- |
| `constituicao/` | Constituição da República de Moçambique (CRM) | 2 | 2005-01-21 to 2018-06-12 | no: the 2023 revision of art. 311(3) is missing (see below) |

| In force | Author |
| --- | --- |
| 2005-01-21 | Constituição da República (2004), approved 16 Nov 2004: the text as approved |
| 2018-06-12 | Lei n.º 1/2018 (revisão pontual), published and in force 12 Jun 2018 |

Because the last version is not today's text, this history is published on the `preview` branch only, not on `main`.

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
- `--before="YYYY-MM-DD"` alone uses your clock's time of day, so a law that took effect that day may be missed before noon. Use `T23:59:59Z` as above, or the tool.

## How it is checked

- Every version is read back from git on the day it took effect and on the day before (`tools/check_repo.py`); both must match `corpus.json`. Results are in `REPRODUCE.md`.
- The two texts were lined up article by article (by heading, since the 2018 revision renumbers most articles from art. 140 on): 128 articles are unchanged in place, 109 kept their words under a new number, and the remaining 70 differ in wording, every one of them reviewed by hand for extraction errors (`tools/parse.py --report`).
- The repository is rebuilt from the same data a second time and must give identical hashes.

## What "author" means

The author of a commit is the law that made the change, with an e-mail address that cannot exist, so `git blame` and `git log --author` name laws. The committer is always Lexifina.

- **The first commit** is the Constitution as approved in 2004.
- **Lei n.º 1/2018** republished the whole revised text with the revision (art. 304(2) of the new text), so every article that differs in 2018 is credited to it, including articles that changed only because the revision renumbered them, and small orthographic differences in the republished text ("juízes", "decretos-lei", "número" for "n.º").
- Blame credits whole lines (one paragraph per line). Use `git log -p --word-diff` to see exactly which words each change made.

## What it does not show

- **Lei n.º 26/2007** (16 Nov 2007) amended art. 304 of the 2004 text (deadline for the provincial assembly elections). The only copy found is a scanned image, so its wording is not here: from 2007 to 2018 `art-304.md` still reads as in 2004. The 2018 revision has no counterpart to that article.
- **The 2023 revision of art. 311(3)** (district elections; approved 3 Aug 2023, promulgated 4 Aug 2023): no law number, Boletim da República issue or text-layer copy was found. `art-311.md` therefore still reads "têm lugar no ano de 2024", which is no longer the law.
- **Headings and formulas.** Title, chapter and section headings and the closing approval and signature formula are not files; each article file starts with its number and heading.
- **No OCR.** The official republication of 2018 (BR I Série n.º 115, 2.º suplemento) and the Conselho Constitucional's PDFs are scans and are not used.

## How it is built

`tools/fetch.py` downloads each PDF with curl, at most one request a second per government host, caching it by the sha256 of its URL (every URL is in `tools/sources.json`). `tools/parse.py` reads only that cache with PyMuPDF's text layer: it joins the PDF lines into paragraphs (blank-line separated in the 2004 PDF; in the 2022 compilation a wrapped line ends in a space), drops page numbers and running footers, normalises spacing, quotes and dashes, splits the text into the preamble and one file per article, and writes `corpus.json`; the shared builder `tools/build.py` turns that into the commits. `REPRODUCE.md` lists every source, command, tool version and decision, and the result of the hash-for-hash rebuild.

## Sources

| Source | What it gives | Used for |
| --- | --- | --- |
| [Assembleia da República](https://www.parlamento.mz): Constituição da República (PDF) | The 2004 text as approved, born-digital | Version 2005-01-21 |
| Assembleia da República, Secretariado-Geral: *Legislação Básica* (2022, PDF) | The CRM as revised by Lei n.º 1/2018, born-digital | Version 2018-06-12 |
| [Conselho Constitucional](https://cconstitucional.org.mz/historial/) | In-force date of the 2004 CRM; scans of the 1990, 2004 and 2018 texts and of Lei n.º 26/2007 | The 2005 date (scans not used) |
| [WIPO Lex](https://www.wipo.int/wipolex/en/legislation/details/21389) | Dates of the CRM and of Lei n.º 1/2018 | The 2018 date |
| [Boletim da República](https://www.inm.gov.mz) (Imprensa Nacional) | The official gazette; sold, not open | Not used |
| [Centro de Integridade Pública](https://www.cipmoz.org) | Analysis of the 2023 revision (dates of approval and promulgation) | Listing what is missing |

## Licence

See [LICENSE](LICENSE). The texts are official legislative texts published by the Assembleia da República; its site states no licence. This repository is private. Lexifina's arrangement, messages, tables and tools are CC0.

This repository is not legal advice.

## Em português

Este repositório guarda a Constituição da República de Moçambique como um histórico git: um ficheiro por artigo e um commit por lei, datado do dia da sua entrada em vigor (meio-dia, hora de Maputo), com a lei como autora. Contém o texto de 2004 (em vigor desde 21 de Janeiro de 2005) e o texto revisto pela Lei n.º 1/2018 (em vigor desde 12 de Junho de 2018), ambos lidos dos PDF da Assembleia da República, sem OCR. Faltam a Lei n.º 26/2007 (altera o artigo 304; só existe digitalizada) e a revisão de 2023 do n.º 3 do artigo 311 (sem texto oficial digital encontrado), pelo que o último texto aqui não é o texto em vigor hoje. Não constitui aconselhamento jurídico.
