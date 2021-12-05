---
layout: page
title: Ćwiczenia 4 - Apache Spark
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

1. **Uczenie wsadowe - batch learning**. To system w którym do jego nauki musisz wykorzystać wszystkie zapisane i już istniejące dane. Zajmuje zazwyczaj dużo czasu i zasobów - przeprowadzany w trybie offline. System wpierw jest uczony, a następnie zostaje wdrożony do cyklu produkcyjnego i już więcej nie jest trenowany (korzysta tylko ze zdobytych wcześniej informacji). Zjawisko to nazywane jest **uczeniem offline**.

Jeśli chcesz aby system uczenia wsadowego brał pod uwagę nowe dane to musisz od podstaw wytrenować nową wersję systemu przy użyciu wszystkich dostępnych danych, wyłączyć stary system i zastąpić go nowym. Na szczęście proces ten jest w pełni automatyzowalny. Jednak trzeba pamiętać, iż trenowanie nowego modelu na pełnym zbiorze danych może trwać bardzo długo (i jest dość kosztowne) stąd wymiana modeli pojawia się np raz na tydzień raz na dzień. W przypadku bardzo dużej ilości informacji system taki może szybko przestać działać - zamiast wykonywać swoje zadania będzie obliczał nowy model.

2. W procesie **uczenia przyrostowego - online learning** system trenowany jest na bieżąco poprzez sekwencyjne dostarczanie danych (pojedyncze lub minipaczki - mini-batches). Każdy krok uczenia jest szybki i mało kosztowny. Uczenie następuje w momencie pojawienia się nowych danych.

Uczenie przyrostowe sprawdza się wszędzie tam gdzie układ odbiera ciągły strumień danych (urządzenia IoT, giełda) i wymagana jest szybkie i autonomiczne dopasowanie do nowych warunków. Przydaje się również przy pracy z ograniczonymi zasobami obliczeniowymi (stare dane nie są istotne).

Dużym problemem uczenia przyrostowego jest stopniowy spadek wydajności systemu w przypadku gdy dostarczone dane przestają być prawidłowe. Np. uszkodzony czujnik, celowe zasypywanie przeglądarki danymi w celu podbicia rankingu w wynikach wyszukiwania (algorytmy wykrywania anomalii).


[Stochastic gradient descent](https://en.wikipedia.org/wiki/Stochastic_gradient_descent)

[Stochastic learning](https://leon.bottou.org/publications/pdf/mlss-2003.pdf)

## Środowisko Apache SPARK

Pierwszym środowiskiem do przetwarzania danych strumieniowych będzie Apache Spark.
Zanim jednak zmierzymy się z strumieniami poznamy narzędzie Spark wykorzystywane do analiz w trybie batch'owym dla dużych danych.


[Darmowa książka z wprowadzeniem do Sparka](https://pages.databricks.com/rs/094-YMS-629/images/LearningSpark2.0.pdf)



1. Silnik analityczny do przetwarzania danych na dużą skalę.
2. Projekt open source, od 2013 w Apache Software Foundation.
3. Napisany w Scali.
4. Udostępnia API w Java, Scala, Python, R.
5. Aplikacje możemy tworzyć  w lokalnym środowisku korzystając z interfejsów wysokiego poziomu,
  - pozwala na interaktywne korzystanie i stosowanie złożonych algorytmów,
  - wykorzystanie różnych metod przetwarzania danych: zapytania SQL, przetwarzanie tekstów, systemy uczące i przetwarzające grafy,
  - przygotowane aplikacje można uruchomić na wielu węzłach uruchomionych w ramach klastra Spark.


<img alt="Dane" src="../img/spark1.png" align="center" />


<img alt="Dane" src="../img/spark2.png" align="center" />


### Instalacja i uruchomienie

Oprogramowanie Apache Spark można uruchomić na wszystkich popularnych systemach (Win, Linux, Mac OS) jako aplikacje jednostanowiskowe.

Do realizacji projektów w środowisku jednostanowiskowym można skorzystać z platformy Jupyter w której zintegrowano program Spark w ramach narzędzia pyspark.


Oprogramowanie można zainstalować lokalnie lub korzystając z przygotowanego kontenera Docker.

**Wersja Docker**

Obraz przygotowany na zajęcia z udostępnieniem katalogu do pracy.
```{bash}
docker run -d -p 8888:8888 -v "full_path_to_your_folder:/notebooks" sebkaz/docker-spark-jupyter
```

Oficjalny obraz Sparka z jupyter notebookiem.
```{bash}
docker run -d -p 8888:8888 jupyter/pyspark-notebook
```

Obie wersje wymagają ściągnięcia ok 2GB.

**Wersja lokalna** (komputer ze środowiskiem Python + instalacja JAVA JDK 8 !)

Środowisko Windows:
- instalacja środowiska Java JDK,
- instalacja oprogramowania Apache Spark:
    - [Ściągnij katalog](https://www.apache.org/dyn/closer.lua/spark/spark-3.1.1/spark-3.1.1-bin-hadoop2.7.tgz)
    - wypakuj ściągnięte archiwum np 7z
    - umieść w wygodnym miejscu i zapisz ścieżkę (zmień nazwę katalogu np na spark)
- środowisko do realizacji projektów w języku python:
    - uruchom jupyter notebook'a