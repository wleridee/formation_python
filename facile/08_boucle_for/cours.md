# Module : La Boucle `for`

## 1. Quoi : La Boucle `for`

La boucle `for` en Python est utilisée pour **itérer sur une séquence** (comme une liste, un tuple, un dictionnaire, un ensemble ou une chaîne de caractères) et exécuter un bloc de code pour chaque élément de cette séquence.

C'est une boucle de type "for each", ce qui signifie qu'elle parcourt les éléments d'un "itérable" un par un.

## 2. Pourquoi : Automatiser les tâches répétitives

La boucle `for` est l'un des outils les plus puissants pour automatiser des actions sur des collections de données.

- Traiter chaque élément d'une liste.
- Lire chaque ligne d'un fichier.
- Effectuer une action un nombre de fois défini.

## 3. Comment : La Syntaxe

La syntaxe de base est : `for <variable_temporaire> in <iterable>:`

```python
fruits = ["apple", "banana", "cherry"]

# Pour chaque fruit dans la liste 'fruits'...
for fruit in fruits:
    # ...exécute ce bloc de code indenté.
    # La variable 'fruit' prend la valeur de l'élément courant à chaque tour de boucle.
    print(f"Current fruit: {fruit}")

print("Loop finished.")
```

**Sortie :**

```
Current fruit: apple
Current fruit: banana
Current fruit: cherry
Loop finished.
```

### A. Itérer sur une chaîne de caractères

Une chaîne est une séquence de caractères, on peut donc itérer dessus.

```python
for char in "Python":
    print(char)
```

**Sortie :** `P`, `y`, `t`, `h`, `o`, `n` (chacun sur une nouvelle ligne).

### B. La fonction `range()`

Pour exécuter une boucle un nombre de fois spécifique, on utilise la fonction `range()`.

- `range(stop)` : Génère des nombres de 0 jusqu'à `stop - 1`.
- `range(start, stop)` : Génère des nombres de `start` jusqu'à `stop - 1`.
- `range(start, stop, step)` : Génère des nombres de `start` à `stop - 1`, avec un pas de `step`.

```python
# Exécute la boucle 5 fois (pour i = 0, 1, 2, 3, 4)
for i in range(5):
    print(f"Iteration {i}")

# Itère de 2 à 5
for number in range(2, 6):
    print(number) # Affiche 2, 3, 4, 5

# Itère de 10 à 0 en décomptant de 2
for countdown in range(10, 0, -2):
    print(countdown) # Affiche 10, 8, 6, 4, 2
```

✅ **Bon à savoir** : Si vous n'avez pas besoin de la variable de boucle, la convention est d'utiliser un underscore `_`.

```python
for _ in range(3):
    print("Hello!") # Affiche "Hello!" trois fois
```

### C. Les instructions `break` et `continue`

- `break` : **Arrête complètement** la boucle, même s'il reste des éléments dans la séquence.
- `continue` : **Saute l'itération actuelle** et passe directement à la suivante.

```python
numbers = [1, 2, 3, 4, 5, 6]

# Exemple avec break
for number in numbers:
    if number == 4:
        print("Found 4, stopping the loop.")
        break  # Sort de la boucle
    print(number) # Affiche 1, 2, 3

# Exemple avec continue
for number in numbers:
    if number % 2 == 0: # Si le nombre est pair
        continue # Saute le print et passe au nombre suivant
    print(number) # Affiche 1, 3, 5
```

### D. Le bloc `else` dans une boucle `for`

C'est une particularité de Python. Le bloc `else` d'une boucle `for` s'exécute **uniquement si la boucle se termine normalement** (c'est-à-dire sans avoir rencontré de `break`).

C'est utile pour rechercher un élément.

```python
my_list = ["a", "b", "c"]

for item in my_list:
    if item == "d":
        print("Found 'd'!")
        break
else:
    # Ce bloc s'exécute car la boucle a terminé sans trouver 'd'
    print("'d' was not found in the list.")
```

### E. Itérer sur des dictionnaires

Quand on itère sur un dictionnaire, par défaut, on itère sur ses **clés**.

```python
user = {"name": "Alice", "age": 30}

# Itérer sur les clés (comportement par défaut)
for key in user:
    print(key, user[key])

# Itérer sur les valeurs
for value in user.values():
    print(value)

# ✅ Bon : Itérer sur les paires clé/valeur (le plus courant)
for key, value in user.items():
    print(f"{key}: {value}")
```
