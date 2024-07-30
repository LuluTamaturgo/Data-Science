# Análise gastos mensais : periodo  Dezembro 2022 - Ano 2023

Neste projeto realizamos uma análise exploratória, tendo como base o histórico de compras para mercado dentro de um periodo, sendo Dezembro de 2022 e o ano de 2023. Abaixo temos dois conjuntos de dados:

1️⃣ **Descrição consumo mensal:** Coletamos dados que nos apresentam o valor total gasto na compra por mês composta pelas categorias: Valor_Mercado, Cartao_Alimentacao, Valor_Pago e Cartao_Natal.

2️⃣ **Descrição consumo mensal por categorias:** Contém informações sobre os gastos obtidos nas compras mensais subdivididos por categorias: Suprimentos, Higiene, Carnes_Verduras, Lanches, Limpeza, Cigarro e Extras


* Importando as bibliotecas utilizadas:

~~~~python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px
~~~~

* Criando as variáveis que receberá os arquivos com os dados para analisarmos:
~~~~python
lista_mercado_cm = pd.read_csv('/content/drive/MyDrive/Colab Notebooks/Projetos/Consumo mercado /lista_mercado_cm.txt')
lista_mercado_categoria = pd.read_csv('/content/drive/MyDrive/Colab Notebooks/Projetos/Consumo mercado /lista_mercado_categoria.txt')
~~~~


## 1.1 Descrição : Gasto mensal mercado - periodo: Dezembro 2022 e Ano 2023

~~~~python
lista_mercado_cm
~~~~

![Teste](C:\Users\Usuário\Pictures\df_dataframe.png)




