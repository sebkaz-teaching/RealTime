---
layout: page
title: 07 - Ćwiczenia 3 - Spark Intro
mathjax: true
---

## Podsumowanie poprzednich zajęć 

Na poprzednich dwóch zajęciach laboratoryjnych zrealizowaliśmy zagadnienia dotyczące przetwarzania danych ustrukturyzowanych i nieustrukturyzowanych w trybie wsadowym. Ponadto przygotowaliśmy środowisko produkcyjne (z wykorzystaniem biblioteki FLASK) wykorzystujące model napisany w pełni obiektowo otrzymany po wstępnym przetworzeniu danych (irys).   

1. Ustrukturyzowane dane - tablice numpy, ramki danych Pandas, tabele danych w bazach SQL (tworzenie, filtrowanie, modyfikacja)
2. Nieustrukturyzowane dane - JSON, tensory numpy (odczyt, zapis, przetworzenie)
3. Wykorzystanie obiektowego programowania w Pythonie - podstawa budowy klas, tworzenie obiektów, korzystanie z pól obiektów i metod (funkcji). 
4. Stworzenie modelu klasyfikacji binarnej opartego o sieć perceprtornu oraz wykorzystującą algorytm Adeline napisany obiektowo w pełnej analogii do modeli z biblioteki sklearn.
5. Wykorzystanie środowiska SQLAlchemy do łączenia się z bazami danych. 
6. Strona www realizująca API z wykorzystaniem modelu - nowe dane w czasie rzeczywistym + prognoza - Jako system odpytywania modelu w czasie rzeczywistym (Zastanów się jak go unowocześnić) 


Podczas przerabiania dowolnych technik uczenia maszynowego najczęściej (jeśli nie zawsze) jesteśmy uczeni realizacji zadań takiego systemu z podziałem na trzy podstawowe kategorie:

1. Uczenie nadzorowane  - supervised learning
    - klasyfikacja - zrealizowany na poprzednich ćwiczeniach
    - Regresja liniowa
2. Uczenie nienadzorowane - unsupervised learning
3. Uczenie przez wzmacnianie - reinforcement learning

Jednak systemy te można również klasyfikować ze względu na `możliwość trenowania przyrostowego przy użyciu strumienia nadsyłanych danych`

1. **Uczenie wsadowe - batch learning**. To system w którym do jego nauki musisz wykorzysać wszystkie zapisane i już istniejące dane. Zajmuje zazwyczaj dużo czasu i zasobów - przeprowadzany w trybie offline. System wpierw jest uczony, a następnie zostaje wdrożony do cyklu produkcyjnego i już więcej nie jest trenowany (korzysta tylko ze zdobytych wcześniej informacji). Zajwisko to nazywane jest **uczeniem offline**. 

Jeśli chcesz aby system uczenia wsadowego brał pod uwagę nowe dane to musisz od podstaw wytrenować nową wersję systemu przy użyciu wszystkich dostępnch danych, wyłączyć stary system i zastąpić go nowym. Na szczęście proces ten jest w pełni automatyzowalny. Jednak trzeba pamiętać, iż trenowanie nowego modelu na pełnym zbiorze danych może trwać bardzo długo (i jest dość kosztowne) stąd wymiana modeli pojawia się np raz na tydzień raz na dzień. W przypadku bardzo dużej ilości informacji system taki może szybko przestać działać - zamiast wykonywać swoje zadania będzie obliczał nowy model. 

2. W procesie **uczenia przyrostowego - online learning** system trenowany jest na bieżąco poprzez sekwencyjne dostarczanie danych (pojedyncze lub minipaczki - mini-batches). Każdy krok uczenia jest szybki i mało kosztowny. Uczenie następuje w momencie pojawienia się nowych danych. 

Uczenie przyrostowe sprawdza się wszędzie tam gdzie układ odbiera ciągły strumień danych (urządzenia IoT, giełda) i wymagana jest szybkie i autonomiczne dopasowanie do nowych warunków. Przydaje się również przy pracy z ograniczonymi zasobami obliczeniowymi (stare dane nie są istotne).

Dużym problemem uczenia przyrostowego jest stopniowy spadek wydajności systemu w przypadku gdy dostarczone dane przestają być prawidłowe. Np. uszkodzony czujnik, celowe zasypywanie przeglądarki danymi w celu podbicia rankingu w wynikach wyszukiwania (algorytmy wykrywania anomalii).


