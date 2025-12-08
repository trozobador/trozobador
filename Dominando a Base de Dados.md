# 🎯 Fase 1: Dominando a Base de Dados (1-2 meses)

# 📊 Visão Geral da Fase 1

Esta é a fase mais importante da sua jornada em Data Science! Dominar manipulação e análise de dados é fundamental antes de avançar para Machine Learning ou IA. Você vai aprender as ferramentas que os cientistas de dados usam diariamente.

**Objetivo:** Tornar-se proficiente em manipulação, limpeza e análise exploratória de dados usando Python, com foco em Pandas, NumPy e PySpark no Databricks.

**Duração:** 1-2 meses (dependendo da dedicação semanal)

**Por que esta fase é crítica:**

- 80% do trabalho de Data Science é preparação de dados
- Sem dominar dados, você não conseguirá treinar modelos eficazes
- Essas habilidades são transferíveis para qualquer projeto de dados

---

# 🐼 Pandas: Excel com Esteróides

## O que é Pandas?

Pandas é a biblioteca Python mais popular para manipulação e análise de dados tabulares. É como ter Excel/Power Query com superpoderes programáticos.

**Por que Pandas?**

- Manipula milhões de linhas facilmente (Excel trava com ~1M)
- Operações vetorizadas extremamente rápidas
- Integra perfeitamente com ecossistema Python de Data Science
- Reprodutível (código vs. cliques manuais)

## 🚀 Como Começar com Pandas

### Instalação

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

### Setup do Ambiente

```bash
# Criar ambiente virtual
python -m venv venv-datascience
source venv-datascience/bin/activate  # Linux/Mac
# ou
venv-datascience\Scripts\activate  # Windows

# Instalar dependências
pip install pandas numpy matplotlib seaborn jupyter openpyxl

# Iniciar Jupyter Notebook
jupyter notebook
```

## 📚 Conceitos Fundamentais do Pandas

### 1. Estruturas de Dados

**Series (1D):**

```python
import pandas as pd
import numpy as np

# Series é como uma coluna do Excel
vendas = pd.Series([1000, 1500, 1200, 1800, 2000])
print(vendas)

# Com índices customizados
vendas = pd.Series(
    [1000, 1500, 1200, 1800, 2000],
    index=['Jan', 'Fev', 'Mar', 'Abr', 'Mai']
)
print(vendas['Mar'])  # 1200
```

**DataFrame (2D):**

```python
# DataFrame é como uma tabela Excel
df = pd.DataFrame({
    'Nome': ['João', 'Maria', 'Pedro', 'Ana'],
    'Idade': [28, 34, 23, 45],
    'Cidade': ['São Paulo', 'Rio de Janeiro', 'Belo Horizonte', 'Brasília'],
    'Salário': [5000, 7500, 4200, 9000]
})

print(df)
print(df.head())  # Primeiras 5 linhas
print([df.info](http://df.info)())  # Informações sobre colunas
print(df.describe())  # Estatísticas descritivas
```

### 2. Leitura e Escrita de Dados

```python
# CSV
df = [pd.read](http://pd.read)_csv('dados.csv')
[df.to](http://df.to)_csv('output.csv', index=False)

# Excel
df = [pd.read](http://pd.read)_excel('dados.xlsx', sheet_name='Plan1')
[df.to](http://df.to)_excel('output.xlsx', sheet_name='Resultados', index=False)

# SQL (conectando ao SQL Server do Dynamics)
import pyodbc
from sqlalchemy import create_engine

connection_string = (
    'mssql+pyodbc://username:password@server/database'
    '?driver=ODBC+Driver+17+for+SQL+Server'
)
engine = create_engine(connection_string)

# Ler tabela
df = [pd.read](http://pd.read)_sql('SELECT * FROM Account', engine)

# Ler query customizada
query = """
SELECT 
    AccountId,
    Name,
    Revenue,
    CreatedOn
FROM Account
WHERE Revenue > 100000
"""
df = [pd.read](http://pd.read)_sql(query, engine)

# JSON
df = [pd.read](http://pd.read)_json('dados.json')
[df.to](http://df.to)_json('output.json', orient='records')

# Dataverse/Dynamics (via Web API)
import requests

headers = {
    'Authorization': f'Bearer {access_token}',
    'OData-MaxVersion': '4.0',
    'OData-Version': '4.0',
    'Accept': 'application/json'
}

response = requests.get(
    '[https://org.crm.dynamics.com/api/data/v9.2/accounts](https://org.crm.dynamics.com/api/data/v9.2/accounts)',
    headers=headers
)

df = pd.DataFrame(response.json()['value'])
```

### 3. Seleção e Filtragem

```python
# Selecionar colunas
df['Nome']  # Uma coluna (Series)
df[['Nome', 'Idade']]  # Múltiplas colunas (DataFrame)

# Selecionar linhas por índice
df.iloc[0]  # Primeira linha
df.iloc[0:3]  # Primeiras 3 linhas
df.iloc[:, 0:2]  # Todas linhas, primeiras 2 colunas

# Selecionar por label
df.loc[0, 'Nome']  # Valor específico
df.loc[:, 'Nome':'Cidade']  # Range de colunas

# Filtragem condicional
df[df['Idade'] > 30]  # Pessoas com mais de 30 anos
df[df['Salário'] >= 5000]  # Salários >= 5000

# Múltiplas condições
df[(df['Idade'] > 25) & (df['Salário'] < 8000)]
df[(df['Cidade'] == 'São Paulo') | (df['Cidade'] == 'Rio de Janeiro')]

# Filtrar por lista
cidades_interesse = ['São Paulo', 'Brasília']
df[df['Cidade'].isin(cidades_interesse)]

# Filtrar strings
df[df['Nome'].str.contains('a', case=False)]  # Nomes com 'a'
df[df['Nome'].str.startswith('M')]  # Começa com M
```

