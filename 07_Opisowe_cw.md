


## Iris data

Korzystając z biblioteki sklearn wczytaj dane Iris.

```python
from sklearn import datasets
iris = datasets.load_iris()
df = pd.DataFrame(iris.data, columns = iris.feature_names)
# przyda się kopia do późniejszej analizy
df_features = df.copy()
df['target'] = iris.target
```

Metody **EDA** (ang. _Exploratory Data Analysis_) dostępne dla ramki danych:

1. dtypes, astype()

```python
df.dtypes
# zamiana typów kolumn
df['name'] = df.['name'].astype('int')
```

2. describe()- generuje podstawowe statystyki ale działa tylko dla kolumn liczbowych

```python
df.describe()
```

3. quantile() - aby otrzymać bardziej szczegółowe dane na temat kwantyli.

```python
df.quantile([0.1, 0.5, 0.9])
df['sepal length (cm)'].quantile([0.2])
```

4. unique()  - dla zmiennych kategoryjnych

```python
df['target'].unique()
```

5. Metody agregujące (porównaj do SQL) - mean(), sum(), avg(), var(), count(), groupby().

```python
grouped_target = df.groupby(['target'])
grouped_target_mean = grouped_target.mean()
grouped_target_var = grouped_target.var()
grouped_target_count = grouped_target.count()
```

6. Wybieranie kolumn i wierszy

```{python}
# jedna kolumna jako obiekt Series
df.['target']
# więcej kolumn jako DataFrame
df[['sepal_length','petal_length']]
# lub
lista = ['sepal_length','petal_length']
df = df[lista]
# .loc()
df.loc(50,'target')
df.iloc(50,4)
```

5. boxplot() - pozwala na szybką graficzną prezentację własności zmiennych. wykresy generowane są w postaci tzw. wykresów pudełkowych (patrz wykład).

```{python}
df.boxplot()
# lub dla jednej zmiennej
df.boxplot(column='sepal length (cm)')
grouped_target.boxplot()
grouped_target.boxplot(column='sepal length (cm)')
```

6. Histogramy zmiennych ilościowych

```{python}
df['sepal_length'].plot(kind='hist', alpha=0.5, bins=20)
```

7. Zależności między zmiennymi

```{python}
df.plot(kind="scatter",x='sepal length (cm)',y='petal length (cm)', c='blue')

# dla wszystkich zmiennych
colors = []
palette = {0:"red", 1:"green", 2:"blue"}
for c in np.nditer(iris.target):
    colors.append(palette[int(c)])
df = pd.DataFrame(iris.data, columns=iris.feature_names)
sc = pd.plotting.scatter_matrix(df, alpha=0.3, figsize=(10,10), diagonal="hist", color=colors, marker="o", grid="True")
```

## Dane do analizy

Przeanalizuj następujące [dane](data/Brain_Size.csv)

```python
"";"Sex";"FSIQ";"VIQ";"PIQ";"Weight";"Height";"MRI_Count"
"1";"Female";133;132;124;"118";"64.5";816932
"2";"Male";140;150;124;".";"72.5";1001121
"3";"Male";139;123;150;"143";"73.3";1038437
"4";"Male";133;129;128;"172";"68.8";965353
"5";"Female";137;132;134;"147";"65.0";951545
```

Wczytaj dane do ramki danych. Dlaczego nie użyjesz tablicy NumPy ?

```python
# czy to wystarczy ?
data = pd.read_csv('brain_size.csv')
```

### Podsatwowe informacje o danych

Wpierw sprawdź ilość wierszy i kolumn.

```python
data.shape
```

Wypiszmy nazwy kolumn.

```python
list(data)
```

Zobaczmy podstawowe statystyki dla danych liczbowych.

```python
data.describe()
```

Mamy jedną zmienną kategoryjną - płeć. Pogrupujmy dane i sprawdzmy średnią wartość zmiennej _VIQ_ w każdej grupie.

