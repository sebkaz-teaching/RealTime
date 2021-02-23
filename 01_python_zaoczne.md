---
layout: page
title: 01C -- Środowisko produkcyjne w obiektowym Pythonie 
mathjax: true
---

## Podstawy obiektowości

Aplikacje powinny być wytwarzane w sposób niezawodny, szybki oraz ekonomiczny.
`Obiekty` (a dokładniej `Klasy`) to jeden ze środków dzięki któremy można uzyskać ten cel. 
Obiekty można rozumieć jako _wieloużywalne_ komponenty oprogramowania (_ang. reusable_).
Potrafią realizować one rozmaite koncepcje i byty np. datę, czas, obrazy, samochody, dźwięk, ludzi etc. 
Praktycznie wszystko co określane jest jako rzeczownik, może być realizowane w kategoriach **atrybutów** obiektów. 
Natomiast zachowania obiektów, wyrażane czasownikami, można określić jako **metody** klas.
Programy oparte o obiekty są dużo łatwiejsze do zrozumienia i weryfikacji niż tzw. programowanie strukturalne. 

> Zadanie domowe - Jakie inne rodzaje programowania używane są współczenie? (zobacz np. język Scala)


Obiekt realizujący konto bankowe można wygenerować z klasy, która zapewne posiada metody reprezentujące wpłaty środków (ang. _deposit_), ich wypłatę (ang. _withdraw_) czy udostępnianie bieżącego salda (ang. _inquire_).

Tak jak wspomniano wcześniej wieloużywalne klasy to takie na podstawie których możemy zrealizować wiele obiektów (egzlemplarzy czy **instancji**). 
Drugą ciekawą własnością obiektowości jest możliwosć tworzenia nowych klas na bazie już istniejących poprzez tzw. mechanizm dziedziczenia (ang. _inheritance_) - Nie odkrywaj Ameryki na nowo.

> Zadanie domowe - Przestań zastanawiać się nad życiowym pytaniem "Python czy R"

```{python}
import this
```

> Zadanie domowe - Sprawdź co możesz zrobić 
wykorzystując podstawowe biblioteki: `collections`,
`decimal`, `json`,`math`,`os`,`random`,`re`,`sqlite3`,`sys`.

### Twoja pierwsza statystyczna Klasa RandomWalk

```{python}
from random import choice

class RandomWalk(object):
    """generowanie błądzenia losowego"""
    def __init__(self, num_points=5000):
        """zdefiniuj punkt początkowy oraz domyślną liczbę iteracji
        - pamiętaj o self - tak by można odnieść się do obiektu
        """
        self.num_points = num_points
        self.x_values = [0]
        self.y_values = [0]

    def fill_walk(self):
        """Prosta pętla - spróbuj zamienić na for 
           Gdybym miał (gitarę...) komputer kwantowy mógłbym 
           generować liczby losowe - a dlaczego nie na zwykłym komputerze?
           wybierz kierunek - prawo lub lewo
           wybierz wartość kroku 
           i zobacz co wyszło !

        """
        while len(self.x_values) < self.num_points:
            x_direction = choice([-1,1])
            x_distance = choice([0,1,2,3,4])
            x_step = x_direction*x_distance

            y_direction = choice([-1,1])
            y_distance = choice([0,1,2,3,4])
            y_step = y_direction*y_distance

            if x_step == 0 and y_step == 0:
                continue

            next_x = self.x_values[-1] + x_step
            next_y = self.y_values[-1] + y_step

            self.x_values.append(next_x)
            self.y_values.append(next_y)

# sprawdź jak to działa 
rw = RandomWalk(50000)
rw.fill_walk()

point_number = list(range(rw.num_points))
plt.scatter(rw.x_values,rw.y_values, c=point_number, cmap=plt.cm.Blues, edgecolor='none', s=15)
plt.scatter(0,0,c='green', edgecolor='none',s=100)
plt.scatter(rw.x_values[-1],rw.y_values[-1], c='red',edgecolor='none', s=100)
plt.axes().get_xaxis().set_visible(False)
plt.axes().get_yaxis().set_visible(False)
plt.show()

```

## klasyfikacja binarna

