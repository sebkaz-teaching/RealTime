---
layout: page
title: Ćwiczenia 1 - Obiektowy Python podstawy
mathjax: true
---

## Dane ustrukturyzowane w pythonie.

Proste typy danych do przechowywania informacji tabelarycznych
```python
# zmienne
klien1_wiek = 38
klien1_wzrost = 178
# lista
klient1 = [38, 178, "Kawaler", 80.9, ["","",""],{}]
```
Dane to nie tylko ich sposób przechowywania ale również możliwości operacyjne: 

```python
a = [1,2,3]
b = [2,34,5]
# a+b jaki wynik otrzymasz ? 
a+b
# [1, 2, 3, 2, 34, 5]
# a taka operacja ? 

try:
    a*b
except TypeError:
    print("nie ma takiej operacji")
```

>> Znajdź informacje jakie operacje arytmetyczne możesz wykonac na składowych tensorów oraz czym jest rozgłaszanie.

operatory działania plus:

```python
4+4
"napis" + " inny napis"
type("napis")
"napis".__add__("inny napis")

"napis". # <press tab>
"napis".__dir__()
```

### Tablice numpy 

```python
import numpy as np
np.array([1,2,3]).__dir__()
aa = np.array([1,2,3])
bb = np.array([1,2,3])
aa+bb
aa*bb
np.dot(aa,bb)
```

### Obiekty 

```python
import this

def moja_funkcja():
    pass

class Nazwa(object):
    pass

a = Nazwa()
a.__dir__()

[Nazwa() for _ in range(10)]

[t**2 for t in x]
```

```python
from random import randint
class Kosc():
    """opis"""
    def __init__(self, sciany=6):
        """ ops metody """
        self.sciany = sciany
        
    def roll(self):
        """opis metody """
        return randint(1,self.sciany)

a = Kosc()
[a.roll() for _ in range(10)]
```

