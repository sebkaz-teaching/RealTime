---
layout: page
title: Syllabus
---

> Nazwa przedmiotu: Analiza danych w czasie rzeczywistym
> 
> Jednostka: SGH w Warszawie
> 
> Kod przedmiotu: 
>
> Punkty ECTS: 3
>
> Język prowadzenia: polski
>
> Poziom przedmiotu: średnio-zaawansowany
>
> Poniedziałek (08:00 - 9:30 wykład, 09:45 - 11:15 ćwiczenia)

**Prowadzący:** [Sebastian Zając](https://sebastianzajac.pl), 
  [sebastian.zajac@sgh.waw.pl](mailto:sebastian.zajac@sgh.waw.pl)


**Website:**
  [http://sebkaz.github.io/RealTime](https://sebkaz.github.io/RealTime)



## Cel Przedmiotu

Podejmowanie prawidłowych decyzji na podstawie danych i ich analiz w biznesie to proces i codzienność. 
Nowoczesne metody modelowania przez uczenie maszynowe (ang. machine learning), sztuczną inteligencję (AI), bądź głębokie sieci neuronowe (ang. deep learning) pozwalają nie tylko na lepsze rozumienie biznesu, ale i wspomagają podejmowanie kluczowych dla niego decyzji. 
Rozwój technologii oraz coraz to nowsze koncepcje biznesowe pracy bezpośrednio z klientem wymagają nie tylko prawidłowych, ale i odpowiednio szybkich decyzji. 
Oferowane zajęcia mają na celu przekazanie studentom doświadczenia oraz kompleksowej wiedzy teoretycznej w zakresie przetwarzania i analizy danych w czasie rzeczywistym oraz zaprezentowanie najnowszych technologii informatycznych (darmowych oraz komercyjnych) służących do przetwarzania danych ustrukturyzowanych (pochodzących np. z hurtowni danych) jak i nieustrukturyzowanych (np. obrazy, dźwięk, strumieniowanie video) w trybie on-line. W toku zajęć przedstawiona zatem zostanie filozofia analizy dużych danych w czasie rzeczywistym jako część koncepcji Big Data w połączeniu ze strumieniowaniem danych, programowaniem strumieniowym w języku Python, R oraz SAS. Zostanie przedstawiona tzw. struktury lambda oraz kappa służące do przetwarzania danych w data lake wraz z omówieniem problemów i trudności jakie spotyka się w realizacji modelowania w czasie rzeczywistym dla dużej ilości danych. Wiedza teoretyczna zdobywana będzie (oprócz części wykładowej) poprzez realizację przypadków testowych w narzędziach takich jak Apache Spark, Nifi, Microsoft Azure, czy SAS. Na zajęciach laboratoryjnych studenci korzystać będą z pełni skonfigurowanych środowisk programistycznych przygotowanych do przetwarzania, modelowania i analizy danych. Tak aby oprócz umiejętności i znajomości technik analitycznych studenci poznali i zrozumieli najnowsze technologie informatyczne związane z przetwarzaniem danych w czasie rzeczywistym.

## Program przedmiotu

1. Modelowanie, uczenie i predykcja w trybie wsadowym (offline learning) i przyrostowym (online learning). Problemy przyrostowego uczenia maszynowego.
2. Modele przetwarzania danych w Big Data. Od plików płaskich do Data Lake. Mity i fakty przetwarzania danych w czasie rzeczywistym.
3. Systemy NRT (near real-time systems), pozyskiwanie danych, streaming, analityka.
4. Algorytmy estymacji parametrów modelu w trybie przyrostowym. Stochastic Gradient Descent.
5. Architektura Lambda i Kappa. Zaprojektowanie architektury IT dla przetwarzania danych w czasie rzeczywistym.
6. Przygotowanie mikroserwisu z modelem ML do zastosowania produkcyjnego.
7. Strukturyzowane i niestrukturyzowane dane. Relacyjne bazy danych i bazy NoSQL
8. Agregacje i raportowanie w bazach NoSQL (na przykładzie bazy Cassandra).
9. Podstawy obiektowego programowania w Pythonie w analizie regresji liniowej, logistycznej oraz sieci neuronowych z wykorzystaniem biblioteki sklearn, TensorFLow i Keras
10. Architektura IT przetwarzania Big Data. Przygotowanie wirtualnego środowiska dla Sparka. Pierwszy program w PySpark. Wykorzystanie przygotowanego środowiska do analizy danych z serwisu Twitter.
11. Analiza 1 Detekcja wyłudzeń w zgłoszeniach szkód samochodowych w czasie rzeczywistym z wykorzystaniem przygotowanego, darmowego środowiska. Cz 1.
12. Analiza 1 Detekcja wyłudzeń w zgłoszeniach szkód samochodowych w czasie rzeczywistym z wykorzystaniem przygotowanego, darmowego środowiska. Cz 2.
13. Przygotowanie środowiska Microsoft Azure. Detekcja anomalii i wartości odstających w logowanych zdarzeniach sieci Ethernet cz 1.
14. Analiza 2 Detekcja anomalii i wartości odstających w logowanych zdarzeniach sieci Ethernet cz 2. Inne narzędzia IT do szybkiej analizy logów.
15. Narzędzia SAS do strumieniowego przetwarzania danych

## Efekty kształcenia

1. Wiedza:

- Zna historię i filozofię modeli przetwarzania danych
Powiązania: (Analiza danych - Big Data)K2A_W01, (Analiza danych - Big Data)K2A_W03, (OGL)O2_W01, (OGL) O2_W02, (OGL)O2_W04, (OGL)O2_W07
Metody weryfikacji: kolokwium pisemne (pytania otwarte, zadania)
Metody dokumentacji: wykaz pytań z kolokwium

- zna typy danych ustrukturyzowanych jak i nieustrukturyzowanych
Powiązania: (Analiza danych - Big Data)K2A_W02, (Analiza danych - Big Data)K2A_W04, (OGL)O2_W04, (OGL) O2_W07
Metody weryfikacji: projekt

- Metody dokumentacji: prace pisemne studenta ( w trakcie semestru, zaliczeniowe, egzaminacyjne)
Znać możliwości i obszary zastosowania procesowania danych w czasie rzeczywistym
Powiązania: (Analiza danych - Big Data)K2A_W01, (Analiza danych - Big Data)K2A_W02, (OGL)O2_W01, (OGL) O2_W04, (OGL)O2_W08
Metody weryfikacji: egzamin pisemny (pytania otwarte, zadania)
Metody dokumentacji: wykaz pytań egzaminacyjnych

- Zna teoretyczne aspekty struktury lambda i kappa
Powiązania: (Analiza danych - Big Data)K2A_W03, (Analiza danych - Big Data)K2A_W05, (OGL)O2_W04, (OGL) O2_W06, (OGL)O2_W08
Metody weryfikacji: kolokwium pisemne (pytania otwarte, zadania)
Metody dokumentacji: wykaz pytań z kolokwium

- Umie wybrać strukturę IT dla danego problemu biznesowego
Powiązania: (Analiza danych - Big Data)K2A_W02, (Analiza danych - Big Data)K2A_W03, (OGL)O2_W01, (OGL) O2_W04, (OGL)O2_W06, (OGL)O2_W08
Metody weryfikacji: projekt
Metody dokumentacji: prace pisemne studenta ( w trakcie semestru, zaliczeniowe, egzaminacyjne)

- Rozumieć potrzeby biznesowe podejmowania decyzji w bardzo krótkim czasie
Powiązania: (Analiza danych - Big Data)K2A_W01, (Analiza danych - Big Data)K2A_W05, (OGL)O2_W01, (OGL) O2_W04, (OGL)O2_W06, (OGL)O2_W08
Metody weryfikacji: projekt
Metody dokumentacji: prace pisemne studenta ( w trakcie semestru, zaliczeniowe, egzaminacyjne)

2. Umiejętności:

- Rozróżniać typy danych strukturyzowanych jak i niestrukturyzowanych
Powiązania: (Analiza danych - Big Data)K2A_U02, (Analiza danych - Big Data)K2A_U07, (Analiza danych - Big Data) K2A_U10, (OGL)O2_U02
Metody weryfikacji: test
Metody dokumentacji: prace pisemne studenta ( w trakcie semestru, zaliczeniowe, egzaminacyjne)

- Umie przygotować, przetwarzać oraz zachowywać dane generowane w czasie rzeczywistym
Powiązania: (Analiza danych - Big Data)K2A_U03, (Analiza danych - Big Data)K2A_U05, (Analiza danych - Big Data) K2A_U09, (OGL)O2_U02, (OGL)O2_U04
Metody weryfikacji: projekt
Metody dokumentacji: prace pisemne studenta ( w trakcie semestru, zaliczeniowe, egzaminacyjne)

 - rozumie ograniczenia wynikające z czasu przetwarzania przez urządzenia oraz systemy informatyczne
Powiązania: (Analiza danych - Big Data)K2A_U01, (Analiza danych - Big Data)K2A_U07, (Analiza danych - Big Data) K2A_U11, (OGL)O2_U02
Metody weryfikacji: projekt
Metody dokumentacji: prace pisemne studenta ( w trakcie semestru, zaliczeniowe, egzaminacyjne)
zastosować i skonstruować system do przetwarzania w czasie rzeczywistym
Powiązania: (Analiza danych - Big Data)K2A_U05, (Analiza danych - Big Data)K2A_U10, (OGL)O2_U05, (OGL) O2_U06, (OGL)O2_U07
Metody weryfikacji: projekt
Metody dokumentacji: prace pisemne studenta ( w trakcie semestru, zaliczeniowe, egzaminacyjne)

- umie przygotować raportowanie dla systemu przetwarzania w czasie rzeczywistym
Powiązania: (Analiza danych - Big Data)K2A_U02, (Analiza danych - Big Data)K2A_U08, (Analiza danych - Big Data) K2A_U10, (OGL)O2_U06, (OGL)O2_U07
Metody weryfikacji: projekt
Metody dokumentacji: prace pisemne studenta ( w trakcie semestru, zaliczeniowe, egzaminacyjne)

3. Kompetencje:

- formułuje problem analityczny wraz z jego informatycznym rozwiązaniem
Powiązania: (Analiza danych - Big Data)K2A_K01, (Analiza danych - Big Data)K2A_K03, (OGL)O2_K02, (OGL) O2_K06, (OGL)O2_K07
Metody weryfikacji: projekt, prezentacja
Metody dokumentacji: prace pisemne studenta ( w trakcie semestru, zaliczeniowe, egzaminacyjne)

- utrwala umiejętność samodzielnego uzupełniania wiedzy teoretycznej jak i praktycznej w zakresie programowania,
modelowania, nowych technologii informatycznych z wykorzystaniem analizy w czasie rzeczywistym. Powiązania: (Analiza danych - Big Data)K2A_K02, (Analiza danych - Big Data)K2A_K04, (OGL)O2_K01, (OGL) O2_K02, (OGL)O2_K05, (OGL)O2_K06
Metody weryfikacji: projekt
Metody dokumentacji: prace pisemne studenta ( w trakcie semestru, zaliczeniowe, egzaminacyjne)



## Harmonogram

Zajęcia odbywają się w **...** w ....

- **Wykład** od 8:00 do 9:30
- **Laboratorium** od 9:45 do 11:15


data        | Wykład      | ćwiczenia
------------|-------------|-----------------
Luty 21  |Wiadomości wstępne | |
Luty 28 |  Software installation, Python cz1| 
Marzec 7  | Big Data  | Python cz2| 
Marzec 15  | TBA  


## Realizacja przedmiotu

- egzamin testowy 40%
- kolokwium 20%
- referaty/eseje 40%

## Literatura

1. Frątczak E., red. "Modelowanie dla biznesu, Regresja logistyczna, Regresja Poissona, Survival Data Mining, CRM, Credit Scoring". SGH, Warszawa 2019. 
2. Frątczak E., red., "Zaawansowane metody analiz statystycznych", Oficyna Wydawnicza SGH, Warszawa 2012. 
3. Rubach P., Zając S., Jastrzebski B., Sulkowska J.I. , Sulkowski P., "Genus for biomolecules", Web Server, Nucleic Acids Research, 2019. 
4. Zając S., Piotrowski E. W., Sładkowski J., Syska J., "The method of the likelihood and the Fisher information in the construction of physical models" Phys. Status Solidi B vol. 246 No 5 (2009) 
5. Indest A., Wild Knowledge. Outthik the Revolution. LID publishing.com 2017. 
6. Real Time Analytic. "The Key to Unlocking Customer Insights & Driving the Customer Experience". Harvard Business Review Analytics Series, Harvard Business School Publishing, 2018. 
7. Svolba G., "Applying Data Science. Business Case Studies Using SAS". SAS Institute Inc., Cary NC, USA, 2017. 
8. Ellis B. "Real-Time Analytics Techniques to Analyze and Visualize Streaming data." , Wiley, 2014 
9. Familiar B., Barnes J. "Business in Real-Time Using Azure IoT and Cortana Intelligence Suite" Apress, 2017

## Literatura uzupełniająca

1. Frątczak E., "Statistics for Management & Economics" SGH, Warszawa, 2015 
2. Simon P., "Too Big to IGNORE. The Business Case for Big Data", John Wiley & Sons Inc., 2013 
3. Nandi A. "Spark for Python Developers", 2015 
4. Frank J. Ohlhorst. "Big Data Analytics. Turning Big Data into Big Money". John Wiley & Sons. Inc. 2013 
5. Russell J. "Zwinna analiza danych Apache Hadoop dla każdego", Helion, 2014 
6. Todman C., "Projektowanie hurtowni danych, Wspomaganie zarządzania relacjami z klientami", Helion, 2011