[Stochastic gradient descent](https://en.wikipedia.org/wiki/Stochastic_gradient_descent)

[Stochastic learning](https://leon.bottou.org/publications/pdf/mlss-2003.pdf)

## Środowisko Apache SPARK

[książka](https://pages.databricks.com/rs/094-YMS-629/images/LearningSpark2.0.pdf) 


1. Silnik analityczny do przetwarzania danych na dużą skalę
2. Projekt open source, od 2013 w Apache Software FOundation
3. Napisany w Scali 
4. Udostępnia API w Java, Scala, Python, R 


<img alt="Dane" src="../img/spark1.png" align="center" />


<img alt="Dane" src="../img/spark2.png" align="center" />


### Instalacja i uruchomienie 

1. Wersja trywialna (Docker) 

```{bash}
docker run -d -p 8888:8888 -v "full_path_to_your_folder:/notebooks" sebkaz/docker-spark-jupyter
```

```{bash}
docker run -d -p 8888:8888 -v "full_path_to_your_folder:/notebooks" jupyter/pyspark-notebook
```

2. Wersja trywialna trywialna (komputer ze środowiskiem Python) 

    - [Ściągnij katalog](https://www.apache.org/dyn/closer.lua/spark/spark-3.1.1/spark-3.1.1-bin-hadoop2.7.tgz)
    - Rozpakuj np 7z
    - umieść w wygodnym miejscu i zapisz ścieżkę (będzie potrzebna do findspark() )
    - uruchom jupyter notebook'a



### Jeśli nie konfigurowałeś środowiska

```{python}
import findspark
# findspark.init()
# findspark.init("C:/Users/SebastianZajac/Desktop/spark")
findspark.init("/Users/air/Desktop/spark3/")
```

### SparkContext

1. Główny, podstawowy obiekt
2. Punkt wejścia do pracy ze Sparkiem
3. Generowanie obiektów RDD

```{python}
# inicjalizacja SparkContext
from pyspark import SparkContext
sc = SparkContext(appName="myAppName")
sc
```

### SparkSession

1. Główny punkt wyjścia do SparkSQL
2. Opakowuje (wrapper) SparkContext
3. Zazwyczaj pierwszy obiekt, który będziemy tworzyć

```{python}
from pyspark.sql import SparkSession

spark = SparkSession.builder\
        .appName("new")\
        .getOrCreate()
# otrzymanie obiektu SparkContext
sc = spark.sparkContext
```

```{python}
spark
```

```{python}
sc
```

### RDD

- Resilient Distributed Dataset
- Podstawowa abstrakcja oraz rdzeń Sparka
- Obsługiwane przez dwa rodzaje operacji:
    - Akcje:
        - operacje uruchamiająceegzekucję transformacji na RDD
        - przyjmują RDD jako input i zwracają wynik NIE będący RDD
    - Transformacje:
        - leniwe operacje
        - przyjmują RDD i zwracają RDD

- In-Memory - dane RDD przechowywane w pamięci
- Immutable 
- Lazy evaluated
- Parallel - przetwarzane równolegle
- Partitioned - rozproszone 

## WAŻNE informacje !

Ważne do zrozumienia działania SPARKA:

Term                   |Definition
----                   |-------
RDD                    |Resilient Distributed Dataset
Transformation         |Spark operation that produces an RDD
Action                 |Spark operation that produces a local object
Spark Job              |Sequence of transformations on data with a final action


Dwie podstawowe metody tworzenia RDD:

Method                      |Result
----------                               |-------
`sc.parallelize(array)`                  |Create RDD of elements of array (or list)
`sc.textFile(path/to/file)`                      |Create RDD of lines from file

Podstawowe transformacje

Transformation Example                          |Result
----------                               |-------
`filter(lambda x: x % 2 == 0)`           |Discard non-even elements
`map(lambda x: x * 2)`                   |Multiply each RDD element by `2`
`map(lambda x: x.split())`               |Split each string into words
`flatMap(lambda x: x.split())`           |Split each string into words and flatten sequence
`sample(withReplacement=True,0.25)`      |Create sample of 25% of elements with replacement
`union(rdd)`                             |Append `rdd` to existing RDD
`distinct()`                             |Remove duplicates in RDD
`sortBy(lambda x: x, ascending=False)`   |Sort elements in descending order

Podstawowe akcje 

Action                             |Result
----------                             |-------
`collect()`                            |Convert RDD to in-memory list 
`take(3)`                              |First 3 elements of RDD 
`top(3)`                               |Top 3 elements of RDD
`takeSample(withReplacement=True,3)`   |Create sample of 3 elements with replacement
`sum()`                                |Find element sum (assumes numeric elements)
`mean()`                               |Find element mean (assumes numeric elements)
`stdev()`                              |Find element deviation (assumes numeric elements)

-----
- **parallelize(c)** - tworzenie RDD na podstawie lokalnej kolekcji
- **map(f)** - zwraca nowe RDD po zastosowaniu podanej funkcji na każdym elemencie oryginalnego RDD (**T**)
- **filter(f)** - zwraca nowe RDD zawierające jedynie elementy które spełniają predykat (**T**)
- **reduce(f)** - agreguje elementy zbioru wykorzystując podaną funkcję. Funkcja redukująca musi być asocjacyjna [(a x b) x c = a x (b x c)] i przemienna [a x b = b x a] (**A**)

```{python}
rdd = sc.parallelize(range(10)) # utworzenie RDD 

rdd
```

**Akcje** 

```{python}
rdd.collect()
```

```{python}
rdd.take(2)
```

```{python}
rdd.takeSample(True,3)
```

```{python}
rdd.takeSample(False,3)
```

> Zadanie - Jaka jest różnica między True i False ? 

```{python}
rdd.count()
```

```{python}
rdd.mean()
```

```{python}
%%file example.txt
first 
second line
the third line
then a fourth line
```

```{python}
text_rdd = sc.textFile('example.txt')
```

```{python}
text_rdd.first()
```


```{python}
text_rdd.take(3)
```

```{python}
text_rdd.takeSample(True,2)
```

```{python}
text_rdd.count()
```



```{python}
rdd1 = sc.parallelize(range(1,20))
rdd2 = sc.parallelize(range(10,25))
rdd3 = rdd1.union(rdd2)
rdd3.collect()
```

```{oython}
rdd4 = rdd3.distinct()
rdd4.collect()
```

-----
- **parallelize(c)** - tworzenie RDD na podstawie lokalnej kolekcji
- **map(f)** - zwraca nowe RDD po zastosowaniu podanej funkcji na każdym elemencie oryginalnego RDD (**T**)
- **filter(f)** - zwraca nowe RDD zawierające jedynie elementy które spełniają predykat (**T**)
- **reduce(f)** - agreguje elementy zbioru wykorzystując podaną funkcję. Funkcja redukująca musi być asocjacyjna [(a x b) x c = a x (b x c)] i przemienna [a x b = b x a] (**A**)


