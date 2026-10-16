# Desempenho ENEM 2021

[![App online](https://img.shields.io/badge/App%20online-Railway-7c3aed?logo=railway&logoColor=white)](https://portfolio-app-production-d745.up.railway.app/enem)  
> 🚀 **Demo interativa:** https://portfolio-app-production-d745.up.railway.app/enem

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/emmanuelvrm/desempenho-enem-2021/blob/main/notebooks/Desempenho_ENEM.ipynb)
![Python](https://img.shields.io/badge/Python-3.10-blue) ![PyCaret](https://img.shields.io/badge/PyCaret-3.x-orange) ![License](https://img.shields.io/badge/license-MIT-green)

Previsão da **média das notas do ENEM 2021** (`NU_MEDIA`) a partir de dados socioeconômicos, com análise exploratória, tratamento de multicolinearidade e comparação de modelos de regressão.

## Sobre o projeto

Meu objetivo neste projeto é explorar alternativas de regressão linear e criar o melhor modelo possível para prever o desempenho de alunos com base em dados socioeconômicos do ENEM (Exame Nacional do Ensino Médio) 2021.

Para isso, utilizarei um conjunto de dados estratificado por estado, representando 10% do total de candidatos inscritos no ENEM. Os dados completos podem ser obtidos no Portal de Dados Abertos do INEP <a href="https://www.gov.br/inep/pt-br">(Instituto Nacional de Estudos e Pesquisas Educacionais Anísio Teixeira)</a>. O conjunto de dados é baseado em duas fontes de informação: o <a href="https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/microdados/enem">questionário socioeconômico</a>, respondido pelos candidatos durante o processo de inscrição em 2021, e as notas obtidas pelos candidatos em cinco áreas do exame: redação, matemática, linguagens e códigos, ciências humanas e ciências da natureza.

A variável-alvo (ou target) deste projeto é a variável NU_MEDIA, que representa a média aritmética das cinco notas obtidas pelos candidatos no ENEM. Meu objetivo é desenvolver um modelo de regressão capaz de prever essa média com base nas informações socioeconômicas dos alunos.

## Metodologia

1. **Análise univariada** (faixa etária, sexo, cor/raça, escolaridade e ocupação dos pais, renda, bens da residência) e **bivariada** (relação com `NU_MEDIA`).
2. **Multicolinearidade:** cálculo do VIF e remoção de variáveis com valores altos (`TP_COR_RACA`, `TP_ANO_CONCLUIU`, banheiros e quartos da residência).
3. **PCA** para entender a contribuição relativa das variáveis.
4. **Modelagem com PyCaret:** `compare_models` e experimentos com outras abordagens; o modelo final é um **LightGBM** (gradient boosting).

## Resultados (modelo final)

| Métrica | Valor |
| ------- | ----- |
| MAE     | 57,61 |
| RMSE    | 72,99 |
| R²      | 0,3244 |
| RMSLE   | 0,1408 |
| MAPE    | 0,1118 |

**Leitura honesta dos resultados:** o erro médio é de cerca de 58 pontos e o modelo explica ~32% da variância da média. Ou seja, as variáveis socioeconômicas carregam sinal relevante sobre o desempenho, mas **sozinhas não bastam** para prever a nota com precisão. Os resultados valem para os dados do ENEM 2021 e podem não se generalizar para outros anos ou populações.

> Nota: o texto original do notebook descreve o modelo final como "regressão linear"; na prática ele é um LightGBM (gradient boosting).

## Estrutura do repositório

```
desempenho-enem-2021/
├── data/
│   └── Base_de_dados_do_Enem.csv      # amostra estratificada por UF (10% dos inscritos)
├── docs/
│   └── Dicionario_de_dados_ENEM.xlsx
├── model/
│   └── previsao_enem.pkl              # pipeline PyCaret salvo
├── notebooks/
│   └── Desempenho_ENEM.ipynb
├── requirements.txt
└── README.md
```

## Como executar

```bash
git clone https://github.com/emmanuelvrm/desempenho-enem-2021.git
cd desempenho-enem-2021
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook notebooks/Desempenho_ENEM.ipynb
```

PyCaret 3.x exige **Python 3.9 a 3.11**. No Google Colab use o botão no topo (os dados são baixados do GitHub). Versão original do notebook no Colab: [link](https://colab.research.google.com/drive/1vYNjmrwPc3VPJZnTKryKSlAftb8TmiNY?usp=sharing).

### Usando o modelo salvo

```python
from pycaret.regression import load_model
modelo = load_model("model/previsao_enem")   # sem a extensão .pkl
```

> O `.pkl` é um arquivo *pickle*: só carregue arquivos desse tipo de fontes em que confia, e use uma versão do PyCaret/scikit-learn compatível com a que o gerou.

## Fonte dos dados

Microdados do ENEM 2021 do [INEP](https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/microdados/enem) (dados abertos).

## Licença

Código e notebooks sob licença MIT (veja `LICENSE`).
