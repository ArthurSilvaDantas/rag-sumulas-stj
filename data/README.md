# Dados

Os arquivos desta pasta não vão para o repositório. Eles são baixados ou gerados pelos notebooks 01 a 03, que levam
poucos segundos depois do download.

```
data/
├── raw/          fontes oficiais, sem alteração (notebook 01)
├── interim/      texto extraído e limpo (notebook 02)
└── processed/    registros prontos para indexar (notebook 03)
```

## Fontes

| Arquivo | Origem | Licença | Observação |
|---|---|---|---|
| `raw/VerbetesSTJ.pdf` | [Enunciados das Súmulas do STJ](https://scon.stj.jus.br/docs_internet/jurisprudencia/tematica/download/SU/Verbetes/VerbetesSTJ.pdf) | Documento público do STJ | Versão usada: gerada em 05/12/2025, até a Súmula 676 |
| `raw/temas.csv` | [Dados Abertos do STJ, precedentes qualificados](https://dadosabertos.web.stj.jus.br/dataset/precedentes-qualificados) | CC-BY | Temas, Controvérsias, IAC, PUIL e SIRDR |

Existe uma cópia antiga do PDF em `www.stj.jus.br/docs_internet/VerbetesSTJ.pdf` que para na Súmula 673. O notebook 01
usa a URL do SCON, que é a atualizada. Para baixar as fontes de novo, mude `ATUALIZAR = True` no notebook 01.

## `interim/sumulas_limpas.jsonl`

Uma linha por súmula, 676 no total.

| Campo | Exemplo | Descrição |
|---|---|---|
| `numero` | `630` | Número da súmula |
| `marca` | `"SÚMULA ALTERADA"` | Marca do cabeçalho no PDF, ou `null` se não houver |
| `enunciado` | `"A incidência da atenuante…"` | Redação em vigor |
| `citacao` | `"(TERCEIRA SEÇÃO, julgado em 10/9/2025, …)"` | Órgão e datas de julgamento e publicação |
| `historico` | `"SÚMULA REVISADA: … REDAÇÃO ANTERIOR: …"` | Redação anterior e notas, separadas para não entrar no índice |

## `interim/temas_limpos.jsonl`

Uma linha por precedente qualificado com tese ou questão, 2.369 no total.

| Campo | Descrição |
|---|---|
| `tipo` | `Tema`, `Controvérsia`, `IAC`, `PUIL`, `SIRDR` ou vazio |
| `numero` | Número do precedente |
| `situacao` | Situação informada pelo STJ, como `Trânsito em Julgado` ou `Afetado` |
| `tese` | Tese firmada, vazia enquanto o julgamento não termina |
| `questao` | Questão submetida a julgamento |
| `orgao` | Órgão julgador |
| `entendimento_anterior` | Entendimento superado, quando houver |
| `assuntos` | Assuntos cadastrados |

## `processed/precedentes.jsonl`

Junta as duas fontes em 3.045 registros com o mesmo formato. Cada registro vira um ponto no Qdrant, e todos os campos
ficam no payload.

| Campo | Exemplo | Descrição |
|---|---|---|
| `id` | `"stj:sumula:676"` | Identificador estável, usado para gerar o id do ponto |
| `citacao` | `"Súmula 676/STJ"` | Forma de citação exibida |
| `fonte` | `"sumula"` ou `"precedente"` | Origem do registro |
| `tipo` | `"Súmula"`, `"Tema"`… | Tipo do precedente |
| `numero` | `676` | Número |
| `enunciado` | `"Em razão da Lei n. 13.964/2019…"` | Texto comparado com a ementa: enunciado da súmula, tese ou, sem tese, a questão |
| `questao` | | Questão submetida, só em precedentes |
| `situacao` | `"vigente"`, `"cancelada"`, `"Afetado"`… | Situação do registro |
| `vigente` | `true` | `false` para súmulas canceladas ou revogadas e precedentes cancelados, prejudicados ou absorvidos |
| `tem_tese` | `true` | `false` para precedentes ainda sem tese firmada |
| `referencia` | | Citação da súmula ou órgão julgador do precedente |
| `historico` | | Redação ou entendimento anterior |
| `assuntos` | | Assuntos cadastrados, só em precedentes |
| `url` | | Link para a súmula no SCON ou para o tema repetitivo |

| Grupo | Registros |
|---|---|
| Súmulas vigentes (inclui 5 alteradas) | 646 |
| Súmulas canceladas ou revogadas | 30 |
| Precedentes vigentes com tese | 1.185 |
| Precedentes vigentes em andamento | 169 |
| Precedentes inativos | 1.015 |
