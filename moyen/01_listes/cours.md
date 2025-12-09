# Module : Les Listes

## 1. Quoi : Les Listes

Une **liste** (`list`) est une collection **ordonnée** et **muable** (modifiable) d'éléments. C'est l'un des types de données les plus polyvalents et les plus utilisés en Python.

- **Ordonnée** : Les éléments conservent l'ordre dans lequel ils ont été ajoutés.
- **Muable** : Vous pouvez ajouter, supprimer ou modifier des éléments après la création de la liste.
- Les éléments peuvent être de **types différents** (bien qu'il soit courant d'avoir des listes avec des éléments de même type).

## 2. Pourquoi : Gérer des collections de données

Les listes sont essentielles pour regrouper et gérer des ensembles de données.

- Stocker une liste d'utilisateurs, de produits, de scores.
- Représenter une série de valeurs à traiter (par exemple, les lignes d'un fichier).
- Servir de base à de nombreuses autres structures de données.

## 3. Comment : Création et Manipulation

### A. Création

On crée une liste avec des crochets `[]`, en séparant les éléments par des virgules.

```python
# Une liste de nombres
numbers = [1, 2, 3, 4, 5]

# Une liste de chaînes de caractères
fruits = ["apple", "banana", "cherry"]

# Une liste avec des types mixtes
mixed_list = [1, "hello", 3.14, True]

# Une liste vide
empty_list = []
```

### B. Accès aux éléments

L'accès aux éléments se fait par **index**, comme pour les chaînes de caractères. L'indexation commence à 0.

```python
fruits = ["apple", "banana", "cherry"]

first_fruit = fruits[0]    # "apple"
second_fruit = fruits[1]   # "banana"
last_fruit = fruits[-1]    # "cherry"
```

### C. Modification des éléments

Comme les listes sont muables, vous pouvez modifier un élément en le réassignant via son index.

```python
fruits = ["apple", "banana", "cherry"]
fruits[1] = "blueberry"
print(fruits)  # Affiche : ['apple', 'blueberry', 'cherry']
```

### D. Ajout d'éléments

- `.append(element)` : Ajoute un élément **à la fin** de la liste.
- `.insert(index, element)` : Insère un élément **à une position spécifique**.

```python
numbers = [1, 2, 3]

# Ajoute à la fin
numbers.append(4)
print(numbers)  # [1, 2, 3, 4]

# Insère à l'index 1
numbers.insert(1, 99)
print(numbers)  # [1, 99, 2, 3, 4]
```

### E. Suppression d'éléments

- `.remove(element)` : Supprime la **première occurrence** de la valeur spécifiée.
- `del list[index]` : Supprime l'élément à l'index spécifié.
- `.pop(index)` : Supprime l'élément à l'index spécifié et **le retourne**. Si aucun index n'est fourni, supprime et retourne le dernier élément.

```python
letters = ['a', 'b', 'c', 'b', 'd']

# Supprimer par valeur
letters.remove('b') # Supprime le premier 'b'
print(letters)  # ['a', 'c', 'b', 'd']

# Supprimer par index
del letters[2] # Supprime l'élément à l'index 2 ('b')
print(letters)  # ['a', 'c', 'd']

# Supprimer et récupérer le dernier élément
last_item = letters.pop()
print(f"Popped item: {last_item}") # Popped item: d
print(f"List after pop: {letters}") # List after pop: ['a', 'c']
```

### F. Slicing (Découpage)

Le slicing fonctionne comme pour les chaînes et permet de créer une **nouvelle liste** (une copie) contenant une sous-partie de la liste originale.

```python
numbers = [0, 1, 2, 3, 4, 5, 6]

# Éléments de l'index 2 à 4 (exclu)
sub_list = numbers[2:4]  # [2, 3]

# Du début jusqu'à l'index 3 (exclu)
first_three = numbers[:3] # [0, 1, 2]

# De l'index 4 jusqu'à la fin
from_four = numbers[4:] # [4, 5, 6]

# Créer une copie complète de la liste
list_copy = numbers[:]
```

### G. Fonctions et Méthodes Utiles

- `len(list)` : Retourne le nombre d'éléments dans la liste.
- `list.sort()` : Trie la liste **en place** (modifie la liste originale).
- `sorted(list)` : Retourne une **nouvelle liste triée** (laisse l'originale intacte).
- `list.reverse()` : Inverse l'ordre des éléments **en place**.

```python
numbers = [3, 1, 4, 1, 5, 9]

# Trier en place
numbers.sort()
print(numbers) # [1, 1, 3, 4, 5, 9]

# Inverser en place
numbers.reverse()
print(numbers) # [9, 5, 4, 3, 1, 1]

# Obtenir une version triée sans modifier l'original
unsorted_list = [5, 2, 8]
sorted_copy = sorted(unsorted_list)
print(f"Original: {unsorted_list}") # Original: [5, 2, 8]
print(f"Sorted copy: {sorted_copy}") # Sorted copy: [2, 5, 8]
```

### H. L'opérateur `in`

Pour vérifier si un élément est présent dans une liste.

```python
fruits = ["apple", "banana", "cherry"]
print("banana" in fruits)  # True
print("orange" in fruits) # False
```
