---
layout: page
title: Ćwiczenia 6 - Spark Streaming
mathjax: true
---

## Streaming

- dane generowane są w sposób ciągły z jednego lub kilku źródeł
- Źródła wysyłają dane ,,równocześnie''
- Dane przychodzą w małych porcjach (kilobyte)

1. Wiele aplikacji używa danych aktualizowanych w sposób ciągły
    - sensory na pojazdach
    - śledzenie danych ze stron internetowych (geolokalizacja w każdej przeglądarce) obsługiwanych na komputera, tabletach, telefonach np w celu zapewnienia lepszych rekomendacji
    - monitorowanie paneli słonecznych
    - gry online

2. Popularne narzędzia strumieniowe
  - Apache Kafka
  - Apache Spark
  - Apache Flink

## Apache Spark

Narzędzia strumieniowe w Sparku rozszerzają podstawowe możliwości Sparka.


### Spark Streaming API - DStream vs Structured Streaming

1. Spark Streaming API
  - RDD based Streaming api
  - Brak możliwości użycia silnika SQL
  - Nie wspiera analizy Event Time

2. Structured Streaming
  - DataFrame based Streaming api
  - SQL Engine Optimization
  - Wspiera możliwość analiz Event time
  - rozwijany

 
