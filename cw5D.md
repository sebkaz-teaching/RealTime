---
layout: page
title: Ćwiczenia 5 - Kafka podstawy
mathjax: true
---


# Apache Kafka

**Distributed Stream Processing System**

Wieloserwerowa platforma do propagacji zdarzeń w rozproszonych systemach informatycznych.
Stworzona na potrzeby LinkedIn w 2011 roku (napisana w Scala i Java). Obecnie zarządzana przez Apache Foundation. Cechuje się znakomitą skalowalnością horyzontalną. Pozwala na budowanie systemów (na klastrach) potrafiących propagować setki tysięcy zdarzeń dziennie.

Do największych firm pracujących na Kafce należą: LinkedIn, Oracle, PayPal, Uber, Yahoo, Trivago, Twitter, Spotify, Skyscanner, Cisco, AirBnB, Adidas, Zalando czy Netflix.

Kluczową cechą i podstawową funkcjonalnością tej technologii jest możliwość publikowania (za pomocą tzw. producentów) różnego rodzaj zdarzeń. Zdarzenia te następnie mogą zostać pobrane przez jednego (lub wielu) tzw. konsumentów.


Jeśli nic z tego nie rozumiesz wyobraź sobie prostą sytuację:

Nagrywasz video z kursem, który chcesz udostępniać. Jak możesz je rozprowadzać?
- możesz nagrać je na dvd i wysyłać do odbiorców (którzy o to poproszą i dadzą adres)
- możesz załączyć video (to chyba nie za duży film) do wiadomości email (tutaj również potrzebujesz zapytania, zgody no i ... adresu email).

Teraz wyobraź sobie, że jednak wrzucasz film na YouTube - bądź inną platformę Brokerską. W tym momencie jesteś już Producentem (jednym z tysięcy).
Produkujesz swoje źródła i kontent wrzucasz na platformę. Treść może być przechowywana na jakiejś bazie. Platforma ta wykorzystywana jest przez setki tysięcy użytkowników (Konsumentów) by odtwarzać zawartość wtedy kiedy mają na to chęć. Nie potrzebujesz znać ani ich adresu email, ani fizycznego adresu na który musisz wysłać film. Platforma działa jako broker. Przechowuje setki tysięcy rzeczy produkowanych przez Producentów (push data) i udostępnia każdemu, kto wyśle zapytanie pobierające (pull data). Zauważ, że jedno źródło może być przetwarzane przez wielu konsumentów na raz. Również jeden producent może generować dane o wielu tematach.

<img src="https://docs.cloudera.com/documentation/kafka/1-2-x/images/kafka-architecture.png" />

> Poszukaj info i przykład Data Driven vs Event Driven Apps


## Instalacja i weryfikacja

1. Przejdź na [stronę Kafki](https://kafka.apache.org). Aktualna wersja stabilna `2.8.0`.
2. Ściągnij plik z Binary downloads (wersja 2.12) i rozpakuj programem 7-zip . Uwaga w systemie Win rozpakuj również archiwum tar.
3. Zmień nazwę katalogu na kafka i wrzuć go do przyjaznej lokalizacji (np na pulpit)
4. W przypadku systemu Win przejdź do katalogu config i znajdź plik server.properties. Otwórz go w dowolnym edytorze i znajdź linie odpowiedzialną za ustawienie katalogu z logami. Ustaw katalog w swojej dogodnej lokalizacji. np. `C:\\Users\\szajac2\\Desktop\\kafka\\logs`
[Instalacja WIN](https://reachmnadeem.wordpress.com/2020/08/30/kafka-2-6-up-and-running-in-windows-10/)

5. Uruchom serwer zookeepera
```{bash}
bin/zookeeper-server-start.sh config/zookeeper.properties
```
lub
```{bash}
bin\windows\zookeeper-server-start.bat config\zookeeper.properties
```
6. uruchom serwer Kafki
```{bash}
bin/kafka-server-start.sh config/server.properties
```
lub
```{bash}
bin\windows\kafka-server-start.bat config\server.properties
```
7. Stwórz topic i uruchom producenta i konsumenta w osobnych konsolach.

Zamiast kroków 5 i 6 możesz uruchomić dockera
[obraz RTA KAFKA](https://github.com/sebkaz/docker-kafka-rta)

```{bash}
docker-compose up
```



Kafka udostępnia API praktycznie we wszystkich istotnych językach programowania. Dla nas istotne jest API Pythona.

## Broker

Brokerem nazywamy pojedynczą aplikację Kafki (serwer). Podczas działania całej struktury można uruchomić wiele brokerów. Dystrybucja wiadomości jest podzielona na **partycje**. Partycje pozwalają na replikacje topików i przydają się podczas awarii któregoś z serwera. Jednostki opisywane są jako **liderzy** i **followerzy**.

## zookeeper

Apache zookeeper to scentralizowana usługa wymagana do działania Kafki. Zajmuje się konfiguracją topików, kontrolą brokerów. Pozwala wyznaczyć ich statusy i metryki.


## Kafka Topic (temat)

Temat to abstrakcja do której można publikować zdarzenia. Kafka umożliwia przetwarzanie wielu tematów w jednym systemie. Ponadto każdy temat może być obsługiwany przez wielu konsumentów pobierających zdarzenia z konkretnych tematów. Dla każdego tematu utrzymywany jest rozproszony log, w którym zapisywane są opublikowane zdarzenia. Logi zawarte na partycjach są uporządkowane czasowo i niezmienialne. Można tylko dopisywać nowe zdarzenia. Wszystkie dane przechowywane są przez określony (konfigurowalny - domyślnie 7 dni) okres czasu (retention time). Jeśli konsument nie odczyta danych do tego czasu traci możliwość jego przetworzenia. Ustawienie długiego czasu wpływa tylko i wyłącznie na ilość zużytego miejsca na dysku. Każde zdarzenie reprezentowane jest jako para 'klucz':'wartość'. Przy czym sam broker (platforma) jest pasywna - nie tworzy nowych eventów. Dane mogą pochodzić tylko od producentów a ich przetwarzaniem zajmują się tylko konsumenci.  

__Topic__ odpowiada w przetwarzaniu danych pojęciu __tabeli__.

__Message__ odpowiada w przetwarzaniu danych pojęciu __wiersza__.

__Partition__ odpowiada w przetwarzaniu danych pojęciu __widoku__.




**Kafka Connect** - zbiór gotowych adapterów pozwalających tworzyć zdarzenia na podstawie danych znajdujących się w rozwiązaniach służących do przechowywania lub przetwarzania danych tj. Active MQ, Amazon S3, ElasticSearch, Cassandra, MySQL, PostgresSQL, SQLServer czy MongoDB. Adaptery te pozwalająca zarówno na pobieranie danych jak i ich zapis.

**Kafka Stream** - rozwiązanie pozwalające na transformację zdarzeń wewnątrz platformy. Wykorzystuje język zapytań KSQL pozwalając na wykonywanie skalowalnych i elastycznych zapytań na strumieniach danych. Najczęściej wykorzystywane do normalizacji danych przychodzących z różnych źródeł.
