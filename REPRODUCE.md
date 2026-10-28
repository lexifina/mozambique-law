# How this repository was made (Mozambique)

This file is written by the build on 2026-10-28, the day the source data was retrieved. Everything below is enough to rebuild this repository hash for hash.

## Sources

| Source | Address | Retrieved | Requests | Licence |
| --- | --- | --- | ---: | --- |
| Assembleia da República: Constituição da República (2004, as approved) | <https://www.parlamento.mz/wp-content/uploads/2022/06/Constituicao-da-Republica.pdf> | 2026-10-28 | 1 | No licence stated (Assembleia da República) |
| Assembleia da República, Secretariado-Geral: Legislação Básica (2022) | <https://www.parlamento.mz/wp-content/uploads/2022/08/Legislacao_Basica.pdf> | 2026-10-28 | 1 | No licence stated (Assembleia da República) |
| Conselho Constitucional: Historial | <https://cconstitucional.org.mz/historial/> | 2026-10-28 | 1 | No licence stated |
| WIPO Lex: Constitution of the Republic of Mozambique (details 21389) | <https://www.wipo.int/wipolex/en/legislation/details/21389> | 2026-10-28 | 1 | WIPO Lex terms of use |

Every response is cached by the sha256 of its URL, and the table above lists every source with its retrieval date. Lexifina keeps its fetch scripts privately; `tools/parse.py` reads only the cache and writes `corpus.json`, so the same cache always gives the same corpus.

## What is in it

| Act | Title | Versions | Span |
| --- | --- | ---: | --- |
| `constituicao` | Constituição da República de Moçambique | 2 | 2005-01-21 to 2018-06-12 |

The build made 2 commits:

| Commits | Kind |
| ---: | --- |
| 1 | first version of an act |
| 1 | one law on its in-force day |
| 0 | changes no amendment note explains, credited to the publisher |

Other counts:
- 0 commits on the branch `futuro`: laws not yet in force on 2026-10-28.
- 0 commits dated 1 January 1970: in force before 1970, with the real day in their `In-force` trailer.
- 2 laws in the record.
- 4 tags.

## Dates

Each commit is dated at noon (Africa/Maputo) on the day the change took effect. The other dates, enacted and published, are trailers.

When a law gives no date of its own, the default rule applies: Lei n.º 6/2006 (Boletim da República): os actos normativos entram em vigor no prazo neles fixado ou, na falta de fixação, 15 dias após a publicação. Each law here states its own commencement, which is used as given. https://www.inm.gov.mz

## Commands, in order

```sh
# fill the cache from the sources listed above (network; Lexifina's fetch scripts are kept privately)
python -X utf8 tools/parse.py                  # cache -> corpus.json (no network)
python -X utf8 tools/build.py <folder> --repo <empty folder> --docs-date 2026-10-28
python -X utf8 tools/check_repo.py <repo> --corpus <folder>/corpus.json --write
python -X utf8 tools/build.py <folder> --repo <another empty folder> --docs-date 2026-10-28 --compare <repo>
```

In the main Lexifina repository these files live as follows:
- `fetch.py`, `parse.py` and `repo/`: in `scripts/mozambique-law/`.
- `build.py`, `check_repo.py` (`check.py` there) and `on.py`: in `scripts/gitlaw_common/`, with the corpus format in `SCHEMA.md`.

## Exact inputs

- `corpus.json` sha256 `a7a7354837abda9a007b1205adb428752bfd03389d736b93e9f53c4232dcdeec` (377017 bytes)
- Script blob hashes (`git hash-object`):

| Script | Blob |
| --- | --- |
| build.py | `2bfc4ac9df193a7be844556d7f69cdb9d93ec3f2` |
| check.py | `cd346c7ff9996d450240e3bab52bcae03b2bc865` |
| on.py | `644a6c0857fbcc72562032b58d57aa113fee0641` |
| fetch.py | `b5284e53b88e3d899309ea426a7cbb1ef5a8732a` |
| parse.py | `a0e210e64dedcd9b58796e4429c32e283593a568` |

- Tools:
  - Python 3.14.0
  - git version 2.52.0.windows.1
  - curl 8.21.0 (Windows) libcurl/8.21.0 Schannel zlib/1.3.2 WinIDN WinLDAP

## Checks

2 versions of 1 acts read back from git on the day each took effect and on the day before: 0 mismatches. Trailers: 0 commits whose trailers git cannot parse. Health: 0 merge commits on main, 0 commit dates that go backwards, 0 commits dated after the build.

A second build from the same corpus into an empty folder gave identical hashes for all 2 history commits and 4 tags (history tip `7ed53af36a2bf73422e33041f210a31b268de5fa`).

These results are from the check run on the build before this one, from the same corpus.

## Decisions made by hand

- Only born-digital PDFs with a text layer are read; scanned Boletim da República issues and scanned PDFs are left out and named (no OCR).
- Text normalisation: NBSP and runs of spaces collapsed, curly quotes to straight, en and em dashes to "-", page numbers and running footers dropped, lines joined into paragraphs (blank-line separated in the 2004 PDF; in the 2022 compilation a wrapped line ends in a space and a new item starts with its number or letter).
- Units: the preamble and one file per article (art-NNN), the article heading on the first line. Title, chapter and section headings and the closing approval and signature formula are not units.
- CRM v1 from 2005-01-21 (Conselho Constitucional, Historial; WIPO Lex), the 2004 text from the Assembleia da República PDF, credited to the Constitution as approved on 2004-11-16.
- CRM v2 from 2018-06-12 (WIPO Lex: Lei n.º 1/2018 published and in force on 12 June 2018), the revised text from the Assembleia da República compilation "Legislação Básica" (2022). Lei n.º 1/2018 republished the whole text (its art. 304(2) of the new text), so every provision that differs is credited to it, including articles that differ only because the revision renumbered them.
- LEFT OUT, Lei n.º 26/2007 (16 Nov 2007, alters art. 304 of the 2004 text, "Assembleias provinciais"): the only copy found is an image scan (Conselho Constitucional), so its wording is unknown and no version shows it. From 2007 until 2018 art-304 therefore reads as in 2004. The 2018 revision has no counterpart of that article (art. 304 of the 2018 text is "Alterações constitucionais"), so the 2018 commit is right for the text it produces.
- LEFT OUT, the 2023 revision of art. 311(3) (approved by the Assembleia da República on 3 Aug 2023 and promulgated on 4 Aug 2023, per Centro de Integridade Pública): no law number, Boletim da República issue or text-layer copy was found. So the last version here is not today's text, and this act is not marked verified (no corpus.d/10-constituicao.PUBLISH).
- The official republication of 2018 (BR I Série n.º 115, 2.º suplemento, 12 Jun 2018) was found only as a scan with an automatic OCR layer (FAOLEX), so it is not used.

## What AI did and did not do

The fetch, parse and check scripts were written by Claude Code running Claude Opus 5.5 (`claude-opus-5-5`) under Lexifina's direction.

No model wrote, translated or corrected legal text. No model runs at build time. Every word of law in this repository comes from the official sources above, through code that anyone can rerun.

The commit messages are written by `build.py` from the source data.

Model use at data-build time:
- Claude Code with Claude Opus 5.5 (claude-opus-5-5) wrote fetch.py and parse.py and chose the sources. No model wrote or altered legal text, and no model runs at build time.
