---
layout: page
title: Ćwiczenia 7 - Spark SQL, Streaming
mathjax: true
---

# Przetwarzanie tekstu

### klasyczny python

```{python}
with open("README.md") as plik:
    count=0
    for line in plik:
        if 'spark' in line.lower():
            count += 1

print(count)
```

### wersja spark

Zapisz plik w jupyter notebooku

```{python}
%%file spark1.py

from pyspark.sql import SparkSession

if __name__ == "__main__":

    spark = SparkSession.builder\
        .appName("spark1")\
        .getOrCreate()
    sc = spark.sparkContext
    sc.setLogLevel("ERROR")
    lines = sc.textFile('README.md')
    spark_count = lines.filter(lambda x: 'spark' in x.lower())
    print(f"Masz {spark_count.count()} wierszy z wyrazem spark")
```

uruchomimy go z konsoli, wykorzystując komórkę jupyter notebooka

```{python}
! spark-submit spark1.py
```

### Data Frame

Otrzymanie obiektu `spark` i `sc`
```{python}
from pyspark.sql import SparkSession
from pyspark.sql import Row
from pyspark.sql import functions as f

spark = SparkSession.builder\
        .appName("spark_DF")\
        .getOrCreate()
# otrzymanie obiektu SparkContext
sc = spark.sparkContext
```
Wykorzystajmy klasę `Row`. Jej elementy zachowują się jak słowniki.

```{python}
person1 = Row(age=34, name='Adam')

person1
# Row(age=34, name='Adam') : output

person1.age, person1['name']

person1[0], person1[1]

'name' not in person1

```

Generator danych
```{python}
newPerson = Row('age', 'name')

person2 = newPerson(24,'Alicja')
person3 = newPerson(None, None)
person4 = newPerson(33, None)
person5 = newPerson(None, 'Peter')
person6 = newPerson(32, 'Peter')
person7 = newPerson(40, 'Greg')
```

Na podstawie istniejących wierszy (ustrukturyzowanych) możemy wygenerować createDataFrame
```{python}
listaPerson = [person1, person2, person3, person4, person5, person6, person7]
peopleDF = spark.createDataFrame(listaPerson)

peopleDF
# output: DataFrame[age: bigint, name: string]


peopleDF.show()
```
Istotny jest również schemat tabeli:
```{python}
peopleDF.schema
# output: StructType(List(StructField(age,LongType,true),StructField(name,StringType,true)))

peopleDF.printSchema()
```

Nowy schemat:
```{python}

from pyspark.sql.types import IntegerType, StringType, StructType, StructField
schema = StructType([StructField("V1", IntegerType()), StructField("V2", StringType())])

df = spark.createDataFrame([(1,2),(3,4),(5,6)], schema)

df.show()
```

DF $\to$ RDD

```{python}
df.rdd.collect()
[Row(V1=1, V2='2'), Row(V1=3, V2='4'), Row(V1=5, V2='6')]
df.rdd.map(tuple).collect()
[(1, '2'), (3, '4'), (5, '6')]

df.rdd.map(tuple).toDF().show()


tupRDD = sc.parallelize([(x, x+1) for x in range(5)])
tupRDD.toDF().show()
schema = StructType([StructField("A", IntegerType()), StructField("B", StringType())])
tupRDD.toDF(schema).show()
```

Podstawowe własności DataFrame

```{python}
peopleDF.columns
peopleDF.dtypes

```
### Dane bez struktury

zapisz następujący JSON
```{python}
%%file example.json

'''
{"string":"string1","int":1,"array":[1,2,3],"dict": {"key": "value1"}}
{"string":"string2","int":2,"array":[2,4,6],"dict": {"key": "value2"}}
{"string":"string3","int":3,"array":[3,6,9],"dict": {"key": "value3", "extra_key": "extra_value3"}}
'''
```
Wczytaj go jako DF

```{python}
d = spark.read.json('example.json')
d.show()
```

### Word Count

```{python}
df = spark.read.csv("RDD_input")
df.show(4)
col_names = ['line']
df = df.toDF(*col_names)
df.show(2)
words = df.select(f.explode(f.split(df.line, " ")).alias("word"))
words.show()

```
# Strumienie DF w Apache Spark

Zapisz plik aplikacji i uruchom

```{python}
%%file sparkStream.py

from pyspark.sql import SparkSession
import pyspark.sql.functions as f

if __name__ == "__main__":
    spark = SparkSession\
            .builder\
            .master("local[2]") \
            .appName("Stream_DF")\
            .getOrCreate()
    print("="*50)
    print("Zaczynamy DataFrame")
    print("="*50)
    spark.sparkContext.setLogLevel("ERROR")
    lines = spark.readStream\
            .format("socket")\
            .option('host', '127.0.0.1')\
            .option('port', 9999)\
            .load()
    words=lines.select(f.explode(f.split(lines.value, " ")).alias("word"))
    wordCount = words.groupBy("word").count()

    query = wordCount.writeStream\
            .outputMode('complete')\
            .format('console')\
            .start()

    query.awaitTermination()
    query.stop()
```

```{python}
! spark-submit sparkStream.py
```

W terminalu wejdź do narzędzia wysyłającego tekst w postaci pakietów przez StructType
```{bash}
nc -lk 9999

<<<tutaj wpisz tekst >>
```

lines = DataFrame reprezentujący nieograniczoną tabelę zawierającą dane strumieniowe.
Zawiera ona jedną kolumnę o nazwie `value`. Każda nowa linia to wiersz w tabeli.

<img src="https://spark.apache.org/docs/latest/img/structured-streaming-stream-as-a-table.png"/>