### 4. Limpeza de Dados

```python
# Valores ausentes
df.isnull().sum()  # Contar NaN por coluna
df.dropna()  # Remover linhas com NaN
df.dropna(subset=['Salário'])  # Remover só se Salário for NaN
df.fillna(0)  # Preencher NaN com 0
df['Idade'].fillna(df['Idade'].mean())  # Preencher com média

# Forward/Backward fill
df.fillna(method='ffill')  # Preencher com valor anterior
df.fillna(method='bfill')  # Preencher com próximo valor

# Duplicatas
df.duplicated().sum()  # Contar duplicatas
df.drop_duplicates()  # Remover duplicatas
df.drop_duplicates(subset=['Nome'], keep='first')  # Por coluna específica

# Renomear colunas
df.rename(columns={'Nome': 'Nome_Completo', 'Salário': 'Remuneracao'})

# Alterar tipos de dados
df['Idade'] = df['Idade'].astype(int)
df['Data'] = [pd.to](http://pd.to)_datetime(df['Data'])
df['Categoria'] = df['Categoria'].astype('category')

# Remover espaços
df['Nome'] = df['Nome'].str.strip()

# Substituir valores
df['Cidade'].replace('SP', 'São Paulo', inplace=True)
```

### 5. Transformações e Agregações

```python
# Criar novas colunas
df['Salário_Anual'] = df['Salário'] * 12
df['Faixa_Etária'] = df['Idade'].apply(lambda x: 'Jovem' if x < 30 else 'Adulto')

# Apply customizado
def categorizar_salario(valor):
    if valor < 5000:
        return 'Baixo'
    elif valor < 8000:
        return 'Médio'
    else:
        return 'Alto'

df['Categoria_Salário'] = df['Salário'].apply(categorizar_salario)

# GroupBy (equivalente a Pivot Table)
df.groupby('Cidade')['Salário'].mean()
df.groupby('Cidade').agg({
    'Salário': ['mean', 'sum', 'count'],
    'Idade': 'mean'
})

# Pivot Table
pivot = df.pivot_table(
    values='Salário',
    index='Cidade',
    columns='Faixa_Etária',
    aggfunc='mean'
)

# Ordenação
df.sort_values('Salário', ascending=False)
df.sort_values(['Cidade', 'Idade'], ascending=[True, False])
```

### 6. Merge e Join

```python
# Dados de exemplo
clientes = pd.DataFrame({
    'ClienteID': [1, 2, 3, 4],
    'Nome': ['João', 'Maria', 'Pedro', 'Ana']
})

pedidos = pd.DataFrame({
    'PedidoID': [101, 102, 103, 104],
    'ClienteID': [1, 1, 2, 3],
    'Valor': [1000, 1500, 2000, 800]
})

# Inner Join (SQL: INNER JOIN)
result = pd.merge(clientes, pedidos, on='ClienteID', how='inner')

# Left Join (SQL: LEFT JOIN)
result = pd.merge(clientes, pedidos, on='ClienteID', how='left')

# Right Join (SQL: RIGHT JOIN)
result = pd.merge(clientes, pedidos, on='ClienteID', how='right')

# Outer Join (SQL: FULL OUTER JOIN)
result = pd.merge(clientes, pedidos, on='ClienteID', how='outer')

# Join com colunas diferentes
result = pd.merge(
    clientes, 
    pedidos, 
    left_on='ClienteID', 
    right_on='ClienteID'
)

# Concatenar DataFrames
df1 = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})
df2 = pd.DataFrame({'A': [5, 6], 'B': [7, 8]})

pd.concat([df1, df2])  # Vertical (empilhar)
pd.concat([df1, df2], axis=1)  # Horizontal (lado a lado)
```

### 7. Trabalhando com Datas

```python
# Criar coluna de data
df['Data'] = [pd.to](http://pd.to)_datetime('2024-01-01')
df['Data'] = [pd.to](http://pd.to)_datetime(df['DataString'], format='%d/%m/%Y')

# Extrair componentes
df['Ano'] = df['Data'].dt.year
df['Mês'] = df['Data'].dt.month
df['Dia'] = df['Data'].[dt.day](http://dt.day)
df['Dia_da_Semana'] = df['Data'].[dt.day](http://dt.day)_name()
df['Trimestre'] = df['Data'].dt.quarter

# Operações com datas
df['Data_Futuro'] = df['Data'] + pd.Timedelta(days=30)
df['Diferença_Dias'] = (df['Data_Fim'] - df['Data_Inicio']).dt.days

# Filtrar por período
df[df['Data'] >= '2024-01-01']
df[df['Data'].between('2024-01-01', '2024-12-31')]

# Resample (agregação temporal)
df.set_index('Data').resample('M')['Valor'].sum()  # Soma mensal
df.set_index('Data').resample('Q')['Valor'].mean()  # Média trimestral
```

## 🎯 Exercícios Práticos - Pandas

### Exercício 1: Análise de Vendas

