---
layout: page
title: 03 -- MS Azure Studio with Python 
mathjax: true
---


# Implementacja algorytmów uczenia maszynowego w celach klasyfikacji

Wszystkie programy i kody będziemy pisać w języku python. 

Wszystkie potrzebne środowiska uruchomieniowe (python, jupyter notebook, spark etc.) będą dostępne za pośrednictwem kontenerów dockera. 

1. Obiektowe programowanie w pythonie
2. Wdrożenie modelu na środowisko produkcyjne z wykorzystaniem biblioteki Flask



[Pobierz notatnik](notebooks/cw1.ipynb) i zapisz w swoim katalogu (np. notebooks). 
Następnie w konsoli uruchom dockera z jupyterem.
```{bash}
docker pull sebkaz/docker-data-science
docker run -d -p 8888:8888 -v "<wybrany katalog>:/opt/notebooks" --name jupyter sebkaz/docker-data-science
```

