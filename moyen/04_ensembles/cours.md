# Module : Les Ensembles (`set`)

## 1. Quoi : Les Ensembles

Un **ensemble** (`set`) est une collection **non ordonnée** et **muable** d'éléments **uniques**.

- **Non ordonnée** : Les éléments n'ont pas de position ou d'index. L'ordre d'insertion n'est pas garanti.
- **Muable** : Vous pouvez ajouter ou supprimer des éléments.
- **Uniques** : Un ensemble ne peut pas contenir de doublons. Si vous ajoutez un élément qui existe déjà, l'ensemble ne change pas.

## 2. Pourquoi : Unicité et Opérations Mathématiques

Les ensembles sont extrêmement utiles pour deux raisons principales :

1.  **Garantir l'unicité** : Le cas d'usage le plus courant est de supprimer les doublons d'une liste.
2.  **Opérations mathématiques sur les ensembles** : Les ensembles en Python implémentent les opérations de la théorie des ensembles (union, intersection, différence), ce qui est très puissant pour comparer des collections de données.

## 3. Comment : Création et Manipulation

### A. Création

- Avec la fonction `set()` à partir d'un itérable (comme une liste).
- Avec des accolades `{}` pour définir les éléments directement.

```python
# Créer un ensemble à partir d'une liste (supprime les doublons)
my_list = [1, 2, 2, 3, 4, 3]
my_set = set(my_list)
print(my_set)  # {1, 2, 3, 4}

# Créer un ensemble directement avec des accolades
fruits = {"apple", "banana", "cherry"}

# ⚠️ Attention : Pour créer un ensemble vide, vous DEVEZ utiliser set().
# {} crée un dictionnaire vide !
empty_set = set()
empty_dict = {}
```

### B. Ajout et Suppression

- `.add(element)` : Ajoute un élément. Ne fait rien si l'élément est déjà présent.
- `.remove(element)` : Supprime un élément. Lève une `KeyError` si l'élément n'existe pas.
- `.discard(element)` : Supprime un élément. **Ne lève pas d'erreur** si l'élément n'existe pas (plus sûr).
- `.pop()` : Supprime et retourne un élément arbitraire (car les ensembles ne sont pas ordonnés).

```python
colors = {"red", "green", "blue"}

# Ajout
colors.add("yellow")
colors.add("red") # Ne fait rien, "red" est déjà là
print(colors) # {'red', 'green', 'blue', 'yellow'}

# Suppression
colors.remove("green")
# colors.remove("purple") # ❌ Lèverait une KeyError

colors.discard("blue")
colors.discard("purple") # Ne fait rien, pas d'erreur
print(colors) # {'red', 'yellow'}
```

### C. Opérations sur les Ensembles

C'est là que les ensembles montrent toute leur puissance.

Soient deux ensembles :

```python
set_a = {1, 2, 3, 4}
set_b = {3, 4, 5, 6}
```

- **Union** (`|` ou `.union()`) : Tous les éléments qui sont dans A, ou dans B, ou dans les deux.

  ```python
  union_set = set_a | set_b
  # ou set_a.union(set_b)
  print(union_set) # {1, 2, 3, 4, 5, 6}
  ```

- **Intersection** (`&` ou `.intersection()`) : Les éléments qui sont **à la fois** dans A et dans B.

  ```python
  intersection_set = set_a & set_b
  # ou set_a.intersection(set_b)
  print(intersection_set) # {3, 4}
  ```

- **Différence** (`-` ou `.difference()`) : Les éléments qui sont dans A mais **pas** dans B.

  ```python
  difference_set = set_a - set_b
  # ou set_a.difference(set_b)
  print(difference_set) # {1, 2}
  ```

- **Différence Symétrique** (`^` ou `.symmetric_difference()`) : Les éléments qui sont dans A ou dans B, mais **pas dans les deux**.
  ```python
  sym_diff_set = set_a ^ set_b
  # ou set_a.symmetric_difference(set_b)
  print(sym_diff_set) # {1, 2, 5, 6}
  ```

### D. Itération et Appartenance

- On peut itérer sur un ensemble avec une boucle `for`.
- On peut vérifier l'appartenance d'un élément avec l'opérateur `in`. C'est une opération **très rapide** sur les ensembles.

```python
my_set = {10, 20, 30}

for item in my_set:
    print(item)

print(20 in my_set) # True
print(50 in my_set) # False
```

## 4. Cas d'usage typique : Trouver les éléments uniques

Le cas d'usage le plus fréquent est la déduplication d'une liste.

```python
email_list = ["a@a.com", "b@b.com", "a@a.com", "c@c.com"]

# 1. Convertir la liste en ensemble pour supprimer les doublons
unique_emails_set = set(email_list)

# 2. Si besoin, reconvertir en liste
unique_emails_list = list(unique_emails_set)

print(unique_emails_list) # ['c@c.com', 'b@b.com', 'a@a.com'] (l'ordre peut varier)
```
