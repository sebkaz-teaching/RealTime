---
layout: page
title: 02 -- Instalacja oprogramowania
---

# Git - system kontroli wersji

[Github](https://github.com/) to miejsce gdzie zapisywać będziemy wszystkie programy z zajęć.

Na temat systemu kontroli wersji git możesz przeczytać na [Git-scm](https://git-scm.com/book/pl/v2).

## Zadanie 1

1. Wejdź na stronę [Github](https://github.com/) i załóż konto
2. Przyciskiem + stwórz nowe repozytorium publiczne o nazwie DM# (gdzie # to nr indeksu)
3. Zgodnie z poniższą instrukcją stwórz katalog i połącz z nim utworzone repozytorium.

## Najważniejsze polecnia do zapamiętania

* _ściąganie repozytorium z sieci_

```{bash}
git clone https://adres_repo.git
```

W przypadku githuba możesz pobrać repozytorium jako plik zip.

* _Tworzenie repozytorium dla lokalnego katalogu_

```bash
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

```bash
git remote add origin https://github.com/<twojGit>/nazwa.git
```

* Obsługa w 3 krokach

```bash
# sprawdź zmiany jakie zostały dokonane
git status
# 1. dodaj wszystkie zmiany
git add .
# 2. zapisz bierzący stan wraz z informacją co zrobiłeś
git commit -m " opis "
# 3. potem już zostaje tylko
git push -u origin master
```

## Zadanie 2

Zapamiętaj! Na każdych następnych zajęciach zapisz wszystko w swoim repozytorium.

# Oprogramowanie

Nauka o danych to dość młoda dziedzina obejmująca takie aspekty jak :
algebra liniowa, modelowanie statysyczne, wizualizacja, analiza grafów, uczenie maszynowe, przechowywanie i przetwarzanie danych, analityka biznesowa.

Jednym z najlepszych narzędzi dla badaczy danych są:

* Python
* R
* SAS
* MATLAB

## Edytory tesktu

[Notepad++](https://notepad-plus-plus.org/download) , [Sublime Text](https://www.sublimetext.com), [Visual Studio Code](https://code.visualstudio.com).

## Python

Powstał w 1991 roku (Guido van Rossum) jako ogólny, _interpretowany_, _zorientowany obiektowo_ język programowania wysokiego poziomu. Od tego czasu zdobywa wielką popularność w społeczności naukowej ze względu na rozbudowany system pakietów do analizy danych i uczenia maszynowego. Ułatwia integrowanie różnych narzędzi i języków programowania np. Java, C, Fortran, R itp. Działa na wszystkich systemach operacyjnych (Win, Linux, Mac Os). Przetwarzanie dużej ilości danych wspomagane jest mechanizmami odzyskiwania pamięci. Dość prosty w nauce.

### Instalacja dystrybucji środowiska

W przypadku systemu Windows wszystkie potrzebne pliki instalacyjne znajdziesz na [www.python.org](http://www.python.org/downloads/).
Zawsze wybieraj wersję 3. Po uruchomieniu w konsoli polecenia:

```bash
python -V
```

powinieneś uzyksać informację o wersji pythona.

Pakiety doinstalowywać należy narzędziem pip.

```bash
pip install <nazwa-pakietu>
# przykład
pip install advanced-scorecard-builder
# bądź dla pakietów anacondy
conda install <nazwa-pakietu>
# update pakietów anacondy
conda update --all
```

Listę dostępnych pakietów znajdziesz na [PyPI](https://pypi.python.org/pypi).

Dla ułatwienia i szybszej instalacji pakietów i środowiska przydatnego do analiz danych zainstaluj naukową dystrybucję **Anaconda**.

## Anaconda

[Anaconda](http://continuum.io/downloads) to dystrybucja udostępniona przez firmę Continuum Analytics. Obejmuje ona prawie 200 pakietów. Najważniejsze, którymi będziemy się posługiwać to:

* [NumPy](http://www.numpy.org) - wielowymiarowe tablice reprezentujące wektory i macierze, bogaty zestaw operacji matematycznych, optymalna alokacja pamięci. 

```python
import numpy as np
```

* [SciPy](http://www.scipy.org) - uzupełnia funkcjonalność NumPyo obszar algebry liniowej, przetwarzanie sygnałów, optymalizacji, transformaty Fouriera.

* [Pandas](http://pandas.pydata.org) - dzięki strukturom danych typu _DataFrame_ i _Series_ pozwala obsługiwać złożone tablice danych z różnymi typami (nieobsługiwane w NumPy i SciPy).

```python
import pandas as pd
```

* [Scikit-learn](http://scikit-learn.org/stable) - udostępnia wszystko czego potrzeba w zakresie uczenia, wyboru modeli, walidacji itp.

* [Jupyter](http://jupyter.org) - rozszerzenie konsoli IPython do przeglądarki www (dokładne info później).

* [Matplotlib](http://matplotlib.org) - wizualizacja danych.

```python
import matplotlib.pyplot as plt
```

* [Beautiful Soup](http://wwwcrummy.com/software/BeautifulSoup) - pobieranie danych z plików html i xml.

* [Theano](http://deeplearning.net/software/theano), [Keras](http://keras.io), [TensorFlow](https://www.tensorflow.org) ([read it](https://www.datasciencecentral.com/profiles/blogs/9-great-articles-about-tensorflow))- Deep Learning.

Inne dystrybucje naukowe to: [Enthought Canopy](https://www.enthought.com/products/canopy/), [Python(x,y)](http://python-xy.github.io/).

Najczęściej ładowane pakiety w pythonie

## Zadanie 2A

<div id="zadanie2"></div>

**Zapamiętaj**

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
```

Po instalacji odpowiedniej wersji Anacondy zapoznaj się z nawigatorem i jego wszystkimi opcjami.

Bardzo dobrym rozwiązaniem przy instalacji Pythona bądź Anacondy jest korzystanie z narzędzia do środowiska wirtualnego _virtualenv_.
Więcej informacji na temat wirtualnego środowiska możesz znaleźć [tutaj](https://docs.python.org/3/tutorial/venv.html).

### Jupyter notebook

Uruchamianie notatnika:

* dla systemów Linux, Unix:

```bash
# przejdź do katalogu home
jupyter notebook
```

* dla systemów Windows:

```bash
# przejdź do katalogu home
jupyter-notebook
```

Można również wykorzystać graficzny interfejs Anaconda Navigator.

## Zadanie 3

Zapoznaj się z notatnikiem Jupyter [learn more](https://www.youtube.com/watch?v=HW29067qVWk).

## Uwaga

Do Anacondy można doinstalować środowisko R postępując zgodnie z [instrukcją](https://www.anaconda.com/developer-blog/jupyter-and-conda-r/). Ponadto możliwe jest zainstalowanie obsługi innych języków programowania takich jak `Haskel` czy `Ruby`. Wszystkie dodatkowe kernele możesz pobrać z [repo](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels)