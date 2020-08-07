---
layout: page
title: 01 -- Czego będziemy się uczyć ?
mathjax: true
---

# Wszędzie dane

Rozwój technologii generowania, gromadzenia i przetwarzania danych oraz upowszechnienie systemów komputerowych (spadek cen i wzrost świadomości użytkowników) oznacza olbrzymi wolumen danych do przechowywania. Narzędziami cyfrowego generowania danych są między innymi kody kreskowe, karty płatnicze, aparaty cyfrowe, email, sieci RFID, edytory tekstu itp.

**Raporty o danych:**

- W 2011 powstało $1.8$ zettabajtów danych (12 miliardów dwugodzinnych filmów HD).
- firma IDC Digital Universe przeprowadziła badania *Extracting Value from Chaos*  gdzie odnotowano wzrost cyfrowych danych ponad dwukrotnie co dwa lata (szybciej niż prawo Moore'a).
- $400 000$ TB emaili rocznie (biblioteka kongeru USA to $\sim 10$ TB - 30 mln książek)
- $\sim 40\%$ wszystkich danych na świecie pochodzi z USA
- Serwis Google przetwarza ponad 20 petabajtów danych dziennie
- $90\%$ wszystkich danych powstało w ostatnich dwóch latach. 

## Gdzie zostawiasz najwięcej informacji o sobie ?

Mechanizm wyszukiwania Google każdego dnia przetwarza setki milionów zapytań. Każde takie zapytanie reprezentuję jedną transakcję gdzie użytkownik wskazuje jakich informacji potrzebuje. Jak myślisz ? jaką nową i użyteczną wiedzę można nabyć analizując tak ogromny zbiór zapytań ?



### Główne źródła danych

- Działalność przedsiębiorstw i instytucji (banki, ubezpieczalnie, sieci handlowe, urzędy ...). Sieć sprzedaży Wal-Mart gromadzi dziennie dane dotyczące 20 milionów trnasakcji. AT&T obsługuje miliardy połączeń dziennie. Danych jest tyle, że nie zapisuje się ich a analizy prowadzone są 'on the fly'.
- Ośrodki naukowe: $10^9$ rekordów danych astronomicznych, $10^2 \sim 10^3$ atrybutów w systemach diagnozy medycznej. Very Long Baseline Interferometry posiada 16 teleskopów, gdzie każdy produkuje 1 Gigabit/sec danych astronomicznych w czasie 25 dniowej sesji obserwacyjnej.
- Baza danych [BrainMaps](http://brainmaps.org) zawiera ponad 50 TB danych z mapami mózgów ssaków.

Wiele firm i przedsiębiorstw, instytucji oraz ośrodków naukowych dysponuje bazami i hurtowniami danych o rozmiarach $20-30$ TB.

## WWW jako źródło danych

Jednym z największych źródeł danych jest obecnie **sieć WEB** zawierająca ponad 40 miliardów zaindeksowanych stron wg. [WorldWideWebSize.com](http://www.worldwidewebsize.com). Rok temu **at least 4.49 billion pages** (Friday, 16 February, 2018). Obecnie **at least 5.55 billion pages** (Thursday, 14 February, 2019).

### W  jakim celu przechowuje się tak olbrzymie wolumeny danych ?

Niewielka część gromadzonych danych analizowanych jest w praktyce !!!

W jaki sposób efektywnie i racjonalnie wykorzystać zgromadzone dane do celów wspomagania działalności biznesowej ?

Czy można wykorzystać dane transakcyjne aby zwiększyć sprzedaż i poprawić rentowność ?

# Trochę historii

- Lata 60-te : Kolekcje danych, bazy danych, sieciowe DNBMS
- Lata 70-te : Relacyjne modele danych i ich implementacja w systemach OLTP
- Lata 80-te : Zaawansowane modele danych, extended-relational, objective oriented, aplikacyjno-zorientowane itp.
- Lata 90-te : Data mining, hurtownie danych, multimedialne bazy danych, systemy OLAP
- Później : NoSQL, Hadoop, SPARK, data lake

## Modele przetwarzania danych

Większość danych przechowywana jest w bazach lub hurtowniach danych. Standardowo dostęp do danych sprowadza się najczęściej do realizacji zapytań poprzez aplikacje. Sposób wykorzystania i realizacji procesu dostępu do bazy danych nazywamy **modelem przetwarzania**.

### Model Tradycyjny

**Model tradycyjny** - przetwarzanie transakcyjne w trybie on-line, OLTP (on-line transaction processing). Świetnie sprawdza się  w przypadku obsługi bieżącej np. obsługa klienta, rejestr zamówień, obsługa sprzedaży itp. 

Model ten dostarcza efektywnych rozwiązań do:

- efektywne i bezpieczne przeowywanie danych,
- transakcyjne odtwarzanie danych po awarii,
- optymalizacja dostępu do danych,
- zarządzanie współbierznością,
- inne.

A co w przypadky gdy mamy doczynienia z:

- agregacjami danych,
- wspomaganie analizy danych, 
- raportowanie i podsumowania danych,
- optymalizacja złożonych zapytań,
- wpomaganie decyzji biznesowych. 

Badania nad tego typu zagadnieniami doprowadziły do sformułowania nowego modelu przetwarzania danych oraz nowego typu baz danych - **Hurtownie Danych** _(Data warehouse)_.

### OLAP

**Przetwarzanie analityczne on-line OLAP (on-line analytic processing).**
 Wspieranie procesów analizy i dostarczanie narzędzi umożliwiających analizę wielowymiarową (czas, miejsce, produkt).

 Analiza danych z hurtowni to przede wszystkim obliczanie **agregatów** (podsumowań) dotyczących wymiarów hurtowni. Proces ten jest całkowicie sterowany przez użytkownika.

**Przykład**

Załóżmy, że mamy dostęp do hurtowni danych gdzie przechowywane są informacje dotyczące sprzedaży produktów w supermarkecie. Jak przeanalizować zapytania:

1. Jaka jest łączna sprzedaż produktów w kolejnych kwartałach, miesiącach, tygodniach ?
2. Jaka jest sprzedaż produktów z podziałem na rodzaje produktów ?
3. Jaka jest sprzedaż produktów z podziałem na oddziały supermarketu ?

Odpowiedzi na te pytania pozwalają określić „wąskie gardła” sprzedaży produktów przynoszących deficyt, zaplanować zapasy w magazynach czy porównać sprzedaż różnych grup w różnych oddziałach supermarketu.

## OLAP $\to$ Data Mining

OLAP to : analiza danych hurtowni sterowana całkowicie przez użytkownika. Użytkownik formułuje zapytania i dokonuje analizy. Rozszerzenie standardu języka dostępu do baz danych SQL o możliwość efektywnego przetwarzania złożonych zapytań zawierających agregaty.

### Wady OLAP

- Oferowanie zbyt szczegółowego poziomu abstrakcji
- Brak możliwości formułowania bardziej ogólnych zapytań, np. Jakie czynniki kształtują popyt? czym różnią się klienci w sklepie A od klientów w sklepie B? jakie produkty kupowane są wraz z piwem ? czy można przewidzieć popyt na określone produkty ? jakie są ogólne korelacje sprzedaży ze względu na lokalizacje i asortyment ? ...
- Brak automatyzacji procesu analizy oraz ograniczony zakres analizy.

### Rozwiązanie ?

# EKSPLORACJA DANYCH (Data Mining)

## Data Mining Definicja pojęć

## Definicja 1

> Eksploracja danych jest procesem odkrywania znaczących nowych powiązań, wzorców i trendów przez przeszukiwanie zgromadzonych danych przy wykorzystaniu metod rozpoznawania wzorców, jak również metod statystycznych i matematycznych.

## Definicja 2

> Eksploracja danych jest między dyscyplinarną dziedziną, łącząca techniki uczenia maszynowego, rozpoznawania wzorców, statystyki, bez danych i wizualizacji w celu uzyskiwania informacji z dużych baz danych.

Wszystkie definicje wskazują, iż celem eksploracji danych jest odkrywanie zależności które nie były wcześniej znane odbiorcy.

**W wyniku realizacji procesu eksploracji danych otrzymujemy:**

## MODEL lub WZORZEC

Modelami mogą być:

- równania liniowe,
- reguły,
- skupienia,
- grafy,
- struktyry drzewiaste,
- wzory rekurencyjne w szeregach czasowych.

Rola badacz DM - jak znaleźć informacje, których wzorce są nieznane i mogą być użyteczne ?

# Zakres wiedzy analityka danych

Analiza danych to proces w którym dane zostają uszeregowane i przeorganizowane tak by dzięki różnym metodom wyjaśnić przeszłość i przewidzieć przyszłość (Fizyka ?)
To nauka o zadawaniu pytań, odkrywaniu wyjaśnienia i stawiania hipotez. Miesza ze sobą takie dziedziny jak:

![Umiejetnosci](/img/rys1.png).

[Jak działa Data Science](https://www.youtube.com/watch?v=tKa0zDDDaQk).

[Jak uczyć się Data Science](https://www.youtube.com/watch?v=Ura_ioOcpQI).

[Kurs Machine Learning - Andrew Ng, Stanford](https://www.youtube.com/watch?v=PPLop4L2eGk&list=PLLssT5z_DsK-h9vYZkQkYNWcItqhlRJLN)

## Matematyka

Algebra liniowa - wektory, macierze, pochodne, wartości i wektory własne, metody numeryczne, prawdopodobieństwo.

## Statystyka

Metody zbierania, analizy i interpretacji danych.

## Computer Science

Nauka tworzenia narzędzi do analizy danych. Potrzebna wiedza dotyczy: _programowania_, _zarządzania bazami dancyh_, _zarządzanie sieciami_, _systemy dyskowe_, _high-performance computing (Python)_.

## Artificial intelligence (AI)

Algorytmy symulujące proces uczenia się maszyn.

## Machine Learning

Uczenie komputerowych algorytmów.

## Wizualizacja danych

Dobra prezentacja danych pozwala szybko odczytać ważne informacje

## Wiedza ekspercka

Znajomość analizowanej dziedziny: finanse, administracja, biznes, nauka itp.

Realizacja zadań nauki o danych wymaga wielu specjalistów.

# Co po eksploracji danych ? 

## Uczenie maszynowe (Machine Learning)

Za pomocą ML identyfikujemy procesy, dzięki którym zdobywamy wiedzę nie zawsze możliwą do bezpośredniego wnioskowania z danych, a jednocześnie przydatną do podejmowania decyzji. Jest narzędziem stosowanym do wieloskalowego przetwarzania danych i świetnie nadaje się do obsługi złożonych zbiorów danych. Zdolność predykcji modeli ML wykorzystuje się bardzo często do systemów sztucznej inteligencji (**AI**).

1. Uczenie nadzorowane (_supervised_)
2. Uczenie nienadzorowane (_unsupervised_)
3. Uczenie przez wzmocnienie (_reinforcement learning_)

W pythonie uczeniu maszynowemu poświęcony jest pakiet _scikit-learn_. 

### Algorytmy

- regresja liniowa
- regresja logistyczna
- drzewa decyzyjne
- k-średnie
- naiwny Bayes
- ...

## Sieci neuronowe 

Perceprton jako jednowarstwowa sieć w przód.

>> Zadanie 1
>> Kiedy powstała pierwsza sieć neuronowa (Perceptron) ?

>> Zadanie 2
>> Przypomnij sobie własności iloczynu skalarnego

## Deep learning


# Zastosowania praktyczne 

1. _AlphaGo_ to maszyna ze sztuczną intelignecją bazującą na uczeniu głębokim, która w 2016 roku pokonała mistrza w grze Go (Lee Sedol).

2. 2009 - Netflix system rekomendacji (nagroda 1 000 000 dolarów)

3. 2013 rok - wykorzystanie metod uczenia maszynowego do wykrywania i identyfikacji ptaków na podstawie danych dźwiękowych.

4. 2015 - Na podstawie dancyh o pogodzie, terenie, badaniach i opryskach przewidzieć kiedy i gdzie określone gatunki komarów przejdą pozytywny wynik  testu na roznoszenie wirusa Zachodniego Nilu.

5. 2015 - Prognoza cen wynajmu mieszkań w Australii Zachodniej

6. 2016 - Przyśpieszenie procesu zarządzania reklamacjami

7. inne - pojazdy bezzałogowe, system rozpoznawania celów, systemy czytające notatki lekarskie, systemy wykrywania twarzy

