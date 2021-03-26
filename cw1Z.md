---
layout: page
title: 04 - Ćwiczenia 1 - Batch processing Python
mathjax: true
---

#  Analiza danych w czasie rzeczywistym

## Ćwiczenia 1

- Przedstawienie środowiska Jupyter notebook (uruchomienie + Docker) 
- Podstawy numpy i pandas 
- Programowanie obiektowe w pythonie 
- Realizacja procesu klasyfikacji 
- Sieć neuronowa oparta na perceptronie 
- Wykorzystanie biblioteki Flask - środowisko produkcyjne

### Podstawowa terminologia

Dane będziemy przedstawiać w postaci tabelarycznej. 
W pythonie najłatwiej wykorzystać do tego celu bibliotekę Pandas. 

Będziemy starać się pisać kod w ujęciu obiektowym.

```{python}
import numpy as np
import pandas as pd
from sklearn.datasets import load_iris
```

```{python}
iris = load_iris()
df = pd.DataFrame(data= np.c_[iris['data'], iris['target']],
                     columns= iris['feature_names'] + ['target'])

df['species'] = pd.Categorical.from_codes(iris.target, iris.target_names)

df = df.drop(columns=['target'])


df.head()

df.info()
```


Powyżej przedstawiona została ramka danych stanowiąca fragment danych **Iris**.
Zbiór ten składa się z wyników pomiarów czterech cech trzech gatunków kwiatów Irysa.

Przez macierz _X_ będziemy oznaczali zbiór wszystkich przypadków i cech. 
Co w naszym przypdaku generuje macierz 150 wierszy oraz 4 kolumn. 

Przez wektor _y_ oznaczać będziemy etykiety. 

### Perceprton jako prosty przykład klasyfikacji binarnej

Zredukujmy nasze dane do dwóch gatunków i dwóch cech

```{python}
y = df.iloc[0:100,4]

y.unique()
```
Zamiana na liczby 

```{python}
y = np.where(y == 'setosa',-1,1)
X = df.iloc[:100,[0,2]].values # pierwsze sto wierszy + kolumna 0 i 2, values- zamienia na np array
```

```{python}
import matplotlib.pyplot as plt

plt.scatter(X[:50,0],X[:50,1],color='red', marker='o',label='setosa')
plt.scatter(X[50:100,0],X[50:100,1],color='blue', marker='x',label='versicolor')
plt.xlabel('sepal length (cm)')
plt.ylabel('petal length (cm)')
plt.legend(loc='upper left')
plt.show()

```
```{python}
# Naszych danych chcemy nauczyć prostą sieć neuronową (bez warstw ukrytych)
ppn = Perceptron(eta=0.1, n_iter=10)
ppn.fit(X,y)
```


### Sztuczne neurony - rys historyczny