```python
# Criar dataset de vendas
vendas = pd.DataFrame({
    'Data': [pd.date](http://pd.date)_range('2024-01-01', periods=100),
    'Produto': np.random.choice(['A', 'B', 'C'], 100),
    'Quantidade': np.random.randint(1, 20, 100),
    'Preço': np.random.uniform(50, 200, 100)
})

# Tarefas:
# 1. Calcule o faturamento total por produto
# 2. Encontre o dia com maior faturamento
# 3. Calcule a média de quantidade vendida por mês
# 4. Identifique produtos com faturamento > R$ 5000
```

### Exercício 2: Limpeza de Dados do Dynamics

```python
# Simular dados sujos do CRM
accounts = pd.DataFrame({
    'AccountId': ['1', '2', '3', '4', '5'],
    'Name': ['  Empresa A  ', 'EMPRESA B', 'Empresa C', '  EMPRESA D', 'Empresa A'],
    'Revenue': [100000, None, 250000, 180000, 100000],
    'CreatedOn': ['2024-01-15', '15/02/2024', '2024-03-10', None, '2024-01-15']
})

# Tarefas:
# 1. Remover espaços dos nomes
# 2. Padronizar capitalização
# 3. Remover duplicatas
# 4. Preencher Revenue ausente com média
# 5. Padronizar formato de data
```

---

# 🔢 NumPy: A Base Matemática

## O que é NumPy?

NumPy (Numerical Python) é a biblioteca fundamental para computação científica em Python. Pandas é construído em cima do NumPy.

**Por que NumPy?**

- Arrays multidimensionais extremamente eficientes
- Operações vetorizadas (50-100x mais rápidas que loops Python)
- Base para todo ecossistema científico (SciPy, Scikit-learn, TensorFlow)
- Álgebra linear, estatística, transformações matemáticas

## 🚀 Como Começar com NumPy

### Arrays vs Listas Python

```python
import numpy as np

# Lista Python
lista = [1, 2, 3, 4, 5]

# Array NumPy
array = np.array([1, 2, 3, 4, 5])

# Por que arrays são melhores?
# 1. Mais rápidos
# 2. Usam menos memória
# 3. Operações vetorizadas

# Comparação de performance
import time

# Com lista Python
start = time.time()
lista_resultado = [x * 2 for x in range(1000000)]
print(f"Lista: {time.time() - start:.4f}s")

# Com NumPy
start = time.time()
array_resultado = np.array(range(1000000)) * 2
print(f"NumPy: {time.time() - start:.4f}s")  # Muito mais rápido!
```

## 📚 Conceitos Fundamentais do NumPy

### 1. Criação de Arrays

```python
# Array 1D
arr1d = np.array([1, 2, 3, 4, 5])

# Array 2D (matriz)
arr2d = np.array([[1, 2, 3], [4, 5, 6]])

# Array 3D
arr3d = np.array([[[1, 2], [3, 4]], [[5, 6], [7, 8]]])

# Arrays especiais
zeros = np.zeros((3, 4))  # Matriz 3x4 de zeros
ones = np.ones((2, 3))  # Matriz 2x3 de uns
eye = np.eye(4)  # Matriz identidade 4x4
random = np.random.rand(3, 3)  # Valores aleatórios [0, 1)

# Ranges
arange = np.arange(0, 10, 2)  # [0, 2, 4, 6, 8]
linspace = np.linspace(0, 1, 5)  # 5 valores igualmente espaçados entre 0 e 1
```

### 2. Propriedades e Atributos

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])

print(arr.shape)  # (2, 3) - dimensões
print(arr.ndim)  # 2 - número de dimensões
print(arr.size)  # 6 - total de elementos
print(arr.dtype)  # int64 - tipo de dados
print(arr.itemsize)  # 8 - bytes por elemento
```

### 3. Indexação e Slicing

```python
arr = np.array([10, 20, 30, 40, 50])

# Indexação básica
print(arr[0])  # 10
print(arr[-1])  # 50

# Slicing
print(arr[1:4])  # [20, 30, 40]
print(arr[:3])  # [10, 20, 30]
print(arr[::2])  # [10, 30, 50] - a cada 2

# Arrays 2D
arr2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
print(arr2d[0, 0])  # 1 - linha 0, coluna 0
print(arr2d[1, :])  # [4, 5, 6] - linha 1 inteira
print(arr2d[:, 1])  # [2, 5, 8] - coluna 1 inteira

# Boolean indexing
arr = np.array([1, 2, 3, 4, 5])
mask = arr > 3
print(arr[mask])  # [4, 5]
print(arr[arr > 3])  # [4, 5] - direto
```

### 4. Operações Matemáticas

```python
arr = np.array([1, 2, 3, 4, 5])

# Operações elemento a elemento
print(arr + 10)  # [11, 12, 13, 14, 15]
print(arr * 2)  # [2, 4, 6, 8, 10]
print(arr ** 2)  # [1, 4, 9, 16, 25]
print(np.sqrt(arr))  # Raiz quadrada

# Operações entre arrays
arr1 = np.array([1, 2, 3])
arr2 = np.array([4, 5, 6])
print(arr1 + arr2)  # [5, 7, 9]
print(arr1 * arr2)  # [4, 10, 18]

# Funções universais
print(np.sin(arr))
print(np.cos(arr))
print(np.exp(arr))
print(np.log(arr))
```

### 5. Agregações e Estatísticas

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])

# Agregações
print(arr.sum())  # 21 - soma total
print(arr.mean())  # 3.5 - média
print(arr.std())  # desvio padrão
print(arr.var())  # variância
print(arr.min())  # 1
print(arr.max())  # 6

# Por eixo
print(arr.sum(axis=0))  # [5, 7, 9] - soma por coluna
print(arr.sum(axis=1))  # [6, 15] - soma por linha
print(arr.mean(axis=0))  # Média por coluna

# Outras funções úteis
print(arr.cumsum())  # Soma cumulativa
print(arr.cumprod())  # Produto cumulativo
print(np.median(arr))  # Mediana
print(np.percentile(arr, 75))  # Percentil 75
```

