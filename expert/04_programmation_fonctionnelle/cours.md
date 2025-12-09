# Module : Programmation Fonctionnelle en Python

## Objectif

Ce module a pour but de vous familiariser avec les concepts et outils de la programmation fonctionnelle (PF) disponibles en Python. Vous apprendrez à utiliser des fonctions d'ordre supérieur, des fonctions pures, et des outils comme `map`, `filter`, `reduce`, et les modules `itertools` et `functools` pour écrire un code plus déclaratif, concis et souvent plus lisible.

## 1. Qu'est-ce que la Programmation Fonctionnelle ?

La programmation fonctionnelle est un paradigme de programmation qui traite le calcul comme l'évaluation de fonctions mathématiques et évite de changer l'état et les données muables.

Les concepts clés sont :

- **Fonctions pures** : Pour les mêmes entrées, elles produisent toujours les mêmes sorties et n'ont pas d'effets de bord observables (ne modifient pas de variables globales, n'écrivent pas dans un fichier, etc.).
- **Immuabilité** : Les données ne sont pas modifiées après leur création. Au lieu de modifier une structure de données, on en crée une nouvelle avec les changements.
- **Fonctions d'ordre supérieur (Higher-Order Functions)** : Ce sont des fonctions qui peuvent prendre d'autres fonctions comme arguments ou les retourner comme résultat.
- **Composition de fonctions** : Construire des fonctions complexes en combinant des fonctions plus simples.

Python n'est pas un langage purement fonctionnel, mais il intègre de nombreux concepts de la PF.

## 2. Fonctions d'Ordre Supérieur

Vous les utilisez déjà, peut-être sans le savoir ! Les décorateurs, par exemple, sont des fonctions d'ordre supérieur.

```python
def mon_decorateur(fonction_a_decorer):
    def wrapper():
        print("Avant l'appel de la fonction.")
        fonction_a_decorer()
        print("Après l'appel de la fonction.")
    return wrapper

@mon_decorateur
def dis_bonjour():
    print("Bonjour !")

dis_bonjour()
```

Ici, `mon_decorateur` prend une fonction en argument et en retourne une nouvelle.

## 3. Les Outils Classiques : `map`, `filter`, `reduce`

### a. `map(fonction, iterable)`

`map` applique une fonction à chaque élément d'un itérable et retourne un itérateur avec les résultats.

```python
nombres = [1, 2, 3, 4]

# Version impérative
carres = []
for n in nombres:
    carres.append(n * n)

# Version fonctionnelle avec map
carres_map = map(lambda n: n * n, nombres)
print(list(carres_map)) # [1, 4, 9, 16]

# Souvent remplacé par une compréhension de liste, plus idiomatique en Python
carres_comprehension = [n * n for n in nombres]
print(carres_comprehension) # [1, 4, 9, 16]
```

### b. `filter(fonction, iterable)`

`filter` construit un itérateur à partir des éléments d'un itérable pour lesquels une fonction retourne `True`.

```python
nombres = [1, 2, 3, 4, 5, 6]

# Version impérative
pairs = []
for n in nombres:
    if n % 2 == 0:
        pairs.append(n)

# Version fonctionnelle avec filter
pairs_filter = filter(lambda n: n % 2 == 0, nombres)
print(list(pairs_filter)) # [2, 4, 6]

# Version avec compréhension de liste
pairs_comprehension = [n for n in nombres if n % 2 == 0]
print(pairs_comprehension) # [2, 4, 6]
```

### c. `functools.reduce(fonction, iterable[, initialisateur])`

`reduce` applique de manière cumulative une fonction à deux arguments aux éléments d'un itérable, de gauche à droite, de manière à réduire la séquence à une seule valeur.

`reduce` a été déplacé dans le module `functools` en Python 3.

```python
from functools import reduce

nombres = [1, 2, 3, 4]

# Calculer la somme
somme = reduce(lambda acc, val: acc + val, nombres)
print(somme) # 10
# ((((1+2)+3)+4) = 10)

# Calculer le produit
produit = reduce(lambda acc, val: acc * val, nombres, 1) # avec initialisateur
print(produit) # 24
```

Bien que puissant, `reduce` peut souvent rendre le code moins lisible. Des boucles `for` explicites ou des fonctions comme `sum()` sont souvent préférées.

## 4. Le Module `functools`

Ce module contient des outils utiles pour la programmation fonctionnelle.

### a. `functools.partial`

`partial` permet de "geler" une partie des arguments d'une fonction, produisant un nouvel objet fonction avec une signature simplifiée. C'est une forme de **currying**.

```python
from functools import partial

def power(base, exponent):
    return base ** exponent

# Créer une nouvelle fonction qui calcule le carré
square = partial(power, exponent=2)

# Créer une nouvelle fonction qui calcule le cube
cube = partial(power, exponent=3)

print(square(5)) # 25 (équivalent à power(5, 2))
print(cube(5))   # 125 (équivalent à power(5, 3))
```

### b. `functools.wraps` (pour les décorateurs)

Lorsque vous écrivez un décorateur, la fonction décorée perd ses métadonnées (nom, docstring, etc.). `wraps` est un décorateur qui copie ces métadonnées sur la fonction wrapper.

```python
from functools import wraps

def mon_decorateur_propre(func):
    @wraps(func) # Copie les métadonnées de func sur wrapper
    def wrapper(*args, **kwargs):
        """Ceci est la docstring du wrapper."""
        return func(*args, **kwargs)
    return wrapper

@mon_decorateur_propre
def ma_fonction():
    """Ceci est la docstring originale."""
    pass

print(ma_fonction.__name__) # ma_fonction (sans @wraps, ce serait 'wrapper')
print(ma_fonction.__doc__)  # Ceci est la docstring originale.
```

## 5. Le Module `itertools`

Ce module est une mine d'or pour la programmation fonctionnelle. Il fournit des fonctions rapides et économes en mémoire pour créer des itérateurs pour des boucles efficaces.

Quelques exemples :

- `itertools.count(start, step)`: Crée un itérateur qui retourne des nombres espacés régulièrement, à l'infini.
- `itertools.cycle(iterable)`: Crée un itérateur qui répète les éléments d'un itérable, à l'infini.
- `itertools.chain(*iterables)`: Crée un itérateur qui retourne les éléments du premier itérable, puis du deuxième, etc., comme s'ils formaient une seule séquence.
- `itertools.islice(iterable, start, stop, step)`: Crée un itérateur qui retourne des éléments sélectionnés de l'itérable, comme le slicing de listes.
- `itertools.groupby(iterable, key=None)`: Crée un itérateur qui retourne des groupes d'éléments consécutifs.

```python
import itertools

# Combiner deux listes
nombres = [1, 2, 3]
lettres = ['a', 'b', 'c']
combine = itertools.chain(nombres, lettres)
print(list(combine)) # [1, 2, 3, 'a', 'b', 'c']

# Grouper des données triées
vehicules = [('Car', 'Ford'), ('Car', 'Audi'), ('Plane', 'Boeing'), ('Bike', 'Trek')]
# Les données DOIVENT être triées par la clé de groupement
for key, group in itertools.groupby(vehicules, key=lambda x: x[0]):
    print(key, list(group))
# Car [('Car', 'Ford'), ('Car', 'Audi')]
# Plane [('Plane', 'Boeing')]
# Bike [('Bike', 'Trek')]
```

## 6. Avantages et Inconvénients de la PF en Python

**Avantages :**

- **Code plus déclaratif** : On décrit _ce que_ l'on veut faire, pas _comment_ le faire.
- **Moins d'effets de bord** : Les fonctions pures sont plus faciles à tester, à raisonner et à paralléliser.
- **Code plus concis** : Des outils comme `map` ou les compréhensions de liste peuvent remplacer des boucles verbeuses.

**Inconvénients :**

- **Peut être moins lisible** : Un usage excessif de `lambda`, `reduce` ou de chaînes de `map`/`filter` peut rendre le code difficile à comprendre pour les non-initiés.
- **Performance** : La création de nombreuses petites fonctions (lambdas) peut avoir un léger surcoût par rapport à une boucle `for` directe.
- **Pas toujours idiomatique** : Python a sa propre "saveur" (le "Pythonic way"). Parfois, une compréhension de liste est plus claire et plus rapide qu'un `map` ou `filter`.

## Conclusion

La programmation fonctionnelle en Python n'est pas une approche "tout ou rien". C'est une boîte à outils de concepts et de fonctions que vous pouvez utiliser pour améliorer votre code. En adoptant des principes comme l'immuabilité et les fonctions pures là où c'est pertinent, et en utilisant judicieusement les outils des modules `functools` et `itertools`, vous pouvez écrire un code plus robuste, plus testable et souvent plus élégant.
