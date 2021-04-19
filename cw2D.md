---
layout: page
title: 06 - Ćwiczenia 2 - OOP Python w przetwarzaniu 
mathjax: true
---

Na poprzednich zajęciach rozwazaliśmy proste dane dla problemu klasyfikacji.


```{python}
X=np.array([[1],[2],[3],[4]])
y=[-1,-1,1,1]
```

### Sztuczne neurony - rys historyczny

W 1943 roku W. McCulloch i W. Pitts zaprezentowali pierwszą koncepcję uproszczonego modelu komórki nerwowej tzw. **Nuronu McCulloch-Pittsa** (MCP). [W.S. McCulloch, W. Pitts, A logical Calculus of the Ideas Immanent in Nervous Activity. "The Bulletin of Mathematical Biophysics" 1943 nr 5(4)](https://www.cs.cmu.edu/~./epxing/Class/10715/reading/McCulloch.and.Pitts.pdf)

Neuronami nazywamy wzajemnie połączone komórki nerwowe w mózgu, które są odpowiedzialne za przetwarzanie i przesyłanie sygnałów chemicznych i elektrycznych. Komórka taka opisana jest jako bramka logiczna zawierająca binarne wyjścia. Do dendrytów dociera duża liczba sygnałów, które są integrowane w ciele komórki i (jeżeli energia przekracza określoną wartość progową) zostaje wygenerowany sygnał wyjściowy przepuszczany przez akson.

Po kilku latach Frank Rosenblatt (na podstawie MCP) zaproponował pierwszą koncepcję reguły uczenia perceprtonu. [F. Rosenblatt, The Perceptron, a Perceiving and Recognizing Automaton, Cornell Aeronautical Laboratory, 1957](https://blogs.umass.edu/brain-wars/files/2016/03/rosenblatt-1957.pdf)



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


## Jaki to kwiatek ?

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


ppn = Perceptron(eta=0.001)
ppn.fit(X,y)
ppn.w_

plt.plot(range(1,len(ppn.errors_)+1), ppn.errors_, marker='o')
```

#### Co tutaj się wydarzyło?
Stworzyliśmy klasę, dzięki, której możemy stworzyć prosty model perceptronu liniowego.  
Funkcja aktywacji w perceptronie liniowym jest bardzo prosta.  
Przypomina nam najprostszy model KMNK (Klasycznej Metody Najmniejszych Kwadratów).  
$y_i = w_0 + w_1x_{1i}+w_2x_{2i}$

1. Zainicjowaliśmy wagi równe 0 dla każdego parametru.  
```{python}
self.w_ = np.zeros(1+X.shape[1])
```  
Początkowo nasz model wygląda tak:  
$$y_i = 0 + 0\times x_{1i} + 0\times x_{2i} $$

2. Liczymy wartości z powyższego wzoru, czyli dla każdego elementu w dataframe X liczymy wartość y.  
`def net_input(self, X):`  
Korzystając z funkcji `np.dot` wymnażamy elementy z macierzy X przez odpowiadające im wagi i dodajemy wyraz wolny.  
Ponieważ mnożymy wszytsko przez 0 i dodajemy zero to wynikiem jest 0.

3. Przewidujemy, ustalamy punkt odcięcia na poziomie 0 i wyznaczamy, że dla wartości $y_i>=0$, wartością przewidywaną jest 1 (versicolor), a dla wartości $y_i<0$ wartością przewidywaną jest -1 (setosa). Dla każdej obserwacji otrzymujemy 0, czyli przewidujemy 1 (versicolor).

4. Liczymy kolejne wagi.  

```{python}
update = self.eta*(target-self.predict(xi))
self.w_[1:] += update*xi
self.w_[0] += update
```  
Dla każdej obserwacji obliczamy wartość update, czyli różnicę między wartością obserwowaną a przewidywaną.  
Na przykładzie dla pierwszej obserwacji:  
$$x_{11}=5.1$$  
$$x_{21}=1.4$$  
$$y_1 = -1$$  
$$\hat{y_1} = 1$$  
$$ update = 0.01*(-1 - 1) = 0.01*(-2) = - 0.02$$  
Tutaj kluczowa jest ustalana przez nas wartość `eta`, która mówi jak bardzo pomyłka na predykcji ma się przełożyć na uaktualnienie wartości wag. W naszym przykładzie wybrano 0.01.
Teraz wyraz wolny uaktualniamy (funkcja liniowa zatem po prostu dodajemy) o wartość update, a wagi dla $w_{1}$ oraz $w_{2}$ o wartość $update$ przemnożoną przez odpowiednio przez $x_{11}$ oraz $x_{22}$.  
$$w_0 = 0 + (-0.02) = -0.02$$ 
$$w_1 = 0 + (-0.02)\times 5.1 = -0.102$$ 
$$w_2 = 0 + (-0.02)\times 1.4 = -0.028$$  

5. Dla obserwacji $x_{12},\;x_{22}$ nowy zestaw wag początkowych to: 

$$w_0 = -0.02;\;\;w_1 = -0.102;\;\;w_2 = -0.028$$  
6. Tak jak to zrobiliśmy powyżej powtarzamy dla każdej obserwacji. Za każdym razem sumujemy wartość $update$ do zmiennej $error$ po wszytskich obserwacjach dodajemy ją do listy wartości błedów, to pozwoli nam określić jak duży błąd model popełnia przy każdej iteracji. Jedna iteracja to przebięgniecie algorytmu po wszystkich obserwacjach. Parametrem ```n_iter``` możemy sterować ile tych iteracji wykonamy.  
W rzeczywistości przy uczeniu sieci neuronowej najczęściej używa się dwóch kryteriów STOP (czyli kiedy przestać uczyć sieć) albo jest to właśnie ilość iteracji. Albo kryterium poprawności predykcji, gdy osiągniemy oczekiwany rezultat kończymy uczyć sieć.


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


> Zadanie 

```{python}
# ZADANIE - Opisz czym różni się poniższy algorytm od Perceprtona ? 
class Adaline():
    '''Klasyfikator  - ADAptacyjny LIniowy NEuron'''
    def __init__(self, eta=0.01, n_iter=10):
        self.eta = eta
        self.n_iter = n_iter

    def fit(self, X,y):
        self.w_ = np.zeros(1+X.shape[1])
        self.cost_ = []

        for i in range(self.n_iter):
            net_input = self.net_input(X)
            output = self.activation(X)
            errors = (y-output)
            self.w_[1:] += self.eta * X.T.dot(errors)
            self.w_[0] += self.eta * errors.sum()
            cost = (errors**2).sum() / 2.0
            self.cost_.append(cost)
        return self

    def net_input(self, X):
        return np.dot(X, self.w_[1:]) + self.w_[0]

    def activation(self, X):
        return self.net_input(X)

    def predict(self, X):
        return np.where(self.activation(X) >= 0.0, 1, -1) 
```

```{python}
ad = Adaline(n_iter=20, eta=0.01)
ad.fit(X,y)
ad.w_

plot_decision_regions(X,y,classifier=ad)
plt.xlabel("dlugosc dzialki [cm]")
plt.ylabel("dlugosc platka [cm]")
plt.legend(loc='upper left')
plt.show()
ad.cost_


ad2 = Adaline(n_iter=20, eta=0.0001)
ad2.fit(X,y)
ad2.w_
ad2.cost_

plot_decision_regions(X,y,classifier=ad2)
plt.xlabel("dlugosc dzialki [cm]")
plt.ylabel("dlugosc platka [cm]")
plt.legend(loc='upper left')
plt.show()
```

# Środowisko produkcyjne

```{python}

### Zapisz swój najlepszy model 
import pickle
with open('model.pkl', "wb") as picklefile:
    pickle.dump(ppn, picklefile)
    # pickle.dump(a,picklefile)

```

```{python}
with open('model.pkl',"rb") as picklefile:
    model = pickle.load(picklefile)

model.predict([5,2])
```

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