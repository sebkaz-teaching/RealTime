cw10D.md

# Spark Structured Streaming

## philosophy: treat data streams like unbounded tables

1. Distributed stream processing built on SQL engine
  - high throughput, second-scale latencies
  - fault-tolarance, exactly-once
  - great set of connectors

## Automatyzacja procesu - Case Study

Dane -> logical Plan -> Optimized plan -> Series of incremental execution plans

1. Data -> JSON every one minute

1000s of customer streaming apps in production produkuje 1000+ trillions wierszy
procesowanych na produkcji

jedno z rozwiązań z platformą DataBricks (Stuctured Streaming + Data Lakes)

Problemy jakie mogą wystąpić z data Lakes:
- failed production jobs
- lack of consistency
- lack of schema enforcement

Rozwiązanie : Delta Lake https://delta.io
OPEN-SOURCE storage layer - ACID transactions to Apache Spark and big data workloads. Łączy zalety hurtowni danych i data lake.


inne oprogramowanie z Apache Spark
https://streamsets.com - głównie ETL




## Podstawowe pytania przy budowie aplikacji strumieniowej (streaming design patterns)

### What ?

#### Input
1. Jakie są dane
2. Jaki format i jaki system dla twoich danych
#### Output
1. Jakich wyników oczekujesz
2. Jakiej wydajności i opóźnienia oczekujesz

### Why ??
1. czemu output w taki sposób
2. kto będzie działał na outpucie
3. kiedy i jak dane będą konsumowane

### How?
1. jak procesować dane
2. Jak zapisać rezultaty


## P1 ETL
1. What ? - konwersja nieustrukturyzowanych danych do postaci tabelowej - few minutes
2. Why ? - Zapytania natychmiastowe czy jako zadania periodyczne
3. How? - Process: użyj zapytań na strumieniu w celu transformacji danych - uruchomione 24/7
Store: zapisz ustrukturyzowane dane do skalowanej bazy: parquet, ORC lub data lake.

## P2.1 Key-value output for lookup
1. What ? - generate updated values for key - seconds/minutes
2. Why ? - Lookup latest values for key
3. How? - Process: użyj structured streaming z stanowymi operacjami agregacji
Store: zapisz w bazie klucz-wartosc



# Narzędzia chmurowe do przetwarzania strumieniowego

## Databricks

1. [AWS](https://docs.databricks.com/?_ga=2.90302814.592354964.1621514203-1965540573.1621081098)
2. [Azure](https://docs.microsoft.com/en-us/azure/databricks/)
3. [Google](https://docs.gcp.databricks.com)
