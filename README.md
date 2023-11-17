# HorrorMoviesDataAnalyis

Este documento descreve uma análise exploratória de dados sobre filmes de terror, utilizando a linguagem de programação Python e diversas bibliotecas, como NumPy, Pandas, Seaborn, Matplotlib, Plotly, e Scikit-Learn.

## 1. Bibliotecas Utilizadas

import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
import plotly.express as px
from sklearn.preprocessing import StandardScaler

## 2. Carregamento dos Dados

# Carregamento dos dados
df = pd.read_csv('./Datasets/horror_movies.csv', sep=',')
bkp = pd.read_csv('./Datasets/horror_movies.csv', sep=',')
df.head()

## 2.1 Explorando e Entendendo os Dados

# Resumo estatístico das colunas 'popularity', 'budget', 'revenue'
df[['popularity', 'budget', 'revenue']].describe()
## 2.2 Visualizando a Distribuição do Orçamento, Receita e Popularidade

# Visualização da distribuição usando Plotly
import plotly.graph_objects as go
from plotly.subplots import make_subplots

fig = make_subplots(rows=3, cols=1, subplot_titles=(
    'Distribuição do orçamento', 'Distribuição da receita', 'Distribuição da popularidade'))

# Gráficos de distribuição
fig.add_trace(go.Histogram(
    x=df['budget'], name='Orçamento', marker_color='blue', nbinsx=50), row=1, col=1)
fig.add_trace(go.Histogram(
    x=df['revenue'], name='Receita', marker_color='green', nbinsx=50), row=2, col=1)
fig.add_trace(go.Histogram(
    x=df['popularity'], name='Popularidade', marker_color='red', nbinsx=50), row=3, col=1)

# Layout do gráfico
fig.update_layout(height=800, width=800,
                  title_text="Distribuição do orçamento, receita e popularidade", showlegend=False)
fig.update_xaxes(title_text="Orçamento", row=1, col=1)
fig.update_xaxes(title_text="Receita", row=2, col=1)
fig.update_xaxes(title_text="Popularidade", row=3, col=1)
fig.update_yaxes(title_text="Quantidade", row=1, col=1)
fig.update_yaxes(title_text="Quantidade", row=2, col=1)
fig.update_yaxes(title_text="Quantidade", row=3, col=1)

# Mostrar o gráfico
fig.show()

# 3. Processamento dos Dados

## 3.1 Contando Dados Faltantes e Valores Duplicados

# Contando dados faltantes
df[['popularity', 'budget', 'revenue']].isnull().sum()

# Checando valores duplicados
df.duplicated().sum()

## 3.2 Normalização dos Dados

# Normalizando as colunas 'budget', 'revenue' e 'popularity'
scaler = StandardScaler()
df[['budget', 'revenue', 'popularity']] = scaler.fit_transform(
    df[['budget', 'revenue', 'popularity']])
    
## 3.3 Conversão do Formato de Data

# Convertendo para datetime
df['release_date'] = pd.to_datetime(df['release_date'])

# Criando a coluna com o mês de lançamento
df['release_month'] = df['release_date'].dt.month
4. Análise dos Dados

## 4.1 Correlação entre Receita e Orçamento

# Verificando a correlação entre a receita e o orçamento da coleção de filmes
correlation_matrix = df[['budget', 'revenue', 'popularity']].corr()
fig1 = px.scatter(df, x='budget', y='revenue', color='popularity', title='Relação de receita e orçamento por popularidade')
fig1.show()

##4.2 Correlação entre Receita, Orçamento e Popularidade
# Verificando a correlação entre a receita, orçamento e a popularidade
correlacao = df[['budget', 'revenue', 'popularity']].corr()
print(correlacao)

## 4.3 Filmes Mais Populares

# Filmes mais populares
top_filmes = pd.DataFrame(df.groupby('original_title')[['overview', 'popularity']].sum(
).sort_values('popularity', ascending=False).round(2).head(10))

fig = px.bar(top_filmes, x=top_filmes.index, y='popularity',
             title='Popularidade ', template='seaborn', color=top_filmes.index, text='popularity')
fig.show()
