---
layout: page
title: 01C -- Obiektowy Python
mathjax: true
---

## Podstawy obiektowości

Aplikacje powinny być wytwarzane w sposób niezawodny, szybki oraz ekonomiczny.
`Obiekty` (a dokładniej `Klasy`) to jeden ze środków dzięki któremy można uzyskać ten cel. 
Obiekty można rozumieć jako _wieloużywalne_ komponenty oprogramowania (_ang. reusable_).
Potrafią realizować one rozmaite koncepcje i byty np. datę, czas, obrazy, samochody, dźwięk, ludzi etc. 
Praktycznie wszystko co określane jest jako rzeczownik, może być realizowane w kategoriach **atrybutów** obiektów. 
Natomiast zachowania obiektów, wyrażane czasownikami, można określić jako **metody** klas.
Programy oparte o obiekty jest dużo łatwiejsze do zrozumienia i weryfikacji iż tzw. programowanie strukturalne. 
> Zadanie domowe - Jakie inne rodzaje programowania używane są współczenie ? (zobacz np. język Scala)


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
