```python
print(data['Sex'])

# prosty selector
data[data['Sex'] == 'Female']['VIQ'].mean()

# grupowanie
groupby_sex = data.groupby('Sex')

gropuby_sex.mean()

groupby_sex.boxplot()

for element, value in groupby_sex['VIQ']:
    print(element,value.mean())
```

### Sprawdź czy potrafisz

1. What is the mean value for VIQ for the full population?
2. How many males/females were included in this study?

Szybki przegląd zmiennych

```python
from pandas.tools import plotting
plotting.scatter_matrix(data[['Weight', 'Height', 'MRI_Count']])
plotting.scatter_matrix(data[['PIQ', 'VIQ', 'FSIQ']])
```

## Proste testy statystyczne

```python
from scipy import stats
```

1. Czy średnia w populacji dla danej zmiennej jest równa wybranej wartości ?
    - test 1-sample t-tsest *ttest_1samp()* zwracający p-value.
    - Czy średnie IQ jest równe 0

```python
stats.ttest_1samp(data['VIQ'],0)
```

2. Czy wartości średnie w grupach są takie same ?
    - test 2-sample t-test *test_ind()*
    - Czy IQ jest takie samo u kobiet i u mężczyzn ?

```python
female_viq = data[data['Gender'] == 'Female']['VIQ']
male_viq = data[data['Gender'] == 'Male']['VIQ']
stats.ttest_ind(female_viq, male_viq)
```


# Pivot in Pandas

[Dane do analizy](data/salesfunnel.xlsx)

```python
# biblioteki
import numpy as np
import pandas as pd
# załaduj dane

df = pd.read_excel("salesfunnel.xlsx")
df.head()
df["Status"] = df["Status"].astype("category")
df["Status"].cat.set_categories(["won","pending","presented","declined"],inplace=True)
```

## Tabele przestawne

Do każdej tabeli pivot mysisz określić ramke danych oraz wskazać index.

```python
pd.pivot_table(df,index=["Name"])
```

Index może zawierać więcej niż jedną kolumnę.

```python
pd.pivot_table(df,index=["Name","Rep","Manager"])
```

## Agregacje

Do wyświetlenia interesujących nas kolumn użyjemy własności _values_

Pierwszym agregatem będzie suma (_np.sum_). Ustawiamy ją dla parametry _aggfunc_.

Możemy korzystać również z listy i podać więcej agregacji.

```python
pd.pivot_table(df,index=["Manager", "Rep"])

pd.pivot_table(df,index=["Manager","Rep"],values=["Price"])
# sumowanie
pd.pivot_table(df,index=["Manager","Rep"],values=["Price"],aggfunc=np.sum)
# wiecej agregacji
pd.pivot_table(df,index=["Manager","Rep"],values=["Price"],aggfunc=[np.mean,len])
```

Opcja columns jest opcjonalna. Warto pamiętać, że agregacje tyczą się opcji values.

```python
pd.pivot_table(df,index=["Manager","Rep"],values=["Price"],columns=["Product"],aggfunc=[np.sum])
```

Wartości NAN

```python
pd.pivot_table(df,index=["Manager","Rep"],values=["Price"],columns=["Product"],aggfunc=[np.sum],fill_value=0)
# dodajmy jeszcze quantity
pd.pivot_table(df,index=["Manager","Rep"],values=["Price","Quantity"], columns=["Product"],aggfunc=[np.sum],fill_value=0)
```

```python
pd.pivot_table(df,index=["Manager","Status"],columns=["Product"],values=["Quantity","Price"],aggfunc={"Quantity":len,"Price":np.sum},fill_value=0)

table = pd.pivot_table(df,index=["Manager","Status"],columns=["Product"],values=["Quantity","Price"],aggfunc={"Quantity":len,"Price":[np.sum,np.mean]},fill_value=0)
table
```

## Zaawansowane Filtrowanie

Z otrzymanej poprzednio tabeli znajdźmy info o _Debra Henley_.

```python
table.query('Manager == ["Debra Henley"]')
```

lub

```python
table.query('Status == ["pending","won"]')
```

