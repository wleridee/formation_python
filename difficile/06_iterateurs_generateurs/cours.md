# Module : Itérateurs et Générateurs

## 1. Quoi : Le Protocole d'Itération

En Python, l'itération (le fait de parcourir une séquence avec une boucle `for`) est un protocole bien défini.

- Un objet **itérable** est un objet capable de retourner ses membres un par un. C'est tout objet sur lequel on peut faire une boucle `for` (listes, tuples, chaînes, dictionnaires, etc.). Techniquement, c'est un objet qui a une méthode `__iter__()`.
- Un **itérateur** est un objet qui représente un flux de données. Il se souvient de sa position dans le flux. Il a une méthode `__next__()` qui retourne l'élément suivant et lève une exception `StopIteration` quand il n'y a plus d'éléments.

Quand vous faites `for element in my_list:`, Python appelle d'abord `iter(my_list)` pour obtenir un itérateur, puis appelle `next()` sur cet itérateur à chaque tour de boucle jusqu'à recevoir `StopIteration`.

## 2. Les Générateurs : Des Itérateurs Simplifiés

Créer une classe d'itérateur manuellement (avec `__iter__` et `__next__`) est possible mais verbeux. Les **générateurs** offrent une manière beaucoup plus simple et élégante de créer des itérateurs.

Il y a deux types de générateurs :

1.  Les **fonctions génératrices** (utilisant le mot-clé `yield`).
2.  Les **expressions génératrices** (syntaxe similaire aux compréhensions de liste).

## 3. Les Fonctions Génératrices et `yield`

Une fonction génératrice est une fonction qui utilise le mot-clé `yield` au lieu de `return`.

- Quand une fonction génératrice est appelée, elle ne s'exécute pas. Elle retourne un **objet générateur**, qui est un type d'itérateur.
- Chaque fois que `next()` est appelé sur le générateur, la fonction s'exécute jusqu'à ce qu'elle rencontre une instruction `yield`.
- `yield` **met en pause** l'exécution de la fonction, retourne la valeur spécifiée, et **sauvegarde son état local** (ses variables).
- À l'appel suivant de `next()`, la fonction reprend son exécution juste après le `yield`, avec son état intact.

**Exemple : Un simple compteur**

```python
def simple_generator():
    print("Generator starts")
    yield 1
    print("Generator resumes")
    yield 2
    print("Generator resumes again")
    yield 3
    print("Generator ends")

# 1. On obtient un objet générateur
gen = simple_generator()
print(type(gen)) # <class 'generator'>

# 2. Premier appel à next()
print(f"First value: {next(gen)}")
# Affiche:
# Generator starts
# First value: 1

# 3. Deuxième appel à next()
print(f"Second value: {next(gen)}")
# Affiche:
# Generator resumes
# Second value: 2

# 4. Troisième appel à next()
print(f"Third value: {next(gen)}")
# Affiche:
# Generator resumes again
# Third value: 3

# 5. Quatrième appel à next() -> StopIteration
# next(gen) # Lèverait une StopIteration
```

Une boucle `for` gère tout cela automatiquement.

```python
for value in simple_generator():
    print(f"Value from for loop: {value}")
```

## 4. Pourquoi utiliser des Générateurs ? La "Lazy Evaluation"

Le principal avantage des générateurs est la **"lazy evaluation"** (évaluation paresseuse). Ils ne génèrent les valeurs qu'au moment où on les demande.

- **Efficacité en mémoire** : C'est extrêmement efficace pour travailler avec de très grandes séquences de données. Une fonction qui retourne une liste d'un milliard d'éléments consommerait une quantité énorme de RAM. Un générateur qui produit ces mêmes éléments un par un n'en stocke qu'un seul en mémoire à la fois.
- **Traitement de flux infinis** : Les générateurs peuvent représenter des séquences infinies (ex: un générateur qui produit tous les nombres premiers, ou des données provenant d'un capteur en temps réel).

**Exemple : Fichiers volumineux**

**Mauvais (charge tout en mémoire)**

```python
def read_file_bad(filename):
    with open(filename, 'r') as f:
        return f.readlines() # Retourne une liste de toutes les lignes
```

**Bon (utilise un générateur)**

```python
def read_file_good(filename):
    with open(filename, 'r') as f:
        for line in f:
            yield line # Retourne les lignes une par une
```

Avec la deuxième version, vous pouvez traiter un fichier de plusieurs gigaoctets sans jamais saturer votre mémoire.

## 5. Les Expressions Génératrices

Une expression génératrice a une syntaxe très similaire à une compréhension de liste, mais avec des parenthèses `()` au lieu de crochets `[]`.

- **Compréhension de liste** : `[x*x for x in range(10)]`
  - Crée une **liste complète** en mémoire.
- **Expression génératrice** : `(x*x for x in range(10))`
  - Crée un **objet générateur** qui produira les valeurs à la demande.

```python
# Crée une liste de 100 millions de nombres en mémoire
list_comp = [i for i in range(100_000_000)]

# Crée un objet générateur, ne consomme presque pas de mémoire
gen_expr = (i for i in range(100_000_000))

# On peut ensuite utiliser le générateur
total = sum(gen_expr) # sum() itère sur le générateur sans tout stocker
```

✅ **Bonne pratique** : Si vous n'avez besoin d'itérer sur le résultat qu'une seule fois (par exemple, pour le passer à une autre fonction comme `sum()` ou dans une boucle `for`), utilisez une expression génératrice. C'est plus efficace en mémoire.
