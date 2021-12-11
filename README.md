# Manipulacion-y-Analisis-de-Datos-con-Pandas-y-Python

[Dataset Search](https://datasetsearch.research.google.com/)
[kaggle.com](https://www.kaggle.com/)
[World Bank Open Data](https://data.worldbank.org/)

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

## Funciones más complejas y lambdas

Además de apply, también se pueden usar las funciones applymap y map, dependiendo de la necesidad.

- `apply()` se utiliza para aplicar una función a lo largo de un eje (columna o fila).
- `applymap()` se usa para aplicar una función a todos los elementos del DataFrame
- `map()` se usa para sustituir cada valor de una fila por otro valor.
  Un ejemplo del uso de map() sería:

![imgs time](./imgs/img_1.png)

## Cómo trabajar con variables tipo texto en Pandas

Pandas cuenta con una gran funcionalidad a la hora de interactuar con texto, es super versatil si estas interesado en crear modelos de análisis de lenguaje natural.

Comencemos cargando nuestra librería y creando un diccionario con nombres de personas.

```python
import pandas as pd
data = {'names':['Sara Moreno 34',
                 'jUAn GOMez 23',
                 'CArlos mArtinez 89',
                 'Alfredo VelaZques 3',
                 'luis Mora 56',
                 '#moonmakers 10',pd.NA]}
```

Usemos los datos del diccionario para crear nuestro DataFrame. Nuestro DataFrame contiene una columna tipo texto, con variedades de caracteres especiales, números, mayúsculas e inclusive variables nulas.

```
df = pd.DataFrame(data)
df
```

|     | names               |
| --- | ------------------- |
| 0   | Sara Moreno 34      |
| 1   | jUAn GOMez 23       |
| 2   | CArlos mArtinez 89  |
| 3   | Alfredo VelaZques 3 |
| 4   | luis Mora 56        |
| 5   | #moonmakers 10      |

Para usar las funciones asociadas a texto usamos str en nuestro DataFrame, por ejemplo, si se quiere colocar el texto en minúscula, basta con escribir:

```python
df['names'].str.lower()
```

|     | names               |
| --- | ------------------- |
| 0   | sara moreno 34      |
| 1   | juan gomez 23       |
| 2   | carlos martinez 89  |
| 3   | alfredo velazques 3 |
| 4   | luis mora 56        |
| 5   | #moonmakers 10      |

Para mayúsculas igualmente:

```python
df['names'].str.upper()
```

```
names
0 SARA MORENO 34
1 JUAN GOMEZ 23
2 CARLOS MARTINEZ 89
3 ALFREDO VELAZQUES 3
4 LUIS MORA 56
5 #MOONMAKERS 10
6
```

O si queremos solo la primera letra en mayúscula:

```python
df['names'].str.capitalize()
```

```
	names
0	Sara moreno 34
1	Juan gomez 23
2	Carlos martinez 89
3	Alfredo velazques 3
4	Luis mora 56
5	#moonmakers 10
6
```

Para contar la longitud de nuestro texto usamos:

```python
df['names'].str.len()
```

```
0 14
1 13
2 18
3 19
4 12
5 20
6
```

Para dividir el texto por espacios usamos split y definimos el carácter por
el que queremos dividir, en este caso, un espacio vacío ' ' o '#':

```python
df['names'].str.split(' ')
```

```
names
0	[‘Sara’, ‘Moreno’, ‘34’]
1	[‘jUAn’, ‘GOMez’, ‘23’]
2	[‘CArlos’, ‘mArtinez’, ‘89’]
3	[‘Alfredo’, ‘VelaZques’, ‘3’]
4	[‘luis’, ‘Mora’, ‘56’]
5	[‘#moonmakers’, ‘10’]
6
```

```python
df['names'].str.split('#')
```

```
names
0	[‘Sara Moreno 34’]
1	[‘jUAn GOMez 23’]
2	[‘CArlos mArtinez 89’]
3	[‘Alfredo VelaZques 3’]
4	[‘luis Mora 56’]
5	[‘moonmakers 10’]
6
```

Para seleccionar los primeros o últimos 5 caracteres usamos:

```python
df['names'].str[:5]
```

```
names
0	Sara
1	jUAn
2	CArlo
3	Alfre
4	luis
5	#moon
6
```

```python
df['names'].str[-5:]
```

```
names
0	no 34
1	ez 23
2	ez 89
3	ues 3
4	ra 56
5	rs 10
6
```

Podemos reemplazar una secuencia de caracteres por otra mediante:

```python
df['names'].str.replace('Alfredo','Antonio')
```

```
names
0 Sara Moreno 34
1 jUAn GOMez 23
2 CArlos mArtinez 89
3 Antonio VelaZques 3
4 luis Mora 56
5 #moonmakers 10
6
```

También podemos buscar una secuencia de texto en específico, en este caso,
'ara':

```python
df['names'].str.findall('ara')
```

```
	names
0	[‘ara’]
1	[]
2	[]
3	[]
4	[]
5	[]
6
```

También podemos crear un filtro basándonos en una secuencia de texto en
específico, en este caso, las filas que tengan 'or':

```python
df['names'].str.contains('or')
```

```
names
0	True
1	False
2	False
3	False
4	True
5	False
6
```

Así mismo, podemos contar el número de ocurrencias de un caracter en específico,
por ejemplo, cuántas veces aparece la letra 'a':

```python
df['names'].str.lower().str.count('a')
```

```
names
0	2
1	0
2	0
3	1
4	1
5	1
6
```

Existen comandos más avanzados usando Regex, por ejemplo, si quiero extraer los
caracteres numéricos:

```python
df['names'].str.extract('([0-9]+)', expand=False)
```

```
names
0	34
1	23
2	89
3	3
4	56
5	10
6	nan
```

O, por ejemplo, si quiero extraer las menciones '@xxxx' del texto:

```python
df['names'].str.replace('@[^\s]+','')
```

```
names
0	Sara Moreno 34
1	jUAn GOMez 23
2	CArlos mArtinez 89
3	Alfredo VelaZques 3
4	luis Mora 56
5	#moonmakers 10
6
```
