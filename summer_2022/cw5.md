---
layout: page
title: Ćwiczenia 5 - Apache Spark DataFrame
mathjax: true
---

## RDD -> DataFrame
Obiekt `SparkContext`

```python
sc
dir(sc)
sc.__dir__()
```

Tworzenie obiektu RDD i transformacje -> akcje
```python
rdd = sc.parallelize(range(20))
rdd
rdd2 = rdd.map(lambda x: x*x+2)
rdd2

rdd2.collect()
```

Funkcje 

```python
def zwroc_liste(x: float) -> list:
    return [x,x]

rdd3 = rdd.map(zwroc_liste)
rdd3.collect()

rdd4 = rdd.flatMap(zwroc_liste).take(8)
rdd4
```

Ramki z Rdd

```python
df = rdd3.toDF(['col1','col2'])
df.show()

rdd = sc.parallelize([(1,2,3,'a ba c'), (4,3,5,'tekst')])
df = rdd.toDF(['col1','col3','col2','col4'])
df.show()
df.printSchema()
```

## Nieustrukturyzowane dane w kolumnach DF 

```python
import requests
import json

def pobierz_info_slowa(slowo):
    language = 'en-gb'
    headers = {"app_id":"c7f6d128",  "app_key":"73ea2ed8109721300050137e74044fa6"}
    url_1 = "https://od-api.oxforddictionaries.com:443/api/v2/entries/"  
    url = f"{url_1}{language}/{slowo.lower()}"
    return requests.get(url, headers=headers)


odp = pobierz_info_slowa('streaming')

```

One Word
```python
odp.text
odp.headers
odp.json()
# ======== RDD ================
odp_rdd = sc.parallelize([odp.text])
# ========= DF ==============
df = spark.read.json(odp_rdd)
df.show(truncate=False)
```
```python
words = ['cogent','digress','dog','cat','diligent','obscure']
lista_odp = [pobierz_info_slowa(word).text for word in words ]
json_rdd = sc.parallelize(lista_odp)
json_df = spark.read.json(json_rdd)
json_df.show()
# for databricks
display(json_df)
json_df.printSchema()
```

Pola `struct` i `array`.

1. Struct
```python
json_df.select('metadata.*').printSchema()
```
2. array
   
```python
json = """
{
"num": [1,2,3,4]
}
"""
df1 = spark.read.json(sc.parallelize([json]))
df1.printSchema()
display(df1)

from pyspark.sql.functions import *

df1.select(explode('num').alias('number')).show()

json_df.select(explode('results').alias('wynik')).show()

json_df.createOrReplaceTempView('slownik')
```

flat df

```python
flat_df = spark.sql("""
select id , b
from slownik
lateral view outer explode(results)tmp1 as a
lateral view outer explode(a.lexicalEntries)tmp2 as b
""")

flat_df.printSchema()


flat_df3 = spark.sql("""select id as word,
a.language, definitions as definition,
examples.text as example
from slownik
lateral view outer explode(results)tmp1 as a
lateral view outer explode(a.lexicalEntries)tmp2 as b
lateral view outer explode(b.entries)tmp3 as c
lateral view outer explode(c.senses)tmp4 as d
lateral view outer explode(d.definitions)tmp5 as definitions
lateral view outer explode(d.examples)tmp6 as examples
""")

flat_df3.show()
```
