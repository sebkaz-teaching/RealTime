---
layout: page
title: Ćwiczenia 2 - Obiektowy Python w analizach danych
mathjax: true
---

## klasy i obiekty

```python
from random import choice

class RandomWalk():
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

rw = RandomWalk()
rw.x_values
rw2 = RandomWalk(num_points=10000)
rw2.num_points

rw.fill_walk()
import matplotlib.pyplot as  plt
point_number = list(range(rw.num_points))
plt.scatter(rw.x_values, rw.y_values, c=point_number, cmap=plt.cm.Blues,
           edgecolor='none', s=15)
plt.scatter(0,0,c='green', edgecolor='none', s=100)
plt.scatter(rw.x_values[-1], rw.y_values[-1],c='red', edgecolor='none', s=100)
plt.axes().get_xaxis().set_visible(False)
plt.axes().get_yaxis().set_visible(False)
plt.show()
```

## Dane tabelaryczne

```python
import numpy as np
import pandas as pd

from sklearn.datasets import load_iris

iris = load_iris()
type(iris)
iris.keys()
```

Ramki danych

```python
df = pd.DataFrame(data=np.c_[iris['data'], iris['target']],
                 columns=iris['feature_names']+['target'])

# proste operacje  

df.head(8)
df.tail()
df.info()
df.describe()
df['spacies'] = pd.Categorical.from_codes(iris.target, iris.target_names)
df.info()
df.head()

X = df.iloc[:100, [0,2]].values
df = df.drop(columns=['target'])
y = df.iloc[:100, 4].values
y = np.where(y=='setosa', -1,1)

plt.scatter(X[:50,0],X[:50,1], color='red', marker='o', label='setosa')
plt.scatter(X[50:100,0],X[50:100,1], color='blue', marker='x',
            label='versicolor')
plt.xlabel('sepal length')
plt.ylabel('petal length')
plt.legend(loc='upper left')
plt.show()
```

>> Otrzymany zbiór X i y będziesz wykorzystywał dośc często.

## bazy danych

```python
from sqlalchemy import create_engine
engine = create_engine('sqlite:///irysy.db')
df.to_sql('irisy', con=engine, index=False)
a = engine.execute("select * from irisy").fetchall()
df2 = pd.DataFrame(a, columns=df.columns)
```

## szybki zbiór danych 

```python
from sklearn import datasets
from sklearn.ensemble import RandomForestClassifier

X, y = datasets.make_classification(n_samples=10**4, 
                                    n_features=20, 
                                    n_informative=2)
train_sample = 7000

X_train=X[:train_sample]
X_test = X[train_sample:]

y_train = y[:train_sample]
y_test = y[train_sample:]

rfc = RandomForestClassifier()
rfc.fit(X_train, y_train)

import pickle

with open('model2.pkl', 'wb') as moj_model:
    pickle.dump(rfc, moj_model)

with open('model2.pkl', 'rb') as mm:
    model = pickle.load(mm)

```

## Dane nieustrukturyzowane

```python
from keras.datasets import mnist

(X_train, y_train),(X_test,y_test) = mnist.load_data()

X_train.shape
```

## model 

```python
dziecko = Perceptron()
dziecko.fit(X,y)

class Perceptron():
    def __init__(self,n_iter=10, eta=0.01):
        self.n_iter = n_iter
        self.eta = eta
        
    def fit(self,X,y):
        self.w_ = np.zeros(1+X.shape[1])
        self.errors_ = []
        for _ in range(self.n_iter):
            pass
        return self
```