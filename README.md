# Manipulacion-y-Analisis-de-Datos-con-Pandas-y-Python

[Dataset Search](https://datasetsearch.research.google.com/)

## 10 minutes to pandas

[url](https://pandas.pydata.org/pandas-docs/stable/user_guide/10min.html): https://pandas.pydata.org/pandas-docs/stable/user_guide/10min.html

## De paneles de datos al DataFrame

La función `query` es mucho mejor que los el otro tipo de filtros porque aumenta la legibilidad del código. Ejemplo:

```python
df.query( 'edad >= 12 and pais == "mx" ')
```

y

```python
df[(df['edad'] >= 12) & (df['pais'] == 'mx')]
```

Muestran los mismos resultados pero el primero es mucho más legible que el segundo.

Algunas funciones muy útiles cuando arrancas a analizar un set de datos luego de tenerlo como un dataframe(df) son:

- `df.info()` te devuelve un resumen con la estructura de las variables y el tipo de datos que contienen.
- `df.describe()` te devuelve un summary() de tu set de datos. Si tienes variables numericas te devolvera: minimo. maximo, media,std,…etc de las columnas numericas.

## Cómo usar Pandas y Python para conectar con tu base de datos SQL

Pandas cuenta con una funcionalidad que facilita el acceso a tus bases de datos tipo SQL, para ello te mostrare algunos ejemplos:

### PostgreSQL

Valida que tengas la librería psycopg2 usando el comando import. Si no está instalada en tu ambiente, usa el comando !pip install psycopg2 en la terminal de python para instalarlo.

Comenzamos cargando las librerías:

```python
import pandas as pd
import psycopg2
```

Luego creamos el elemento de conexión con el siguieente código:

```python
conn_sql = psycopg2.connect(user = "user_name",
                            password = "password",
                            host = "xxx.xxx.xxx.xxx",
                            port = "5432",
                            database = "postgres_db_name")
```

Seguido simplemente definimos nuestra query en SQL:

```python
query_sql = '''
select *
from table_name
limit 10
'''
```

Y creamos nuestro dataframe:

```python
df = pd.read_sql(query_sql, sql_conn)
df.head(5)
```

### SQL Server:

Valida que tengas la librería pyodbc usando el comando import, si no está instalada en tu ambiente, usa el comando !pip install pyodbc en la terminal python para instalarlo.

Comenzamos cargando las librerías:

```python
import pandas as pd
import pyodbc
```

Luego creamos el elemento de conexión con el siguiente código:

```python
driver = '{SQL Server}'
server_name = 'server_name'
db_name = 'database_name'
user = 'user'
password = 'password'
sql_conn = pyodbc.connect('''
DRIVER={};SERVER={};DATABASE={};UID={};PWD={};
Trusted_Connection=yes
'''.format(driver, server_name, db_name, user, password))
```

O si tienes el DSN:

```python
dsn = 'odbc_datasource_name'
sql_conn = pyodbc.connect('''
DSN={};UID={};PWD={};Trusted_Connection=yes;
'''.format(dsn, user, password))
Seguido simplemente definimos nuestra query en SQL:
query_sql = 'select * from table_name limit 10'
```

Y creamos nuestro dataframe con:

```python
df = pd.read_sql(query_sql, sql_conn)
df.head(5)
```

### MySQL / Oracle / Otras:

Valida que tengas la librería sqlalchemy usando el comando import, si no está instalada en tu ambiente, usa el comando !pip install sqlalchemy en la terminal de python para instalarlo.

Comenzamos cargando las librerías:

```python
import pandas as pd
import sqlalchemy as sql
Escogemos nuestra base de datos, Oracle, MySql o la de tu preferencia:
database_type = 'mysql'
database_type = 'oracle'
```

Luego creamos el elemento de conexión con el siguiente código:

```python
user = 'user_name'
password = 'password'
host = 'xxx.xxx.xxx.xxx:port'
database = 'database_name'

conn_string = '{}://{}:{}@{}/{}'.format(
database_type, user, password, host, database)

sql_conn = sql.create_engine(conn_string)
```

Seguido simplemente definimos nuestra query en SQL:

```python
query_sql = "
select *
from table_name
limit 10
"
```

Y creamos nuestro dataframe con:

```python
df = pd.read_sql(query_sql, sql_conn)
df.head(5)
```

La libreria `sqlalchemy` también soporta PostgreSQL y otras fuentes de datos.

## Ventajas y desventajas de los formatos de importar y guardado

- **CSV** - Es muy versatil ya que solo tiene comas y saltos de linea
- **JSON** - Tiene un formato muy similar al de un diccionario de Python
- **Excel** - Permite guardar el archivo en formato .xls para trabajar con el en Excel o Spreadsheets
- **Pickle** - Permite comprimir la información, es util cuando se tienen tablas grandes
- **Parquet** - Permite darle un formato que puede usarse en ambientes de Big Data como Hadoop

## Formatos de lectura para cargar y guardar DataFrames

- **CSV y formatos String** : Son simples, requieren alto costo computacional y algo lentos.

- **HDF** : Gran soporte, adecuado para grandes cantidades de datos, rápido a costo de alto costo computacional.

- **Parquet** : Puede igualar a hdf e inclusive trabajar por chunks y en paralelo.

- **Pickle** : Es práctico pero lento con grandes cantidades de datos.

## Fixing a data type

- **object** -> Python strings (or other Python objects)
- **bool** -> enables logical and mathematical operations
- **int** -> mathematical operations can be performed
- **float** -> mathematical operations can be performed
- **category** -> results in less memory usage and faster processing
- **datetime** -> enables a rich set of date-based attributes and methods