### 6. Reshape e Transposição

```python
arr = np.array([1, 2, 3, 4, 5, 6])

# Reshape
arr_2d = arr.reshape(2, 3)  # Matriz 2x3
print(arr_2d)
# [[1, 2, 3],
#  [4, 5, 6]]

arr_3d = arr.reshape(2, 3, 1)

# Flatten
flat = arr_2d.flatten()  # Volta para 1D

# Transpose
print(arr_2d.T)  # Transposta
# [[1, 4],
#  [2, 5],
#  [3, 6]]
```

### 7. Broadcasting

```python
# Broadcasting permite operações entre arrays de shapes diferentes

# Scalar + Array
arr = np.array([1, 2, 3])
print(arr + 10)  # [11, 12, 13]

# Array 1D + Array 2D
arr1d = np.array([1, 2, 3])
arr2d = np.array([[10], [20], [30]])
result = arr1d + arr2d
print(result)
# [[11, 12, 13],
#  [21, 22, 23],
#  [31, 32, 33]]
```

### 8. Álgebra Linear

```python
# Produto de matrizes
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

# Produto elemento a elemento
print(A * B)

# Produto matricial (dot product)
print([np.dot](http://np.dot)(A, B))
print(A @ B)  # Operador @ (Python 3.5+)

# Determinante
print(np.linalg.det(A))

# Inversa
print(np.linalg.inv(A))

# Autovalores e autovetores
eigenvalues, eigenvectors = np.linalg.eig(A)

# Resolver sistema linear Ax = b
A = np.array([[3, 1], [1, 2]])
b = np.array([9, 8])
x = np.linalg.solve(A, b)
print(x)  # [2, 3]
```

## 🎯 Exercícios Práticos - NumPy

### Exercício 1: Análise de Vendas

```python
# Simular vendas de 100 dias
vendas = np.random.randint(50, 200, size=100)

# Tarefas:
# 1. Calcule média, mediana e desvio padrão
# 2. Encontre dias com vendas > média + 1 desvio padrão
# 3. Calcule vendas acumuladas
# 4. Normalize os valores (min-max scaling)
```

### Exercício 2: Matrizes de Correlação

```python
# Criar dados de vendas de 3 produtos
produto_a = np.random.randint(100, 500, 50)
produto_b = np.random.randint(80, 400, 50)
produto_c = np.random.randint(120, 600, 50)

data = np.column_stack([produto_a, produto_b, produto_c])

# Tarefas:
# 1. Calcule matriz de correlação
# 2. Identifique produtos com correlação > 0.7
# 3. Normalize cada produto individualmente
```

---

# ⚡ PySpark no Databricks

## O que é PySpark?

PySpark é a interface Python para Apache Spark, um framework de processamento distribuído de Big Data. Permite processar terabytes de dados em clusters.

**Pandas vs PySpark:**

- **Pandas:** Dados cabem na memória de uma máquina (~10GB)
- **PySpark:** Dados distribuídos em cluster (terabytes/petabytes)

## 🚀 Por que Databricks?

Databricks é uma plataforma gerenciada para Spark que simplifica:

- Setup de clusters
- Notebooks interativos
- Integração com Azure/AWS/GCP
- MLflow para tracking de experimentos
- Delta Lake para data lakehouse

## 🏁 Setup Inicial no Databricks

### 1. Criar Conta Gratuita

