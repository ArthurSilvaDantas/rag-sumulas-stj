# Resultados

Exemplos reais de saída e placares dos gabaritos. As saídas completas estão nos notebooks 05, 06 e 07.

Saídas reais de uma execução em 30/10/2026, com enunciados e respostas longos encurtados (`…`). O exemplo 1 é a ementa
do pedido original. As ementas dos exemplos 2 a 4 foram escritas para teste e não são decisões reais.

Cada avaliação traz a relação e o tipo da justificativa: **jurídica**, quando a resposta depende de comparar regras ou
matérias, ou **fática**, quando depende de um fato concreto do caso.

A aderência é o cosseno entre os vetores: **forte** a partir de 0,65 e **moderada** de 0,50 a 0,65. A ordem das listas
vem da busca híbrida, por isso as aderências nem sempre aparecem em ordem decrescente.

## 1. Ementa com súmula em consonância

**Consulta:** "Verifique se exista alguma sumula em consonancia com a ementa abaixo: PRISÃO PREVENTIVA. PORTE ILEGAL DE
ARMA DE FOGO DE USO PERMITIDO, TRÁFICO DE DROGAS E ASSOCIAÇÃO PARA O TRÁFICO. […] não é possível a decretação da prisão
preventiva de ofício em face do que dispõe a Lei n. 13.964/2019 […]"

**Súmulas**

| Súmula | Aderência | Enunciado |
|---|---|---|
| **676/STJ** | **0,698 (forte)** | Em razão da Lei n. 13.964/2019, não é mais possível ao juiz, de ofício, decretar ou converter prisão em flagrante em prisão preventiva. |
| 409/STJ | 0,604 (moderada) | Em execução fiscal, a prescrição ocorrida antes da propositura da ação pode ser decretada de ofício (art. 219, § 5º, do CPC). |
| 668/STJ | 0,532 (moderada) | Não é hediondo o delito de porte ou posse de arma de fogo de uso permitido, ainda que com numeração, marca ou qualquer outro sinal de identificação raspado… |
| 267/STJ | 0,603 (moderada) | A interposição de recurso, sem efeito suspensivo, contra decisão condenatória não obsta a expedição de mandado de prisão. |
| 630/STJ (alterada) | 0,547 (moderada) | A incidência da atenuante da confissão espontânea no crime de tráfico ilícito de entorpecentes quando o acusado admitir a posse ou a propriedade para uso próprio… |

**Outras seções**

| Seção | Precedente | Aderência | Situação |
|---|---|---|---|
| Com tese firmada | Tema 134/STJ (execução fiscal) | 0,649 | Trânsito em Julgado |
| Com tese firmada | Tema 1347/STJ (regressão cautelar de regime) | 0,618 | Trânsito em Julgado |
| Com tese firmada | Tema 1167/STJ (audiência do art. 16 da Lei 11.340/2006) | 0,610 | Trânsito em Julgado |
| Com tese firmada | Tema 1098/STJ (acordo de não persecução penal) | 0,606 | Trânsito em Julgado |
| Com tese firmada | Tema 1259/STJ (majorante de arma no tráfico) | 0,552 | Trânsito em Julgado |
| Em andamento | Tema 1431/STJ (solicitação de droga em presídio) | 0,666 | Em Julgamento |
| Inativo (não usar) | Tema 583/STJ (liberdade provisória no tráfico) | 0,698 | Cancelado |
| Inativo (não usar) | Controvérsia 438/STJ (prisão em flagrante por guarda municipal) | 0,653 | Vinculada a Tema |

Só a Súmula 676 trata da questão. Os demais entram por termos em comum, como "de ofício" ou "arma de fogo", o que mostra
por que aderência não é consonância.

**Avaliação dos modelos**

| Súmula | Esperado | `llama2:7b` | `llama3.1:8b` |
|---|---|---|---|
| 676/STJ | CONSONANTE | CONSONANTE, jurídica | CONSONANTE, jurídica |
| 409/STJ | SEM CONSONÂNCIA | CONSONANTE (erro), jurídica | SEM CONSONÂNCIA, fática (tipo errado) |
| 668/STJ | SEM CONSONÂNCIA | CONSONANTE (erro), jurídica | SEM CONSONÂNCIA, jurídica |

Justificativas do `llama3.1:8b`:

- **676/STJ:** "A ementa trata da impossibilidade de decretação de prisão preventiva de ofício, o que é confirmado pelo
  precedente que também menciona a Lei n. 13.964/2019 como base para essa regra."
- **668/STJ:** "A ementa não trata da regra do precedente, pois trata de prisão preventiva e porte ilegal de arma de
  fogo de uso permitido, enquanto o precedente trata especificamente da hediondez do delito de porte ou posse de arma de
  fogo de uso permitido."

