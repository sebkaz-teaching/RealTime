---
layout: page
title: Praca z systemem GIT
---

## Zacznij korzystać z serwisu GitHub

Tekst na podstawie [strony](http://pbiecek.github.io/Przewodnik/Programowanie/jak_korzystac_z_serwisu_github_i_waffle.html)

![Final](img/final.gif)

Pracując nad projektem np. praca magisterska, (samodzielnie lub w zespole) często potrzebujesz sprawdzić jakie zmiany, kiedy i przez kogo zostały wprowadzone do projektu.
W zadaniu tym świetnie sprawdza się `system kontroli wersji` czyli [GIT](https://git-scm.com). 

Git możesz pobrać i zainstalować jak zwykły program na dowolnym komputerze.
Jednak najczęściej (małe projekty) korzysta się z serwisów z jakimś systemem git. 
Jednym z najbardziej rozpoznawanych jest [GitHub](www.github.com) dzięki któremu możesz korzystać z systemu git bez jego instalacji na swoim komputerze.

W darmowej wersji serwisu `GitHub` swoje pliki możesz przechowywać w publicznych (dostęp mają wszyscy) repozytoriach.  
Skupimy się wyłącznie na darmowej wersji serwisu GitHub.

```{bash}
git --version
```

## Struktura GitHuba

Na najwyższym poziomie znajdują się konta indywidualne (np [http://github.com/sebkaz](http://github.com/sebkaz), bądź zakładane przez organizacje. 
Użytkownicy indywidualni mogą tworzyć **repozytoria** publiczne (`public` ) bądź prywatne (`private`). 

Jeden plik nie powinien przekraczać 100 MB.

**Repo** (skrót do repozytorium) tworzymy za pomocą `Create a new repository`.
 Każde repo powinno mieć swoją indywidualną nazwę. 

### Branche

Główna (tworzona domyślnie) gałąź rapozytorium ma nazwę `master`.


### Najważniejsze polecnia do zapamiętania

* _ściąganie repozytorium z sieci_

```{bash}
git clone https://adres_repo.git
```

> W przypadku githuba możesz pobrać repozytorium jako plik zip.

* _Tworzenie repozytorium dla lokalnego katalogu_

```{bash}
# tworzenie nowego katalogu
mkdir datamining
# przejście do katalogu
cd datamining
# inicjalizacja repozytorium w katalogu
git init
# powinien pojawić się ukryty katalog .git
# dodajmy plik
echo "Info " >> README.md
```

* Połącz lokalne repozytorium z kontem na githubie

```{bash}
git remote add origin https://github.com/<twojGit>/nazwa.git
```

* Obsługa w 3 krokach

```{bash}
# sprawdź zmiany jakie zostały dokonane
git status
# 1. dodaj wszystkie zmiany
git add .
# 2. zapisz bierzący stan wraz z informacją co zrobiłeś
git commit -m " opis "
# 3. potem już zostaje tylko
git push origin master
```
Warto obejrzeć [Youtube course](https://www.youtube.com/watch?v=HVsySz-h9r4).