W 1943 roku W. McCulloch i W. Pitts zaprezentowali pierwszą koncepcję uproszczonego modelu komórki nerwowej tzw. **Nuronu McCulloch-Pittsa** (MCP). [W.S. McCulloch, W. Pitts, A logical Calculus of the Ideas Immanent in Nervous Activity. "The Bulletin of Mathematical Biophysics" 1943 nr 5(4)](https://www.cs.cmu.edu/~./epxing/Class/10715/reading/McCulloch.and.Pitts.pdf)

Neuronami nazywamy wzajemnie połączone komórki nerwowe w mózgu, które są odpowiedzialne za przetwarzanie i przesyłanie sygnałów chemicznych i elektrycznych. Komórka taka opisana jest jako bramka logiczna zawierająca binarne wyjścia. Do dendrytów dociera duża liczba sygnałów, które są integrowane w ciele komórki i (jeżeli energia przekracza określoną wartość progową) zostaje wygenerowany sygnał wyjściowy przepuszczany przez akson.

Po kilku latach Frank Rosenblatt (na podstawie MCP) zaproponował pierwszą koncepcję reguły uczenia perceprtonu. [F. Rosenblatt, The Perceptron, a Perceiving and Recognizing Automaton, Cornell Aeronautical Laboratory, 1957](https://blogs.umass.edu/brain-wars/files/2016/03/rosenblatt-1957.pdf)


## Podstawy obiektowości

Aplikacje powinny być wytwarzane w sposób niezawodny, szybki oraz ekonomiczny.
`Obiekty` (a dokładniej `Klasy`) to jeden ze środków dzięki któremy można uzyskać ten cel. 
Obiekty można rozumieć jako _wieloużywalne_ komponenty oprogramowania (_ang. reusable_).
Potrafią realizować one rozmaite koncepcje i byty np. datę, czas, obrazy, samochody, dźwięk, ludzi etc. 
Praktycznie wszystko co określane jest jako rzeczownik, może być realizowane w kategoriach **atrybutów** obiektów. 
Natomiast zachowania obiektów, wyrażane czasownikami, można określić jako **metody** klas.
Programy oparte o obiekty są dużo łatwiejsze do zrozumienia i weryfikacji niż kody pisane w konwencji tzw. programowania strukturalnego.

> Zadanie domowe - Jakie inne koncepcje programowania używane są współczenie ? (zobacz np. język Scala)


Obiekt realizujący konto bankowe można wygenerować z klasy, która zapewne posiada metody reprezentujące wpłaty środków (ang. _deposit_), ich wypłatę (ang. _withdraw_) czy udostępnianie bieżącego salda (ang. _inquire_).

Tak jak wspomniano wcześniej wieloużywalne klasy to takie na podstawie których możemy zrealizować wiele obiektów (egzlemplarzy czy **instancji**). 
Drugą ciekawą własnością obiektowości jest możliwosć tworzenia nowych klas na bazie już istniejących poprzez tzw. mechanizm dziedziczenia (ang. _inheritance_) - Nie odkrywaj Ameryki na nowo.

> Zadanie domowe - Usiądź do komputera, wyłącz fb i inne rozpraszacze ! Zacznij myśleć i pisz kod zorientowany obiektowo (ang. _Object Oriented Analysis and Design_). Ale wpierw sprawdź kiedy i gdzie powstał język Python. Znajdź inne języki zorientowane obiektowo. Gdzie w analizach danych słyszałeś o takich językach ? 

> Zadanie domowe bis - Przestań zastanawiać się nad życiowym pytaniem "Python czy R"


```{python}
import this
```


> Zadanie domowe - Sprawdź co możesz zrobić 
wykorzystując podstawowe biblioteki: `collections`,
`decimal`, `json`,`math`,`os`,`random`,`re`,`sqlite3`,`sys`.

```{python}
from random import randint

class Die(object):
    """Pojedynczy rzut kością"""
    def __init__(self, num_sides=6):
        """Kość to zazwyczaj sześcian"""
        self.num_sides = num_sides

    def roll(self):
        """Zwraca losową wartość od 1 do liczby ścian"""
        return randint(1,self.num_sides)

# program 

die = Die() # stwórz kość

results = []
for roll_num in range(10): # powtórz 10 razy
    result = die.roll() # rzuć kością raz
    results.append(result) # zapisz do listy

print(results)
```

> Zadanie domowe - przeanalizuj kod klasy RandomWalk

```{python}
from random import choice

class RandomWalk(object):
    """generowanie błądzenia losowego"""
    def __init__(self, num_points=5000):
        self.num_points = num_points
        self.x_values = [0]
        self.y_values = [0]

    def fill_walk(self):
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
```{python}
# a tak powinno działać !!!
from random import choice

class RandomWalk(object):
    """generowanie błądzenia losowego"""
    def __init__(self, num_points=5000):
        self.num_points = num_points
        self.x_values = [0]
        self.y_values = [0]

    def fill_walk(self):
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

```

```{python}
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

## KLASYFIKACJA

```{python}

import numpy as np
import matplotlib.pyplot as plt

# dwa wiersze danych realizujące pierwszą klasę
x1 = [1,2]
x2 = [3,4]
# dwa wiersze danych realizujące drugą klasę 
y1 = [-1,-1]
y2 = [1,1]
plt.scatter(x1, y1, marker='o', color='r')
plt.scatter(x2, y2, marker='x', color='b')
plt.show()
```

```{python}
# wersja dla Numpy i Panadas
# w postaci macierzowej
X=np.array([[1],[2],[3],[4]])
X.shape
# do rozwiązania dodajemy 1 kolumnę (wartość bias)
np.zeros(1+X.shape[1])
y=[-1,-1,1,1]

print(X)

print(y)
```

### Niech się uczy !!!

```{python}
dziecko = Perceptron()
dziecko.fit()
```
#### dziecko musi mieć parametr uczenia

`dziecko.eta`

#### możemy sprawdzić jak szybko się uczy == ile błędów robi

`dziecko.errors_ `

#### rozwiązania znajdą się w wagach
dziecko.w_
#### w naszym przypadku dziecko uczy się dwóch wag !

```{python}
# implementacja 
class Perceptron():
    
    def __init__(self,eta=0.01, n_iter=10):
        self.eta = eta
        self.n_iter = n_iter
    
    def fit(self, X, y):
        self.w_ = np.zeros(1+X.shape[1])
        self.errors_ = []
        
        for _ in range(self.n_iter):
            errors = 0
            for xi, target in zip(X,y):
                #print(xi, target)
                update = self.eta*(target-self.predict(xi))
                #print(update)
                self.w_[1:] += update*xi
                self.w_[0] += update
                #print(self.w_)
                errors += int(update != 0.0)
            self.errors_.append(errors)
        return self
    
    def net_input(self, X):
        return np.dot(X, self.w_[1:])+self.w_[0]
    
    def predict(self, X):
        return np.where(self.net_input(X)>=0.0,1,-1)

# uzycie jak wszsytkie klasy sklearn
d = Perceptron()
d.fit(X,y)
print(d.errors_)
print(d.w_)
```

```{python}
d.predict(np.array([1]))

d.predict(np.array([1,2,3,4]).reshape(-1,1))
```
> Zadanie - zmień wagi tak by zamiast od zera uzyskiwały małą wartość losową.

```{python}
# Jaki to kwiatek ?

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

```{python}
# mozna usunać printy :) 

ppn = Perceptron()

ppn.fit(X,y)

ppn.w_

plt.plot(range(1,len(ppn.errors_)+1), ppn.errors_, marker='o')
```

```{python}
X # teraz musisz podawać dwie wartości 
ppn.predict([5.1,3.1])
ppn.predict([[5.1,3.1],[6.2,4.1]])
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

## Server Flask

```{python}
# pomocny model dla flaska
from sklearn.linear_model import Perceptron
a = Perceptron()
a.fit(df.iloc[:,0:4],df.iloc[:,4])
df.iloc[0,0:4].values

a.predict(np.array([5.1, 3.5, 1.4, 0.2]).reshape(1,-1))
a.predict(np.array([[5.1, 3.5, 1.4, 0.2],[3.31,3,1,2.5]]))

### Zapisz swój najlepszy model 
import pickle
with open('model.pkl', "wb") as picklefile:
    pickle.dump(ppn, picklefile)
    # pickle.dump(a,picklefile)

with open('model.pkl',"rb") as picklefile:
    model = pickle.load(picklefile)

model.predict([5,2])
```

### Aplikacja z modelem 

```{python}
%%file app.py

import pickle
from math import log10

from flask import Flask
from flask import request
from flask import jsonify
import numpy as np

class Perceptron():
    
    def __init__(self,eta=0.01, n_iter=10):
        self.eta = eta
        self.n_iter = n_iter
    
    def fit(self, X, y):
        self.w_ = np.zeros(1+X.shape[1])
        self.errors_ = []
        
        for _ in range(self.n_iter):
            errors = 0
            for xi, target in zip(X,y):
                #print(xi, target)
                update = self.eta*(target-self.predict(xi))
                #print(update)
                self.w_[1:] += update*xi
                self.w_[0] += update
                #print(self.w_)
                errors += int(update != 0.0)
            self.errors_.append(errors)
        return self
    
    def net_input(self, X):
        return np.dot(X, self.w_[1:])+self.w_[0]
    
    def predict(self, X):
        return np.where(self.net_input(X)>=0.0,1,-1)

# Create a flask
app = Flask(__name__)

# Create an API end point
@app.route('/api/v1.0/predict', methods=['GET'])
def get_prediction():

    # sepal length
    sepal_length = float(request.args.get('sl'))
    # sepal width
    #sepal_width = float(request.args.get('sw'))
    # petal length
    petal_length = float(request.args.get('pl'))
    # petal width
    #petal_width = float(request.args.get('pw'))

    # The features of the observation to predict
    #features = [sepal_length,
    #            sepal_width,
    #            petal_length,
    #           petal_width]
    
    features = [sepal_length,
                petal_length]
    
    print(features)
    # Load pickled model file
    with open('model.pkl',"rb") as picklefile:
        model = pickle.load(picklefile)
    print(model)
    # Predict the class using the model
    predicted_class = int(model.predict(features))

    # Return a json object containing the features and prediction
    return jsonify(features=features, predicted_class=predicted_class)

if __name__ == '__main__':
    app.run()
```

```{bash}
!python app.py
```


```{python}
# http://127.0.0.1:5000/api/v1.0/predict?&sl=4.5&pl=1.3

# pamiętaj otworzyć nowy notebook !
import requests
response = requests.get("http://127.0.0.1:5000/api/v1.0/predict?&sl=4.5&pl=1.3")
print(response.content)
```