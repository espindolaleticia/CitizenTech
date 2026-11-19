# Demanda por bicicletas compartilhadas
Global Solution de MLAM · FIAP · CitizenTech

## Integrantes

| Nome completo | RM |
|---|---|
| Felipe Mitsuo Takahashi Stephano | 570692 |
| Letícia Araújo Espindola | 569308 |
| Milena de Aguiar Lopes Cardoso | 570599 |

## Problema e objetivo

Quem opera um sistema de bicicletas compartilhadas precisa antecipar dias de maior procura. Investigamos duas perguntas com os dados do Capital Bikeshare: quantos aluguéis são esperados sob determinadas condições de temperatura e umidade? Qual é a probabilidade de o total do dia superar o patamar de alta demanda?

## Base de dados

O arquivo `day.csv` do *Bike Sharing Dataset* reúne 731 registros diários do Capital Bikeshare em 2011 e 2012. Cada linha representa um dia. Usamos `temp` (temperatura normalizada) e `hum` (umidade relativa, em escala de 0 a 1) para analisar `cnt` (total diário de aluguéis). Para a classificação, criamos uma variável binária a partir de `cnt`, usando como limite a mediana dos dias de treino.

[Base utilizada no Kaggle](https://www.kaggle.com/datasets/lakshmi25npathi/bike-sharing-dataset) · [Fonte original e dicionário de variáveis na UCI](https://archive.ics.uci.edu/dataset/275/bike+sharing+dataset)

## Arquivos do repositório

| Arquivo na pasta principal | Conteúdo |
|---|---|
| [day.csv](./day.csv) | Base diária utilizada em todas as análises. |
| [Global_Solution_MLAM.ipynb](./Global_Solution_MLAM.ipynb) | Notebook Python com cálculos, gráficos, modelos e saídas. |
| [Relatorio_Estatistico_MLAM.pdf](./Relatorio_Estatistico_MLAM.pdf) | Relatório com tabelas, figuras, interpretação dos resultados e limitações. |

## Como executar

1. Baixe o notebook e o arquivo `day.csv` pelos links acima.
2. No Google Colab, abra `Global_Solution_MLAM.ipynb`. Na barra lateral **Arquivos**, envie o `day.csv` para a pasta de trabalho da sessão. A primeira célula carrega a base com `pd.read_csv("day.csv")`.
3. Execute todas as células em ordem, do início ao fim. O notebook calcula as probabilidades e os testes de hipóteses antes de ajustar as regressões e avaliar a classificação.

O projeto usa **Python 3** e as bibliotecas **pandas**, **SciPy**, **Matplotlib**, **seaborn**, **scikit-learn** e **statsmodels**. Para executar localmente, salve o CSV e o notebook na mesma pasta e rode os comandos abaixo em um terminal nessa pasta:

```bash
python -m pip install jupyterlab pandas scipy matplotlib seaborn scikit-learn statsmodels
python -m jupyterlab Global_Solution_MLAM.ipynb
```

## Principais resultados

- **Probabilidades e testes de média.** Sob a hipótese de normalidade, a probabilidade de ficar abaixo da média de 4.504,35 aluguéis foi de 50%. A de superar a mediana observada de 4.548 foi de 49,10%. Os testes unilaterais com 20 e 25 dias não apresentaram evidência suficiente de uma média inferior ou superior à referência nos níveis de significância adotados.
- **Regressão linear.** A temperatura apresentou correlação positiva com os aluguéis (r = 0,6275). Usada sozinha, explicou 39,37% da variação observada. Ao incluir a umidade, o R² passou para 42,68%, um ganho de 3,31 pontos percentuais. Para temperatura normalizada de 0,60 e umidade de 70%, o modelo múltiplo estimou **5.045 aluguéis**.
- **Classificação de alta demanda.** Definimos alta demanda como mais de 4.586 aluguéis, a mediana dos 511 dias de treino. Nos 220 dias de teste, o modelo acertou 171 classificações (**77,73% de acurácia**). Identificou 74 dos 100 dias de alta demanda, deixou passar 26 e gerou 23 alertas falsos. Para o mesmo cenário de temperatura e umidade, a probabilidade estimada de alta demanda foi de **59,83%**.

## Limitações e uso dos resultados

Os dados abrangem uma única rede em 2011 e 2012. Os totais diários permitem avaliar a procura na rede inteira, mas não indicam a estação ou o horário em que podem faltar bicicletas. As relações observadas são associações e o R² das regressões descreve o ajuste aos dias analisados. A classificação foi avaliada por divisão aleatória do mesmo período. Para uso operacional, seria preciso testar o desempenho em datas posteriores. Como o modelo deixou passar 26 dias de alta demanda, seus alertas podem ser combinados com o histórico recente de aluguéis para apoiar o planejamento da oferta.