1. Acesse: [https://www.databricks.com/try-databricks](https://www.databricks.com/try-databricks)
2. Escolha "Community Edition" (gratuita) ou trial do Azure Databricks
3. Crie um cluster (Runtime 13.3 LTS ou superior)

### 2. Primeiro Notebook

```python
# No Databricks, SparkSession já está disponível como 'spark'
print(spark.version)

# Verificar configurações do cluster
print(spark.sparkContext.getConf().getAll())
```

## 📚 Conceitos Fundamentais do PySpark

### 1. DataFrame vs Pandas DataFrame

```python
# Criar DataFrame PySpark
data = [
    (1, "João", 28, 5000),
    (2, "Maria", 34, 7500),
    (3, "Pedro", 23, 4200),
    (4, "Ana", 45, 9000)
]

columns = ["id", "nome", "idade", "salario"]

df_spark = spark.createDataFrame(data, columns)

# Mostrar dados
df_[spark.show](http://spark.show)()

# Schema
df_spark.printSchema()

# Converter de/para Pandas
df_pandas = df_spark.toPandas()  # Spark -> Pandas
df_spark = spark.createDataFrame(df_pandas)  # Pandas -> Spark
```

### 2. Leitura de Dados

```python
# CSV
df = [spark.read](http://spark.read).csv(
    "/path/to/file.csv",
    header=True,
    inferSchema=True
)

# Com opções customizadas
df = [spark.read](http://spark.read) \
    .option("header", "true") \
    .option("inferSchema", "true") \
    .option("delimiter", ";") \
    .csv("/path/to/file.csv")

# Parquet (formato otimizado)
df = [spark.read](http://spark.read).parquet("/path/to/file.parquet")

# JSON
df = [spark.read](http://spark.read).json("/path/to/file.json")

# Delta Lake (recomendado no Databricks)
df = [spark.read](http://spark.read).format("delta").load("/path/to/delta/table")

# SQL Server / Azure SQL
df = [spark.read](http://spark.read) \
    .format("jdbc") \
    .option("url", "jdbc:sqlserver://server:1433;database=DB") \
    .option("dbtable", "dbo.Accounts") \
    .option("user", "username") \
    .option("password", "password") \
    .load()
```

### 3. Seleção e Filtragem

```python
from pyspark.sql.functions import col

# Selecionar colunas
[df.select](http://df.select)("nome", "idade").show()
[df.select](http://df.select)(col("nome"), col("idade")).show()

# Filtrar
df.filter(col("idade") > 30).show()
df.filter("idade > 30").show()  # SQL-like
df.where(col("salario") >= 5000).show()

# Múltiplas condições
df.filter((col("idade") > 25) & (col("salario") < 8000)).show()
df.filter((col("nome") == "João") | (col("nome") == "Maria")).show()
```

### 4. Transformações

```python
from pyspark.sql.functions import *

# Adicionar coluna
df = df.withColumn("salario_anual", col("salario") * 12)

# Renomear coluna
df = df.withColumnRenamed("nome", "nome_completo")

# Aplicar função
df = df.withColumn(
    "faixa_etaria",
    when(col("idade") < 30, "Jovem")
    .when(col("idade") < 50, "Adulto")
    .otherwise("Sênior")
)

# UDF (User Defined Function)
from pyspark.sql.types import StringType

def categorizar_salario(valor):
    if valor < 5000:
        return "Baixo"
    elif valor < 8000:
        return "Médio"
    else:
        return "Alto"

categorizar_udf = udf(categorizar_salario, StringType())
df = df.withColumn("categoria_salario", categorizar_udf(col("salario")))
```

### 5. Agregações

```python
# Agregações simples
[df.select](http://df.select)(avg("salario"), max("idade"), min("idade")).show()

# GroupBy
df.groupBy("faixa_etaria").agg(
    avg("salario").alias("salario_medio"),
    count("*").alias("quantidade")
).show()

# Múltiplas agregações
df.groupBy("cidade").agg(
    avg("salario").alias("salario_medio"),
    sum("salario").alias("salario_total"),
    count("*").alias("total_pessoas"),
    max("idade").alias("idade_maxima")
).show()
```

### 6. Joins

```python
# Dados de exemplo
clientes = spark.createDataFrame([
    (1, "João"),
    (2, "Maria"),
    (3, "Pedro")
], ["cliente_id", "nome"])

pedidos = spark.createDataFrame([
    (101, 1, 1000),
    (102, 1, 1500),
    (103, 2, 2000)
], ["pedido_id", "cliente_id", "valor"])

# Inner Join
result = clientes.join(pedidos, "cliente_id", "inner")

# Left Join
result = clientes.join(pedidos, "cliente_id", "left")

# Right Join
result = clientes.join(pedidos, "cliente_id", "right")

# Full Outer Join
result = clientes.join(pedidos, "cliente_id", "outer")

# Join com condições diferentes
result = clientes.join(
    pedidos,
    clientes.cliente_id == pedidos.cliente_id,
    "inner"
)
```

### 7. SQL no PySpark

```python
# Registrar DataFrame como tabela temporária
df.createOrReplaceTempView("pessoas")

# Executar SQL
result = spark.sql("""
    SELECT 
        faixa_etaria,
        AVG(salario) as salario_medio,
        COUNT(*) as quantidade
    FROM pessoas
    WHERE idade > 25
    GROUP BY faixa_etaria
    ORDER BY salario_medio DESC
""")

[result.show](http://result.show)()
```

### 8. Performance e Otimização

```python
# Cache (manter em memória)
df_cached = df.cache()

# Persist (mais opções de armazenamento)
from pyspark import StorageLevel
df.persist(StorageLevel.MEMORY_AND_DISK)

# Repartition (redistribuir dados)
df_repartitioned = df.repartition(10)  # 10 partições

# Coalesce (reduzir partições sem shuffle)
df_coalesced = df.coalesce(2)

# Explain plan (debug de performance)
df.explain()
df.explain(mode="extended")
```

## 🎯 Projeto Prático: PySpark no Databricks

### Análise de Logs do Dynamics 365

```python
# 1. Ler dados de audit logs (exemplo)
audit_logs = [spark.read](http://spark.read).format("delta").load("/mnt/dynamics/audit_logs")

# 2. Limpeza
logs_clean = audit_logs \
    .filter(col("action").isNotNull()) \
    .filter(col("timestamp") >= "2024-01-01")

# 3. Análise de ações mais comuns
action_stats = logs_clean.groupBy("action", "user").agg(
    count("*").alias("count"),
    min("timestamp").alias("first_occurrence"),
    max("timestamp").alias("last_occurrence")
).orderBy(desc("count"))

action_[stats.show](http://stats.show)(20)

# 4. Análise temporal
logs_with_hour = logs_clean.withColumn(
    "hour",
    hour(col("timestamp"))
)

hourly_activity = logs_with_hour.groupBy("hour").agg(
    count("*").alias("actions")
).orderBy("hour")

# 5. Detectar anomalias (usuários com atividade suspeita)
user_stats = logs_clean.groupBy("user").agg(
    count("*").alias("total_actions"),
    countDistinct("entity").alias("distinct_entities"),
    (max("timestamp").cast("long") - min("timestamp").cast("long")).alias("time_span_seconds")
)

suspicious = user_stats.filter(
    (col("total_actions") > 1000) | 
    (col("time_span_seconds") < 60)
)

# 6. Salvar resultados
action_stats.write \
    .format("delta") \
    .mode("overwrite") \
    .save("/mnt/analytics/action_stats")

print("Análise completa!")
```

---

# 📊 Projeto Final: EDA Completa

## Objetivo

Realizar uma Análise Exploratória de Dados (EDA) completa em um dataset real, aplicando todos os conceitos aprendidos.

## 🎯 Opções de Dataset

### Opção 1: Dataset Público (Kaggle)

**Sugestões de datasets:**

1. **Titanic** (clássico para iniciantes)
    - [https://www.kaggle.com/c/titanic](https://www.kaggle.com/c/titanic)
    - Predição de sobrevivência
2. **House Prices**
    - [https://www.kaggle.com/c/house-prices-advanced-regression-techniques](https://www.kaggle.com/c/house-prices-advanced-regression-techniques)
    - Previsão de preços de imóveis
3. **Customer Churn**
    - Análise de cancelamento de clientes
    - Relevante para CRM/Dynamics
4. **E-commerce Transactions**
    - Análise de vendas online
    - Padrões de compra

### Opção 2: Dados do Dynamics 365 (Anonimizados)

```python
# Extrair dados do Dynamics via API
import requests
import pandas as pd

def get_dynamics_data(entity, token):
    url = f"[https://org.crm.dynamics.com/api/data/v9.2/{entity}](https://org.crm.dynamics.com/api/data/v9.2/{entity})"
    headers = {
        'Authorization': f'Bearer {token}',
        'OData-MaxVersion': '4.0',
        'Accept': 'application/json'
    }
    
    response = requests.get(url, headers=headers)
    return pd.DataFrame(response.json()['value'])

# Exemplo: Analisar contas
accounts = get_dynamics_data('accounts', access_token)

# Anonimizar dados sensíveis
accounts['name'] = 'Account_' + accounts.index.astype(str)
accounts = accounts.drop(columns=['emailaddress', 'telephone1'])
```

## 📋 Estrutura da EDA

### 1. Compreensão dos Dados

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Configurar visualizações
sns.set_style("whitegrid")
plt.rcParams['figure.figsize'] = (12, 6)

# Carregar dados
df = [pd.read](http://pd.read)_csv('dataset.csv')

# Visão geral
print("Shape:", df.shape)
print("\nInfo:")
[df.info](http://df.info)()
print("\nPrimeiras linhas:")
print(df.head(10))
print("\nÚltimas linhas:")
print(df.tail(10))
print("\nEstatísticas descritivas:")
print(df.describe())
print("\nTipos de dados:")
print(df.dtypes)
```

### 2. Limpeza de Dados

```python
# Valores ausentes
print("Valores ausentes por coluna:")
missing = df.isnull().sum()
print(missing[missing > 0])

# Visualizar missing values
import missingno as msno
msno.matrix(df)
[plt.show](http://plt.show)()

# Estratégias de tratamento
# Numéricos: preencher com média/mediana
df['idade'].fillna(df['idade'].median(), inplace=True)

# Categóricos: preencher com moda ou categoria 'Desconhecido'
df['categoria'].fillna('Desconhecido', inplace=True)

# Ou remover linhas com muitos valores ausentes
df = df.dropna(thresh=len(df.columns) * 0.7)  # 70% de dados presentes

# Duplicatas
print(f"\nDuplicatas: {df.duplicated().sum()}")
df = df.drop_duplicates()

# Outliers (IQR method)
def remove_outliers(df, column):
    Q1 = df[column].quantile(0.25)
    Q3 = df[column].quantile(0.75)
    IQR = Q3 - Q1
    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR
    return df[(df[column] >= lower) & (df[column] <= upper)]

df = remove_outliers(df, 'revenue')
```

### 3. Análise Univariada

```python
# Variáveis numéricas
numeric_cols = [df.select](http://df.select)_dtypes(include=[np.number]).columns

for col in numeric_cols:
    fig, axes = plt.subplots(1, 3, figsize=(15, 4))
    
    # Histograma
    axes[0].hist(df[col].dropna(), bins=30, edgecolor='black')
    axes[0].set_title(f'Histograma - {col}')
    axes[0].set_xlabel(col)
    axes[0].set_ylabel('Frequência')
    
    # Boxplot
    axes[1].boxplot(df[col].dropna())
    axes[1].set_title(f'Boxplot - {col}')
    axes[1].set_ylabel(col)
    
    # Densidade
    df[col].plot(kind='density', ax=axes[2])
    axes[2].set_title(f'Densidade - {col}')
    axes[2].set_xlabel(col)
    
    plt.tight_layout()
    [plt.show](http://plt.show)()
    
    # Estatísticas
    print(f"\n{col}:")
    print(f"  Média: {df[col].mean():.2f}")
    print(f"  Mediana: {df[col].median():.2f}")
    print(f"  Desvio padrão: {df[col].std():.2f}")
    print(f"  Min: {df[col].min():.2f}")
    print(f"  Max: {df[col].max():.2f}")

# Variáveis categóricas
categorical_cols = [df.select](http://df.select)_dtypes(include=['object']).columns

for col in categorical_cols:
    print(f"\n{col}:")
    value_counts = df[col].value_counts()
    print(value_counts)
    
    # Gráfico de barras
    plt.figure(figsize=(10, 6))
    value_counts.head(10).plot(kind='bar')
    plt.title(f'Top 10 valores - {col}')
    plt.xlabel(col)
    plt.ylabel('Contagem')
    plt.xticks(rotation=45)
    plt.tight_layout()
    [plt.show](http://plt.show)()
```

### 4. Análise Bivariada

```python
# Correlação entre variáveis numéricas
correlation_matrix = df[numeric_cols].corr()

plt.figure(figsize=(12, 10))
sns.heatmap(
    correlation_matrix,
    annot=True,
    fmt='.2f',
    cmap='coolwarm',
    center=0,
    square=True
)
plt.title('Matriz de Correlação')
plt.tight_layout()
[plt.show](http://plt.show)()

# Identificar correlações fortes
strong_corr = []
for i in range(len(correlation_matrix.columns)):
    for j in range(i):
        if abs(correlation_matrix.iloc[i, j]) > 0.7:
            strong_corr.append((
                correlation_matrix.columns[i],
                correlation_matrix.columns[j],
                correlation_matrix.iloc[i, j]
            ))

print("\nCorrelações fortes (|r| > 0.7):")
for col1, col2, corr in strong_corr:
    print(f"  {col1} vs {col2}: {corr:.3f}")

# Scatter plots para correlações interessantes
for col1, col2, corr in strong_corr[:5]:  # Top 5
    plt.figure(figsize=(8, 6))
    plt.scatter(df[col1], df[col2], alpha=0.5)
    plt.xlabel(col1)
    plt.ylabel(col2)
    plt.title(f'{col1} vs {col2} (r={corr:.3f})')
    plt.tight_layout()
    [plt.show](http://plt.show)()

# Análise categórica vs numérica
for cat_col in categorical_cols[:3]:  # Primeiras 3 categóricas
    for num_col in numeric_cols[:3]:  # Primeiros 3 numéricas
        plt.figure(figsize=(10, 6))
        df.boxplot(column=num_col, by=cat_col)
        plt.title(f'{num_col} por {cat_col}')
        plt.suptitle('')  # Remover título padrão
        plt.tight_layout()
        [plt.show](http://plt.show)()
```

### 5. Análise Temporal (se aplicável)

```python
# Se houver coluna de data
df['data'] = [pd.to](http://pd.to)_datetime(df['data'])
df = df.sort_values('data')

# Adicionar componentes temporais
df['ano'] = df['data'].dt.year
df['mes'] = df['data'].dt.month
df['dia_semana'] = df['data'].[dt.day](http://dt.day)_name()
df['trimestre'] = df['data'].dt.quarter

# Tendência ao longo do tempo
plt.figure(figsize=(14, 6))
df.groupby('data')['valor'].sum().plot()
plt.title('Evolução Temporal')
plt.xlabel('Data')
plt.ylabel('Valor')
plt.grid(True)
plt.tight_layout()
[plt.show](http://plt.show)()

# Sazonalidade mensal
monthly = df.groupby('mes')['valor'].mean()
plt.figure(figsize=(10, 6))
monthly.plot(kind='bar')
plt.title('Média por Mês')
plt.xlabel('Mês')
plt.ylabel('Valor Médio')
plt.tight_layout()
[plt.show](http://plt.show)()

# Padrão semanal
weekday_order = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
weekly = df.groupby('dia_semana')['valor'].mean().reindex(weekday_order)
plt.figure(figsize=(10, 6))
weekly.plot(kind='bar')
plt.title('Média por Dia da Semana')
plt.xlabel('Dia')
plt.ylabel('Valor Médio')
plt.xticks(rotation=45)
plt.tight_layout()
[plt.show](http://plt.show)()
```

### 6. Feature Engineering

```python
# Criar novas features

# Binning (categorização)
df['faixa_idade'] = pd.cut(
    df['idade'],
    bins=[0, 18, 30, 50, 100],
    labels=['Jovem', 'Adulto Jovem', 'Adulto', 'Sênior']
)

# Log transformation (para distribuições skewed)
df['revenue_log'] = np.log1p(df['revenue'])

# Interações
df['revenue_per_employee'] = df['revenue'] / df['employees']

# Encoding categórico
from sklearn.preprocessing import LabelEncoder

le = LabelEncoder()
df['categoria_encoded'] = [le.fit](http://le.fit)_transform(df['categoria'])

# One-hot encoding
df_encoded = pd.get_dummies(df, columns=['categoria'], prefix='cat')
```

### 7. Insights e Conclusões

```python
# Resumo executivo
print("=" * 60)
print("RESUMO DA ANÁLISE EXPLORATÓRIA")
print("=" * 60)

print(f"\n1. VISÃO GERAL DOS DADOS")
print(f"   - Total de registros: {len(df):,}")
print(f"   - Total de variáveis: {len(df.columns)}")
print(f"   - Período: {df['data'].min()} a {df['data'].max()}")

print(f"\n2. QUALIDADE DOS DADOS")
print(f"   - Completude: {(1 - df.isnull().sum().sum() / df.size) * 100:.1f}%")
print(f"   - Duplicatas removidas: {duplicatas_removidas}")

print(f"\n3. PRINCIPAIS ACHADOS")
print(f"   - Variável com maior variação: {df[numeric_cols].std().idxmax()}")
print(f"   - Correlação mais forte: {strong_corr[0] if strong_corr else 'Nenhuma >0.7'}")
print(f"   - Categoria mais frequente: {df['categoria'].mode()[0]}")

print(f"\n4. RECOMENDAÇÕES")
print(f"   - Investigar outliers em: [lista de colunas]")
print(f"   - Considerar transformações para: [colunas skewed]")
print(f"   - Features candidatas para ML: [lista]")
```

### 8. Exportar Relatório

```python
# Salvar análise em HTML
import pandas_profiling

profile = pandas_profiling.ProfileReport(df)
[profile.to](http://profile.to)_file("relatorio_eda.html")

# Ou criar notebook executivo
# ...
```

---

# 📚 Recursos de Aprendizado

## Cursos Online

### Pandas e NumPy

1. **Kaggle Learn** - Pandas (gratuito)
    - [https://www.kaggle.com/learn/pandas](https://www.kaggle.com/learn/pandas)
2. **DataCamp** - Data Manipulation with Pandas
3. **Coursera** - Applied Data Science with Python (University of Michigan)

### PySpark

1. **Databricks Academy** - Apache Spark Programming (gratuito)
    - [https://academy.databricks.com/](https://academy.databricks.com/)
2. **Udemy** - Spark and Python for Big Data with PySpark
3. **Coursera** - Big Data Analysis with Scala and Spark

## Livros

1. **"Python for Data Analysis"** - Wes McKinney (criador do Pandas)
2. **"Learning Spark"** - O'Reilly
3. **"Hands-On Data Analysis with Pandas"** - Stefanie Molin

## Documentação Oficial

- [Pandas Docs](https://pandas.pydata.org/docs/)
- [NumPy Docs](https://numpy.org/doc/)
- [PySpark Docs](https://spark.apache.org/docs/latest/api/python/)
- [Databricks Docs](https://docs.databricks.com/)

## Datasets para Prática

- **Kaggle**: [https://www.kaggle.com/datasets](https://www.kaggle.com/datasets)
- **UCI Machine Learning Repository**: [https://archive.ics.uci.edu/ml/](https://archive.ics.uci.edu/ml/)
- **Google Dataset Search**: [https://datasetsearch.research.google.com/](https://datasetsearch.research.google.com/)
- **Azure Open Datasets**: [https://azure.microsoft.com/en-us/services/open-datasets/](https://azure.microsoft.com/en-us/services/open-datasets/)

## Comunidades

- Stack Overflow (tag: pandas, pyspark)
- Reddit: r/datascience, r/learnpython
- Databricks Community Forums

---

# ✅ Checklist de Progresso

## Semana 1-2: Pandas Básico

- [ ]  Instalei Pandas e configurei ambiente
- [ ]  Entendi Series e DataFrame
- [ ]  Consigo ler/escrever CSV e Excel
- [ ]  Sei selecionar e filtrar dados
- [ ]  Praticei limpeza de dados básica

## Semana 3-4: Pandas Avançado

- [ ]  Domino agregações e GroupBy
- [ ]  Sei fazer merge e join
- [ ]  Manipulo datas com facilidade
- [ ]  Criei visualizações básicas
- [ ]  Completei 3 exercícios práticos

## Semana 5-6: NumPy

- [ ]  Entendi arrays multidimensionais
- [ ]  Domino indexação e slicing
- [ ]  Sei fazer operações vetorizadas
- [ ]  Aplico funções de álgebra linear
- [ ]  Entendo broadcasting

## Semana 7-8: PySpark Setup e Básico

- [ ]  Criei conta no Databricks
- [ ]  Configurei primeiro cluster
- [ ]  Entendi conceito de DataFrame distribuído
- [ ]  Sei ler dados de diferentes fontes
- [ ]  Executei transformações básicas

## Semana 9-10: PySpark Avançado

- [ ]  Domino agregações complexas
- [ ]  Sei otimizar queries (cache, repartition)
- [ ]  Uso SQL no PySpark
- [ ]  Entendo particionamento
- [ ]  Trabalhei com Delta Lake

## Semana 11-12: Projeto EDA

- [ ]  Escolhi dataset relevante
- [ ]  Completei limpeza de dados
- [ ]  Realizei análise univariada completa
- [ ]  Identifiquei correlações importantes
- [ ]  Criei features novas
- [ ]  Gerei insights acionáveis
- [ ]  Documentei todo o processo

---

# 🚀 Próximos Passos Imediatos

## Esta Semana

1. **Setup do Ambiente:**
    
    ```bash
    pip install pandas numpy matplotlib seaborn jupyter
    jupyter notebook
    ```
    
2. **Primeiro Exercício:**
    - Baixe dataset Titanic do Kaggle
    - Carregue com Pandas
    - Explore com .head(), .info(), .describe()
3. **Meta Diária:**
    - 30-60 minutos de prática
    - 1 conceito novo por dia

## Próximas 2 Semanas

1. Complete todos exercícios de Pandas
2. Leia documentação oficial
3. Assista 1 curso online (Kaggle Learn)

## Mês 1 - Meta Final

- Projeto: Análise completa de dados do Dynamics 365
- Extrair dados reais via API
- Criar dashboard de insights
- Documentar processo

## Mês 2 - Meta Final

- Migrar projeto para Databricks
- Processar dataset grande (>1GB) com PySpark
- Implementar pipeline de dados completo
- Preparar para Fase 2 (Machine Learning)

---

**Lembre-se:** Prática consistente é mais importante que intensidade! Faça um pouco todos os dias. 💪

**Dica:** Conecte o aprendizado ao seu trabalho atual - analise dados do Dynamics 365, isso tornará o aprendizado mais relevante e prático!
