# Estrutura / Structure

| Pasta | O que contém |
| --- | --- |
| `constituicao/` | A Constituição da República de Moçambique (2004; revista em 2007 e 2018) |
| `codigos/<codigo>/` | Códigos: `codigos/codigo-comercial/`, `codigos/codigo-penal/`, `codigos/codigo-de-processo-penal/`, ... O diploma que aprova o código fica numa subpasta (`lei-24-2019/`) |
| `leis/<ano>/<NNN>-<objecto>/` | Leis da Assembleia da República, pelo ano e número da lei, ex. `leis/2019/022-lei-familia/`, `leis/2023/013-lei-do-trabalho/` |
| `decretos-lei/<ano>/…`, `decretos/<ano>/…`, `decretos-presidenciais/<ano>/…` | Decretos-Leis, Decretos do Conselho de Ministros, Decretos Presidenciais |
| `diplomas-ministeriais/<ano>/…`, `diplomas-legislativos/<ano>/…`, `portarias/<ano>/…` | Diplomas Ministeriais; Diplomas Legislativos e Portarias (incluindo os do período colonial) |
| `resolucoes/`, `despachos/`, `avisos/`, `acordos-internacionais/` | Os restantes tipos, também por `<ano>/` |

Cada pasta de diploma tem um ficheiro por artigo (`art-012.md` é o artigo 12; `art-012-a.md` o 12-A), o
`preambulo.md`, `formula-final.md` (aprovação, promulgação, assinaturas) e um `README.md`. Quando o diploma aprova
outro texto com numeração própria (um regulamento, um estatuto), esse texto fica em `anexo-1/`, `anexo-2/`, …. Um
diploma sem artigos fica em `texto.md`. Nenhuma pasta tem mais de 1.000 entradas.

## Datas / Dates

Each act is one commit, as published, dated 12:00 Africa/Maputo on the day it took effect, repeated in its
`In-force:` trailer; `In-force-basis:` quotes the act's own entry-into-force clause, counted from the Boletim da
República date (`Published:`). Acts with no such clause, or one that cannot be dated, are held, not guessed. Acts in
force before 1970 are stamped 1970-01-01, with the real day in the subject and the `In-force:` trailer. No date in
this repository is the day we downloaded anything.

## Fontes / Sources

The Constitution comes from the Assembleia da República's editions; the other acts from PDF copies of the Boletim da
República: FAOLEX (FAO), the Assembleia da República, the Conselho Constitucional, the Banco de Moçambique and
Reformar. Copies not published by a Mozambican state body carry `Source-status: secondary copy (<source>, <URL>)`.
Text read from a scan's own OCR layer carries `Text-source: publisher OCR` (we do no OCR ourselves).

## Agora / Now

501 acts from the runner: 447 secondary copies, 103 with the publisher's own OCR layer.
