# WSL Data Analysis

🇺🇸 This README has an English version and a Portuguese version: [English](#english) is first, followed by [Português](#português).

🇧🇷 Este README tem uma versão em inglês e uma versão em português: a [English](#english) vem primeiro, seguida da [Português](#português).

---

## English

Data analysis and a predictive model for the **World Surf League (WSL) Championship Tour**, built as a project for the **Introduction to Data Science (ICdD)** course at PUC-Rio.

The project has two goals:

1. **Understand the domain**: explore 13 seasons of the tour (2010–2022) to answer questions about athletes, beaches and events.
2. **Predict the winner of an event**: train a model that estimates each surfer's score in an event and, from that, picks the winner and the podium.

### Repository structure

```
WSL-Data-Analysis/
├── WorldSurfLeague_Análise.ipynb   # Notebook with the whole project (EDA, adjustments and prediction)
├── DataSet/
│   └── df_complete.csv             # Local copy of the dataset
├── requirements.txt                # Jupyter environment dependencies
└── README.md
```

All the work is in a single notebook, written in Portuguese and organized into three sections.

#### 1. EDA (Exploratory Data Analysis)

Charts and tables used to understand the problem and in the project presentation. The questions explored include:

- Which athlete scored the most points each year? Which countries hold the most titles?
- *Regular* or *goofy* surfers: which stance is more common, and does that hold among champions?
- Is there an "ideal body" (age, height, weight) for a champion?
- Which year was the most contested (by standard deviation, coefficient of variation and the gap between 1st and 2nd)?
- Do athletes win more often at home?
- Which beaches had the most distinct winners?
- At which beaches does Gabriel Medina score more (or less) than the field average?

This section also creates the `praia` (beach) and `event_country` columns from the event name.

#### 2. Adjustments (Ajustes Pontuais)

Data cleaning and preparation done during development, such as per-season *min-max* normalization of scores and checking the computed champions against the official WSL rankings.

#### 3. Prediction (Predição)

All the predictive model code. This section keeps **the full history of how the project evolved over the sprints**, including attempts that did not work well, because they document the process. That is why it contains several training rounds:

| Stage | What changes |
|---|---|
| Initial training | Athlete history features (overall and per beach), exponential time-decay sample weights, *label encoding*; comparison of Linear Regression, KNN, Decision Tree and Random Forest |
| Feature engineering | New features: wave type (maneuver/barrel), wave direction (right/left/peak) and alignment with the athlete's stance |
| Hyperparameter tuning | Model tuning, including a *Bayes Search* round (`scikit-optimize`) |
| Modeling fixes | Target normalized within each event, to handle changes in the WSL scoring system, and train/test split **by event**, to avoid data leakage |
| Final metrics | Evaluation of the predicted ranking and comparison with a naive model that always picks Medina |

### How the model works

The model is a **regression**: it predicts an athlete's score in an **event** (a beach in a given year). It does **not** predict the season champion, which is decided by the sum of several events.

Inference:

1. Each of the 32 athletes in an event gets a predicted score.
2. The scores are sorted in descending order.
3. The athlete with the highest score is the predicted winner.

The final evaluation is done as **classification**, answering two questions:

- **Task 1**: was the predicted athlete actually the event winner?
- **Task 2**: did the predicted athlete at least make the podium (top 3)?

### Dataset

- **Source:** [WSL Dataset on Kaggle](https://www.kaggle.com/datasets/lucasreis95/wsldatasettest) (`lucasreis95/wsldatasettest`)
- **Size:** 4,419 rows, 145 athletes, 44 events, seasons 2010 to 2022
- **Granularity:** one row per athlete per event

| Column | Description |
|---|---|
| `athlete_name` | Athlete name |
| `year` | Season |
| `event_order` | Order of the event in the season calendar |
| `points_earned` | Points earned in the event |
| `event_name` | Official event name |
| `athlete_country` | Athlete's country |
| `athlete_profile_url` | Link to the WSL profile |
| `athlete_stance` | Stance: `Regular` or `Goofy` |
| `athlete_first_season` | First season on the CT |
| `athlete_age`, `athlete_height`, `athlete_weight` | Age, height (cm) and weight (kg) |
| `injuried_flag` | Whether the athlete was injured |

The notebook downloads the dataset automatically with `kagglehub`. If the download fails in your environment, use the copy in `DataSet/df_complete.csv` by changing the read line to:

```python
df = pd.read_csv("DataSet/df_complete.csv")
```

### How to run

**Requirement:** Python 3.11 or later (`pandas` 3.x requires it).

```bash
# 1. Clone the repository
git clone https://github.com/Pedro-Consales/WSL-Data-Analysis.git
cd WSL-Data-Analysis

# 2. Create and activate a virtual environment
python3 -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# 3. Install the dependencies
pip install -r requirements.txt
pip install matplotlib scikit-learn scikit-optimize kagglehub

# 4. Open the notebook
jupyter lab WorldSurfLeague_Análise.ipynb
```

Run the cells **in order, from top to bottom**, because later sections use columns and variables created in earlier ones.

The notebook also runs on **Google Colab**: open the `.ipynb` file and run the first cell, which installs `scikit-optimize` and downloads the dataset.

---

## Português

Análise de dados e modelo preditivo sobre o **Championship Tour da World Surf League (WSL)**, desenvolvidos como trabalho da disciplina de **Introdução à Ciência de Dados (ICdD)** da PUC-Rio.

O projeto tem dois objetivos:

1. **Entender o domínio**: explorar 13 temporadas do circuito (2010–2022) para responder perguntas sobre atletas, praias e campeonatos.
2. **Prever o vencedor de um evento**: treinar um modelo que estime a pontuação de cada surfista em uma etapa e, a partir disso, aponte o campeão e o pódio.

### Estrutura do repositório

```
WSL-Data-Analysis/
├── WorldSurfLeague_Análise.ipynb   # Notebook com todo o projeto (EDA, ajustes e predição)
├── DataSet/
│   └── df_complete.csv             # Cópia local do dataset
├── requirements.txt                # Dependências do ambiente Jupyter
└── README.md
```

Todo o trabalho está em um único notebook, organizado em três seções.

#### 1. EDA (Análise Exploratória)

Gráficos e tabelas usados para entender o problema e na apresentação do projeto. As perguntas investigadas incluem:

- Quem foi o atleta com mais pontos em cada ano? Quais países concentram os títulos?
- Surfistas *regular* ou *goofy*: qual base é mais comum, e ela se reflete entre os campeões?
- Existe um "corpo ideal" (idade, altura, peso) para ser campeão?
- Qual foi o ano mais disputado (por desvio padrão, coeficiente de variação e diferença entre 1º e 2º)?
- Atletas vencem mais quando competem em casa?
- Quais praias tiveram mais campeões diferentes?
- Em quais praias Gabriel Medina pontua mais (ou menos) do que a média?

Nesta seção também são criadas as colunas `praia` e `event_country` a partir do nome do evento.

#### 2. Ajustes Pontuais

Tratamento e preparação dos dados feitos ao longo do desenvolvimento, como a normalização *min-max* da pontuação por temporada e a conferência dos campeões calculados com a tabela oficial da WSL.

#### 3. Predição

Contém todo o código do modelo preditivo. A seção foi mantida **com o histórico completo da evolução do projeto ao longo das sprints**, incluindo tentativas que não deram bons resultados, porque elas documentam o processo. Por isso, ela tem várias rodadas de treino:

| Etapa | O que muda |
|---|---|
| Treinamento inicial | Features de histórico do atleta (geral e por praia), peso temporal com decaimento exponencial, *label encoding*; comparação entre Regressão Linear, KNN, Árvore de Decisão e Random Forest |
| Engenharia de atributos | Novas features: tipo de onda (manobra/tubo), direção da onda (direita/esquerda/pico) e alinhamento com a base do atleta |
| Otimização de hiperparâmetros | Ajuste dos modelos, incluindo uma rodada com *Bayes Search* (`scikit-optimize`) |
| Correções de modelagem | Alvo normalizado dentro de cada campeonato, para lidar com as mudanças no sistema de pontuação da WSL, e divisão treino/teste **por campeonato**, para evitar vazamento de dados |
| Métricas finais | Avaliação do ranking previsto e comparação com um modelo ingênuo que sempre aposta no Medina |

### Como o modelo funciona

O modelo é uma **regressão**: ele prevê a pontuação de um atleta em um **evento** (uma praia em um ano). Ele **não** prevê o campeão da temporada, que é decidido pela soma de vários eventos.

Inferência:

1. Os 32 atletas de um evento recebem uma pontuação prevista.
2. As pontuações são ordenadas em ordem decrescente.
3. O atleta com a maior pontuação é o campeão previsto.

A avaliação final é feita como **classificação**, respondendo a duas perguntas:

- **Task 1**: o atleta previsto foi de fato o campeão do evento?
- **Task 2**: o atleta previsto ficou pelo menos no pódio (top 3)?

### Dataset

- **Fonte:** [WSL Dataset no Kaggle](https://www.kaggle.com/datasets/lucasreis95/wsldatasettest) (`lucasreis95/wsldatasettest`)
- **Tamanho:** 4.419 registros, 145 atletas, 44 eventos, temporadas de 2010 a 2022
- **Granularidade:** uma linha por atleta por evento

| Coluna | Descrição |
|---|---|
| `athlete_name` | Nome do atleta |
| `year` | Temporada |
| `event_order` | Ordem da etapa no calendário |
| `points_earned` | Pontos obtidos no evento |
| `event_name` | Nome oficial do evento |
| `athlete_country` | País do atleta |
| `athlete_profile_url` | Link para o perfil na WSL |
| `athlete_stance` | Base: `Regular` ou `Goofy` |
| `athlete_first_season` | Primeira temporada no CT |
| `athlete_age`, `athlete_height`, `athlete_weight` | Idade, altura (cm) e peso (kg) |
| `injuried_flag` | Indica se o atleta estava lesionado |

O notebook baixa o dataset automaticamente com `kagglehub`. Se o download não funcionar no seu ambiente, use a cópia em `DataSet/df_complete.csv`, trocando a leitura por:

```python
df = pd.read_csv("DataSet/df_complete.csv")
```

### Como executar

**Pré-requisito:** Python 3.11 ou superior (o `pandas` 3.x exige essa versão).

```bash
# 1. Clonar o repositório
git clone https://github.com/Pedro-Consales/WSL-Data-Analysis.git
cd WSL-Data-Analysis

# 2. Criar e ativar um ambiente virtual
python3 -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# 3. Instalar as dependências
pip install -r requirements.txt
pip install matplotlib scikit-learn scikit-optimize kagglehub

# 4. Abrir o notebook
jupyter lab WorldSurfLeague_Análise.ipynb
```

Execute as células **em ordem, de cima para baixo**, porque as seções posteriores usam colunas e variáveis criadas nas anteriores.

O notebook também roda no **Google Colab**: basta abrir o arquivo `.ipynb` e executar a primeira célula, que instala o `scikit-optimize` e baixa o dataset.