```{python}
import numpy as np
import matplotlib.pyplot as plt

# dwa wiersze danych realizujące pierwszą klasę
x1 = [1,2]
y1 = [-1,-1]
# dwa wiersze danych realizujące drugą klasę 
x2 = [3,4]
y2 = [1,1]
plt.scatter(x1, y1, marker='o', color='r')
plt.scatter(x2, y2, marker='x', color='b')
plt.show()

```

```{python}

# w postaci macierzowej
X=np.array([[1],[2],[3],[4]])
X.shape
# do rozwiązania dodajemy 1 kolumnę (wartość bias)
np.zeros(1+X.shape[1])
y=[-1,-1,1,1]
```

### Niech się uczy !!!

```{python}
dziecko = Perceptron()
dziecko.fit()

# dziecko musi mieć parametr uczenia
dziecko.eta

# możemy sprawdzić jak szybko się uczy == ile błędów robi

dziecko.errors_ 

# rozwiązania znajdą się w wagach
dziecko.w_
# w naszym przypadku dziecko uczy się dwóch wag !

```


```{python}
class Perceptron():
    
    def __init__(self,eta=0.01, n_iter=10):
        """Czasem jednak ważne jak się zaczyna! """
        self.eta = eta
        self.n_iter = n_iter
    
    def fit(self, X, y):
        self.w_ = np.zeros(1+X.shape[1])
        self.errors_ = []
        """Przelicz poniższy kod w excelu krok po kroku"""
        for _ in range(self.n_iter):
            errors = 0
            for xi, target in zip(X,y):
                print(xi, target)
                update = self.eta*(target-self.predict(xi))
                print(update)
                self.w_[1:] += update*xi
                self.w_[0] += update
                print(self.w_)
                errors += int(update != 0.0)
            self.errors_.append(errors)
        return self
    
    def net_input(self, X):
        return np.dot(X, self.w_[1:])+self.w_[0]
    
    def predict(self, X):
        return np.where(self.net_input(X)>=0.0,1,-1)
```

```{python}
d = Perceptron()
d.fit() # i już umiem !!!
print(d.errors_)
print(d.w_)
```

> Zadanie - zmień wagi tak by zamiast od zera uzyskiwały małą wartość losową. 

### Tato, co to za kwiatek ? 

```{python}
df = pd.read_csv('https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data', header=None)
df.head()

y = df.iloc[0:100, 4].values
y = np.where(y == 'Iris-setosa', -1, 1)
X = df.iloc[0:100, [0,2]].values

plt.scatter(X[:50,0], X[:50,1], color='red', marker='o', label='Setosa')
plt.scatter(X[50:100,0], X[50:100,1], color='blue', marker='x', label='Versicolor')
plt.xlabel("dlugosc dzialki [cm]")
plt.ylabel("dlugosc platka [cm]")
plt.legend(loc='upper left')
plt.show()
```
### Tato to proste !

```{python}
ppn = Perceptron()

ppn.fit(X,y)

ppn.w_

plt.plot(range(1,len(ppn.errors_)+1), ppn.errors_, marker='o')
```
```{python}
# dodatkowa funkcja

from matplotlib.colors import ListedColormap

def plot_decision_regions(X,y,classifier, resolution=0.02):
    markers = ('s','x','o','^','v')
    colors = ('red','blue','lightgreen','gray','cyan')
    cmap = ListedColormap(colors[:len(np.unique(y))])

    x1_min, x1_max = X[:,0].min() - 1, X[:,0].max()+1
    x2_min, x2_max = X[:,1].min() -1, X[:,1].max()+1
    xx1, xx2 = np.meshgrid(np.arange(x1_min, x1_max, resolution),
                           np.arange(x2_min, x2_max, resolution))
    Z = classifier.predict(np.array([xx1.ravel(), xx2.ravel()]).T)
    Z = Z.reshape(xx1.shape)
    plt.contourf(xx1, xx2, Z, alpha=0.4, cmap=cmap)
    plt.xlim(xx1.min(), xx1.max())
    plt.ylim(xx2.min(),xx2.max())

    for idx, cl in enumerate(np.unique(y)):
        plt.scatter(x=X[y == cl,0], y=X[y==cl,1], alpha=0.8, c=cmap(idx), marker=markers[idx], label=cl)

# dla kwiatków

plot_decision_regions(X,y,classifier=ppn)
plt.xlabel("dlugosc dzialki [cm]")
plt.ylabel("dlugosc platka [cm]")
plt.legend(loc='upper left')
plt.show()


```

## Środowisko produkcyjne 























