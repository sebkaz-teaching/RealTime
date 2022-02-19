---
layout: page
title: Ćwiczenia 3 - Obiektowy modele (sztucznych) sieci neuronowych
mathjax: true
---

Zdefiuniujmy prostą ramkę danych ze zmiennymi ilościowymi i jakościowymi dla zadania klasyfikacji binarnej.
Zmienne posiadają dodatkowo braki danych. Przykład ten może posłużyc do automatyzacji procesu przygotowania i analizy danych.  

```python
# obiektowosc sklearn
import numpy as np
import pandas as pd

dane = {'zm1':[3,4,np.NaN,2,6,1], 
        'zm2':[54,np.NaN, 56,342,643,123],
        'zm3':['niebieski','czerwony','czerwony',
               'niebieski','zielony','zielony'],
        'zm4':['male','male','female','male','female','female'],
        'target':[0,1,1,0,0,1]}

df = pd.DataFrame(dane)
df
```

```python
X = df.drop(columns=['target'])
y = df['target']
```

Zamiast od razu przetwarzac wpierw zastanówmy się co potrzebujemy zrobic i zapiszmy wszystko jako `pipeline`.
```python
from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import StandardScaler
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import OneHotEncoder

numeric_features = ["zm1", "zm2"]
numeric_transformer = Pipeline(steps=[
    ("imputer", SimpleImputer(strategy="mean")),
    ("scaler", StandardScaler())
])
categorical_features = ["zm3", "zm4"]
categorical_transformer = OneHotEncoder(handle_unknown="ignore")

```

zmienne numeryczne można przetworzyc niezależnie od jakościowych.
```python
preprocessor = ColumnTransformer(transformers=[
    ("num_trans", numeric_transformer, numeric_features),
    ("cat_trans", categorical_transformer, categorical_features)
])

from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier

pipeline = Pipeline(steps=[
    ("preproc", preprocessor),
    ("model", LogisticRegression())
])
```

Zweryfikuj jak wygłąda diagram 

```python
from sklearn import set_config
set_config(display='diagram')
pipeline
```

Metoda fit uruchamia potok metod `fit_transform()`
```python
from sklearn.model_selection import train_test_split
X_tr, X_test, y_tr, y_test = train_test_split(X,y,
test_size=0.2, random_state=42)

pipeline.fit(X_tr, y_tr)

score = pipeline.score(X_test, y_test)
print(score)

```

Wynik całej operacji i wszystkich opcji możesz zapisac jako obiekt: 
```python
import joblib
joblib.dump(pipeline, 'your_pipeline.pkl')
```

Zamiast cieszyc się jednym modelem od razu poszukajmy najlepszego rozwiązania: 
```python
param_grid = [
              {"preproc__num_trans__imputer__strategy":
              ["mean","median"],
               "model__n_estimators":[2,5,10,100,500],
               "model__min_samples_leaf": [1, 0.1],
               "model":[RandomForestClassifier()]},
              {"preproc__num_trans__imputer__strategy":
                ["mean","median"],
               "model__C":[0.1,1.0,10.0,100.0,1000],
                "model":[LogisticRegression()]}
]

from sklearn.model_selection import GridSearchCV


grid_search = GridSearchCV(pipeline, param_grid,
cv=2, verbose=1, n_jobs=-1)


grid_search.fit(X_tr, y_tr)

grid_search.best_params_

```

W przypadku gdy lista transformacji nie wyczerpuje Twoich oczekiwań zawsze możesz dodac swoje własne transformacje. 
Przydaje się do automatyzacji tworzenia nowych zmiennych. 

```python
from sklearn.base import BaseEstimator, TransformerMixin

class DelOneValueFeature(BaseEstimator, TransformerMixin):
    """Transformacja usuwająca zmienne, które posiadają
    tylko jedną wartość w całej kolumnie. Takie kolumny
    nie nadają się do modelowania. Metoda fit() wyszuka
    wszystkie takie kolumny. Natomiast metoda transform()
    usunie je ze zbioru danych.
"""
    def __init__(self):
        self.one_value_features = []
    def fit(self, X, y=None):
        for feature in X.columns:
            unikalne = X[feature].unique()
            if len(unikalne)==1:
                self.one_value_features.append(feature)
        return self
    def transform(self, X, y=None):
        if not self.one_value_features:
            return X
        return X.drop(axis='columns',
        columns=self.one_value_features)
```

nowy pipeline

```python
pipeline2 = Pipeline([
    ("moja_transformacja",DelOneValueFeature()),
    ("preprocesser", preprocessor),
    ("classifier", LogisticRegression())])
    
pipeline2.fit(X_tr, y_tr)
score2 = pipeline2.score(X_test, y_test)
```

## Własne modele sztucznych sieci neuronowych 

```python
# własny model 

# implementacja 
class Perceptron():
    
    def __init__(self, eta=0.01, n_iter=10):
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
        return np.dot(X, self.w_[1:]) + self.w_[0]
    
    def predict(self, X):
        return np.where(self.net_input(X)>=0.0,1,-1)
```

Przykładowe dane. 
```python
X=np.array([[1],[2],[3],[4]])
y=[-1,-1,1,1]

# AND
# X = np.array([0,0],[0,1],[1,0],[1,1])
# y = [-1,-1,-1,1]
# XOR

```

Zrealizuj kody również dla danych bramki `AND`, `OR` i `XOR`. 

```python
d = Perceptron()
d.fit(X,y)

print(d.errors_)
print(d.w_)

d.predict(np.array([-3]))
np.array([1,2,3,4]).reshape(-1,1)

d.predict(np.array([1,2,3,4,6,-1,23,-3]).reshape(-1,1))

```
> Wyjaśnij na czym polega różnica między perceprtonem a następną siecią neuronową ? 
> Dlaczego nazywa się on Adeline - kim była i co zrobiła ? 

```python
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

## Środowisko produkcyjne z modelem 

```python
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
Uruchom serwer
```bash
python app.py
```

Przejdź na stronę: 

http://127.0.0.1:5000/api/v1.0/predict?&sl=4.5&pl=1.3

```python
# możesz też wykorzystac kod pythona
import requests
response = requests.get("http://127.0.0.1:5000/api/v1.0/predict?&sl=4.5&pl=1.3")
print(response.content)
```

### Przydatne sposoby tworzenia API 

```python
from flask import Flask, request
from flask_restful import Resource, Api
from flask_jsonpify import jsonify


app  = Flask(__name__)

api = Api(app)

@app.route('/')
@app.route('/index')
def home():
    #return render_template('home.html')
    return "<h1>Strona poczatkowa</h1>"

@app.route('/hello/<name>')
def success(name):
    return f'<h2>{name}</h2>'

```

Lub
```python
class Main(Resource):
    def get(self):
        return jsonify("Hello world")

api.add_resource(Main,'/test')


class Irys(Resource):
    
    def get(self):
        conn=engine.connect()
        query = conn.execute('select * from dane')
        result = {'dane':[i for i in query.cursor.fetchall()]}
        return jsonify(result)


api.add_resource(Irys, '/irys')
```

