# Module : Fonctions Lambda

## 1. Quoi : Les Fonctions Lambda

Une **fonction lambda** (ou fonction anonyme) est une petite fonction définie sans nom, en une seule ligne. Elle peut prendre plusieurs arguments, mais ne peut avoir qu'**une seule expression**.

La valeur de cette expression est ce que la fonction retourne implicitement.

## 2. Pourquoi : Des fonctions "jetables"

Les fonctions lambda sont utiles lorsque vous avez besoin d'une petite fonction pour une courte durée, typiquement comme argument pour une autre fonction (une fonction d'ordre supérieur).

- **Concises** : Elles permettent d'écrire du code plus court pour des opérations simples.
- **Immédiates** : Vous les définissez là où vous en avez besoin, sans avoir à "polluer" votre code avec la définition d'une fonction `def` que vous n'utiliserez qu'une seule fois.

Elles sont très couramment utilisées avec des fonctions comme `sorted()`, `map()`, et `filter()`.

## 3. Comment : La Syntaxe

La syntaxe de base est : `lambda arguments: expression`

- `lambda` : Le mot-clé qui déclare une fonction anonyme.
- `arguments` : Les paramètres de la fonction, séparés par des virgules (comme pour une fonction `def`).
- `expression` : Une unique expression dont la valeur sera retournée.

**Exemple : une fonction qui ajoute 10**

**Version `def`**

```python
def add_ten(x):
    return x + 10
```

**Version `lambda`**

```python
add_ten_lambda = lambda x: x + 10

# Les deux s'appellent de la même manière
print(add_ten(5))         # 15
print(add_ten_lambda(5))  # 15
```

Assigner une lambda à une variable (comme ci-dessus) est possible, mais c'est souvent un anti-pattern. Si une fonction mérite un nom, il vaut mieux utiliser `def`.

## 4. Cas d'usage principaux

### A. Avec `sorted()`

La fonction `sorted()` peut prendre un argument nommé `key` qui est une fonction. Cette fonction est appliquée à chaque élément avant de faire la comparaison pour le tri. Les lambdas sont parfaites pour cela.

**Exemple : Trier une liste de tuples par leur deuxième élément**

```python
points = [(1, 5), (9, 2), (4, 7)]

# On veut trier par le deuxième élément de chaque tuple (5, 2, 7)
# La fonction key doit donc prendre un tuple 'p' et retourner p[1]
sorted_points = sorted(points, key=lambda p: p[1])

print(sorted_points) # [(9, 2), (1, 5), (4, 7)]
```

**Exemple : Trier une liste de dictionnaires par âge**

```python
users = [
    {"name": "Alice", "age": 30},
    {"name": "Bob", "age": 25},
    {"name": "Charlie", "age": 35},
]

# La clé de tri est l'âge de l'utilisateur
sorted_users = sorted(users, key=lambda u: u["age"])

print(sorted_users)
# [{'name': 'Bob', 'age': 25}, {'name': 'Alice', 'age': 30}, {'name': 'Charlie', 'age': 35}]
```

### B. Avec `map()`

La fonction `map(function, iterable)` applique une fonction à chaque élément d'un itérable et retourne un objet `map` (que l'on peut convertir en liste).

**Exemple : Mettre au carré chaque nombre d'une liste**

```python
numbers = [1, 2, 3, 4]

# La fonction lambda x: x*x est appliquée à chaque nombre
squared_numbers_map = map(lambda x: x * x, numbers)

# On convertit l'objet map en liste pour voir le résultat
squared_numbers_list = list(squared_numbers_map)

print(squared_numbers_list) # [1, 4, 9, 16]
```

> **Note** : Dans ce cas précis, une compréhension de liste `[x*x for x in numbers]` est souvent considérée comme plus "pythonic" et plus lisible.

### C. Avec `filter()`

La fonction `filter(function, iterable)` retourne un objet `filter` contenant uniquement les éléments de l'itérable pour lesquels la fonction retourne `True`.

**Exemple : Garder uniquement les nombres pairs**

```python
numbers = [1, 2, 3, 4, 5, 6]

# La fonction lambda n: n % 2 == 0 retourne True si n est pair
even_numbers_filter = filter(lambda n: n % 2 == 0, numbers)

# On convertit en liste
even_numbers_list = list(even_numbers_filter)

print(even_numbers_list) # [2, 4, 6]
```

> **Note** : Ici aussi, une compréhension de liste `[n for n in numbers if n % 2 == 0]` est une alternative très populaire.

## 5. Quand ne PAS les utiliser ?

- **Pour des fonctions complexes** : Une lambda est limitée à une seule expression. Si votre logique nécessite plusieurs lignes, des `if/elif/else` complexes ou des boucles, utilisez une fonction `def` normale.
- **Quand la lisibilité en pâtit** : Une lambda très longue ou obscure est pire qu'une fonction `def` bien nommée. Le but est d'améliorer la clarté, pas de l'obscurcir.
- **Si vous l'assignez à un nom** : Comme dit plus haut, `my_func = lambda x: ...` est fonctionnel, mais `def my_func(x): ...` est plus clair et fournit de meilleures informations pour le débogage.
