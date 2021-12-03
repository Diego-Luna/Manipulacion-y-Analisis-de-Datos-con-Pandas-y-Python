# Manipulacion-y-Analisis-de-Datos-con-Pandas-y-Python

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

* `df.info()` te devuelve un resumen con la estructura de las variables y el tipo de datos que contienen.
* `df.describe()` te devuelve un summary() de tu set de datos. Si tienes variables numericas te devolvera: minimo. maximo, media,std,…etc de las columnas numericas.