Justificativa do `llama2:7b` para a 409/STJ, que trata de execução fiscal: "A decisão do precedente é baseada na
interpretação da Lei n. 13.964/2019, que estabelece que a decretação da prisão preventiva de ofício é possível apenas
após o ocorrência de audiência de custódia…". O precedente não diz nada disso, e a lei foi citada de forma incorreta.

## 2. Ementa que contraria a súmula

**Consulta:** "PRISÃO PREVENTIVA. CONVERSÃO DE OFÍCIO DA PRISÃO EM FLAGRANTE. POSSIBILIDADE. O juiz pode, sem
requerimento do Ministério Público ou representação da autoridade policial, converter de ofício a prisão em flagrante
em prisão preventiva, mesmo após a Lei n. 13.964/2019."

A Súmula 676 volta em 1º lugar e com aderência **maior (0,824)** que no exemplo 1, mesmo dizendo o contrário da ementa.
Quem separa os dois casos é a avaliação do modelo:

| Súmula | Esperado | `llama2:7b` | `llama3.1:8b` |
|---|---|---|---|
| 676/STJ | DIVERGENTE | CONSONANTE (erro), jurídica | **DIVERGENTE**, jurídica |
| 409/STJ | SEM CONSONÂNCIA | CONSONANTE (erro), jurídica | SEM CONSONÂNCIA, jurídica |
| 635/STJ | SEM CONSONÂNCIA | CONSONANTE (erro), jurídica | SEM CONSONÂNCIA, jurídica |

Justificativa do `llama3.1:8b` para a 676/STJ: "A ementa afirma que o juiz pode converter prisão em flagrante em prisão
preventiva de ofício, mesmo após a Lei n. 13.964/2019, o que contraria a regra do precedente que afirma que não é mais
possível converter prisão em flagrante em prisão preventiva de ofício após a referida lei."

O `llama2:7b` se contradiz: rotula a 676 como CONSONANTE e conclui na própria justificativa que "a regra do precedente
não se aplica neste caso".

## 3. Outra área do direito

**Consulta:** "Banco responde objetivamente pelos danos decorrentes de golpe praticado por estelionatário em conta do
correntista, por se tratar de fortuito interno."

| Súmula | Aderência | Assunto | Esperado | `llama2:7b` | `llama3.1:8b` |
|---|---|---|---|---|---|
| **479/STJ** | **0,757 (forte)** | Fraudes de terceiros em operações bancárias | CONSONANTE | CONSONANTE | CONSONANTE |
| 475/STJ | 0,593 (moderada) | Protesto indevido por endossatário | SEM CONSONÂNCIA | CONSONANTE (erro) | SEM CONSONÂNCIA |
| 130/STJ | 0,595 (moderada) | Furto de veículo em estacionamento | SEM CONSONÂNCIA | CONSONANTE (erro) | SEM CONSONÂNCIA |

Todas as respostas vieram como jurídicas. Justificativa do `llama3.1:8b` para a 130/STJ: "A ementa trata de
responsabilidade do banco por danos decorrentes de golpe, enquanto o precedente trata de responsabilidade da empresa
por furto de veículo. A ementa decide que o banco responde pelos danos, o que é diferente da regra do precedente."

A busca também encontrou o Tema 466/STJ (0,649), com tese igual à da Súmula 479, e a Controvérsia 710/STJ (0,595),
ainda pendente, sobre o dever do banco de detectar fraude.

## 4. Justificativa fática

**Consulta:** "Estupro de vulnerável afastado. A vítima tinha quinze anos completos à época dos fatos e não apresentava
enfermidade ou deficiência, conforme laudo e certidão de nascimento juntados aos autos."

Aqui a regra da Súmula 593/STJ (aderência 0,681) é do mesmo assunto, e o que afasta a aplicação é um fato: a idade da
vítima. O `llama3.1:8b` respondeu **SEM CONSONÂNCIA, fática**:

> A ementa trata de uma vítima com 15 anos, enquanto o precedente se refere a menores de 14 anos. A idade da vítima é o
> fator determinante para a configuração do crime de estupro de vulnerável.

Essa resposta recebeu um alerta falso de "números sem lastro" porque a ementa escreve "quinze" por extenso. O alerta
pede conferência, não indica necessariamente erro. O `llama2:7b` respondeu CONSONANTE, jurídica, e escreveu a
justificativa quase toda em inglês.

## Placar nos gabaritos do notebook 07

| Modelo | Relação (11 casos) | Com justificativa (11 casos) | Tipo da justificativa (8 casos) |
|---|---|---|---|
| `llama2:7b` | 3, sempre CONSONANTE | 11 | 5, sempre jurídica |
| `llama3.1:8b` | 10 | 11 | 5 |

Prompt e definições foram escolhidos olhando esses mesmos casos, então o placar é otimista. No tipo, o `llama3.1:8b`
acertou 4 dos 5 casos jurídicos e só 1 dos 3 fáticos.
