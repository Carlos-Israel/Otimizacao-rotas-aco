# 🐜 Otimização de Rotas de Vistorias Rodoviárias no Pará (ACO) - Pará

Este projeto implementa o **Algoritmo de Colônia de Formigas** (ACO), uma meta-heurística bioinspirada, para resolver um problema de **Caixeiro Viajante (TSP)** com restrição de *depot*. O objetivo é determinar a rota de menor distância que visita 15 pontos de fiscalização nas rodovias federais do Pará, iniciando e retornando à capital, Belém.

---

## Contexto do Problema

Um órgão de fiscalização rodoviária precisa planejar a rota mais eficiente para visitar 15 pontos em rodovias federais do Pará, iniciando e retornando à capital, Belém. A solução implementa o ACO clássico, que utiliza **feromônio**, **visibilidade** e **evaporação** para guiar a busca da melhor sequência de paradas (nós) com base em uma matriz de distâncias pré-calculada.

---

## Setup e Instruções de Execução

O projeto é executado através do `notebook.ipynb` e requer as seguintes bibliotecas Python:

### Requisitos

```bash
pip install pandas numpy geopandas matplotlib folium shapely
```

### Execução
1 - Clone o repositório.

2 - Abra o arquivo `notebook.ipynb` no Jupyter ou Google Colab.

3 - Execute todas as células sequencialmente. 

O notebook é **autocontido**, baixa os dados automaticamente e executa os **3 testes de ACO**.

### Resumo da Solução Implementada

##### Modelagem do Custo (Função Objetivo)

O custo da rota é a soma das distâncias. A solução modela o ciclo completo:

$$
\text{Rota Completa} = \text{Belém} \to n_1 \to \dots \to n_{15} \to \text{Belém}
$$

  * O ponto de partida **($n_1$)** é o posto de vistoria mais próximo de Belém, encontrado por cálculo de **Haversine**.
  * O custo total inclui o circuito interno ($n_i \to n_j$) da matriz `brs-pa-distances.csv` mais as duas pernas de Belém (saída e retorno).

### Parâmetros-Chave do ACO
| Parâmetro | Descrição            | Importância                                                |
| --------- | -------------------- | ---------------------------------------------------------- |
| **α**     | Peso do Feromônio    | Favorece rotas já exploradas (memória do sistema)          |
| **β**     | Peso da Visibilidade | Favorece escolhas de vizinhos mais próximos (busca gulosa) |
| **ρ**     | Taxa de Evaporação   | Evita estagnação limpando feromônio antigo                 |

### Descrição da Experimentação e Análise

Foram realizados 3 testes, cada um com 5 execuções, variando os parâmetros 𝛼, β e o número de formigas (m), para avaliar consistência e velocidade de convergência.
| Teste | Objetivo Principal           | α   | β   | Formigas (m) | Resultado Esperado                          |
| ----- | ---------------------------- | --- | --- | ------------ | ------------------------------------------- |
| **1** | Baseline (Equilíbrio)        | 1.5 | 1.7 | 30           | Bom equilíbrio entre qualidade e tempo      |
| **2** | Foco no Feromônio (História) | 1.7 | 0.5 | 30           | Convergência rápida, risco de ótimos locais |
| **3** | Busca Gulosa + População     | 1.0 | 2.5 | 50           | Exploração agressiva por caminhos curtos    |

