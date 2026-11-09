# Notebooks

Execute na ordem. Cada etapa grava o que a seguinte lê, então só é preciso voltar a uma etapa quando algo antes dela
mudar. Os notebooks são salvos com as saídas da última execução, para que os resultados possam ser lidos no GitHub
sem rodar nada.

| Notebook | Entrada | Saída | Precisa de | Tempo aproximado |
|---|---|---|---|---|
| `01_coleta_fontes` | URLs do STJ | `data/raw/` | Internet | Segundos |
| `02_extracao_limpeza` | `data/raw/` | `data/interim/` | | Segundos |
| `03_estruturacao_registros` | `data/interim/` | `data/processed/precedentes.jsonl` | | Segundos |
| `04_indexacao_qdrant` | `precedentes.jsonl` | Coleção `stj_precedentes` | Qdrant, `bge-m3` | 1 minuto |
| `05_busca_consonancia` | Uma ementa | Candidatos por seção | Qdrant, `bge-m3` | Segundos |
| `06_justificativa_llama` | Uma ementa | Relação e justificativa por candidato | Qdrant, `bge-m3`, Llama | 3 minutos |
| `07_avaliacao` | Gabaritos escritos à mão | Placares da busca e dos modelos | Qdrant, `bge-m3`, os dois Llamas | 15 minutos |

Os tempos foram medidos em um Mac com Apple Silicon e variam com a máquina.

## Como os notebooks se ligam

O 06 carrega as funções do 05 com `%run ./05_busca_consonancia.ipynb`, e o 07 carrega o 06 da mesma forma. Para não
repetir os exemplos do notebook carregado, eles definem antes uma variável:

| Variável | Definida em | Efeito |
|---|---|---|
| `EXEMPLOS_BUSCA = False` | 06 | O 05 não roda o exemplo de busca |
| `EXEMPLOS_JUSTIFICATIVA = False` | 07 | O 06 não roda o exemplo com os modelos |

Por isso o Jupyter precisa ser aberto com a pasta `notebooks/` como diretório de trabalho, o que acontece normalmente
ao abrir os arquivos pelo Jupyter Lab.

## Configurações

As configurações ficam no início de cada notebook.

| Variável | Notebook | Padrão | Uso |
|---|---|---|---|
| `ATUALIZAR` | 01 | `False` | `True` baixa as fontes de novo |
| `RECRIAR` | 04 | `True` | `False` mantém a coleção e só atualiza os pontos |
| `QDRANT_URL` | 04, 05 | `http://localhost:6333` | Endereço do Qdrant |
| `MODELO_DENSO` | 04, 05 | `bge-m3` | Modelo de embedding no Ollama |
| `ADERENCIA_FORTE`, `ADERENCIA_MINIMA` | 05 | `0.65`, `0.50` | Limiares de aderência |
| `MODELO_LLM` | 06 | `llama2:7b` | Modelo que justifica |
| `MODELO_COMPARACAO` | 06 | `llama3.1:8b` | Modelo usado na comparação |

## Consultar outra ementa

No 05, só a busca:

```python
mostrar(consonancia("Verifique se existe súmula em consonância com a ementa abaixo: ..."))
```

No 06, busca e justificativa:

```python
pareceres = analisar("...", modelo="llama3.1:8b", sumulas=3, precedentes=2)
```

`analisar` devolve uma lista de dicionários com `citacao`, `relacao`, `trecho`, `tipo_justificativa`, `justificativa`,
`alertas` e `segundos`.
