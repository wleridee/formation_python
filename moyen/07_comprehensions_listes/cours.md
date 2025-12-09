# Module : Compréhensions de Liste

## 1. Quoi : Les Compréhensions de Liste

Une **compréhension de liste** (_list comprehension_) est une syntaxe concise et élégante pour créer une nouvelle liste à partir d'un itérable existant. C'est l'une des fonctionnalités les plus appréciées et les plus "pythonic" du langage.

Elle combine une boucle `for` et la création d'un nouvel élément en une seule ligne.

## 2. Pourquoi : Un code plus lisible et plus rapide

Comparer la méthode classique avec une compréhension de liste :

**Méthode classique (avec une boucle `for`)**

```python
numbers = [1, 2, 3, 4, 5]
squares = []
for n in numbers:
    squares.append(n * n)
# squares est maintenant [1, 4, 9, 16, 25]
```

**Méthode avec une compréhension de liste**

```python
numbers = [1, 2, 3, 4, 5]
squares = [n * n for n in numbers]
# squares est maintenant [1, 4, 9, 16, 25]
```

Les avantages sont clairs :

- **Concise** : Moins de lignes de code.
- **Lisible** : La syntaxe est très proche de la description en langage naturel : "crée une liste de `n*n` pour chaque `n` dans `numbers`".
- **Performante** : Elle est souvent plus rapide qu'une boucle `for` équivalente, car elle est optimisée en C dans l'interpréteur Python.

## 3. Comment : La Syntaxe

La syntaxe de base est : `[<expression> for <element> in <iterable>]`

- `<expression>` : L'opération à appliquer à chaque élément pour créer le nouvel élément.
- `<element>` : La variable temporaire qui représente chaque élément de l'itérable.
- `<iterable>` : La séquence source (liste, tuple, range, etc.).

```python
# Mettre en majuscules une liste de noms
names = ["alice", "bob", "charlie"]
upper_names = [name.upper() for name in names]
# Résultat : ['ALICE', 'BOB', 'CHARLIE']

# Obtenir la longueur de chaque mot
words = ["hello", "world", "python"]
lengths = [len(word) for word in words]
# Résultat : [5, 5, 6]
```

### Ajout d'une condition `if`

On peut ajouter une condition `if` à la fin pour filtrer les éléments de l'itérable source.

**Syntaxe** : `[<expression> for <element> in <iterable> if <condition>]`

**Méthode classique**

```python
numbers = [1, 2, 3, 4, 5, 6]
even_squares = []
for n in numbers:
    if n % 2 == 0: # Condition de filtre
        even_squares.append(n * n)
# even_squares est [4, 16, 36]
```

**Avec une compréhension de liste**

```python
numbers = [1, 2, 3, 4, 5, 6]
even_squares = [n * n for n in numbers if n % 2 == 0]
# even_squares est [4, 16, 36]
```

Description en langage naturel : "crée une liste de `n*n` pour chaque `n` dans `numbers` **si** `n` est pair".

### Ajout d'une expression conditionnelle (`if/else`)

Que faire si vous voulez appliquer une expression différente selon une condition ? On utilise alors l'opérateur ternaire `if/else` **avant** la boucle `for`.

**Syntaxe** : `[<expr_si_vrai> if <condition> else <expr_si_faux> for <element> in <iterable>]`

**Exemple** : Créer une liste qui indique si chaque nombre est "pair" ou "impair".

```python
numbers = [1, 2, 3, 4, 5]
labels = ["even" if n % 2 == 0 else "odd" for n in numbers]
# labels est ['odd', 'even', 'odd', 'even', 'odd']
```

Description en langage naturel : "crée une liste avec 'even' si `n` est pair, sinon 'odd', pour chaque `n` dans `numbers`".

## 4. Au-delà des listes

La même syntaxe peut être utilisée pour créer d'autres types de collections :

- **Compréhensions d'ensemble (`set`)** : Pour créer un ensemble (garantit l'unicité).

  ```python
  numbers = [1, 2, 2, 3, 1]
  unique_squares = {n * n for n in numbers}
  # Résultat : {1, 4, 9}
  ```

- **Compréhensions de dictionnaire (`dict`)** : Pour créer un dictionnaire.
  ```python
  names = ["Alice", "Bob", "Charlie"]
  name_lengths = {name: len(name) for name in names}
  # Résultat : {'Alice': 5, 'Bob': 3, 'Charlie': 7}
  ```

## 5. Quand ne PAS les utiliser ?

Les compréhensions de liste sont puissantes, mais il ne faut pas en abuser.

- **Évitez les compréhensions trop complexes** : Si votre compréhension de liste devient très longue, avec plusieurs boucles `for` imbriquées ou une logique `if/else` compliquée, une boucle `for` traditionnelle sera plus lisible.
- **N'utilisez pas de compréhension de liste si vous n'avez pas besoin de créer une liste** : Si vous voulez juste exécuter une action pour chaque élément (comme un `print`), utilisez une boucle `for` normale. Une compréhension de liste qui n'est pas assignée à une variable crée une liste en mémoire pour rien.

✅ **Règle générale** : Si c'est concis et lisible, utilisez une compréhension. Si ça devient confus, revenez à une boucle `for`.
