# Module : Gestion des Erreurs (`try...except`)

## 1. Quoi : Les Exceptions

Une **exception** est un événement qui se produit durant l'exécution d'un programme et qui interrompt son flux normal. En Python, les erreurs sont principalement gérées via des exceptions.

Quand une erreur se produit, Python crée un **objet exception**. Si cet objet n'est pas géré, le programme s'arrête et affiche un "traceback" (la trace de l'erreur).

Exemples d'exceptions courantes :

- `TypeError` : Opération sur un type inapproprié (ex: `5 + "hello"`).
- `ValueError` : La valeur d'un argument est incorrecte (ex: `int("abc")`).
- `NameError` : Utilisation d'une variable non définie.
- `IndexError` : Accès à un index invalide dans une séquence (ex: `my_list[99]`).
- `KeyError` : Accès à une clé inexistante dans un dictionnaire (ex: `my_dict["foo"]`).
- `ZeroDivisionError` : Division par zéro.

## 2. Pourquoi : Écrire du code robuste

La gestion des exceptions est cruciale pour créer des programmes robustes et fiables.

- **Prévenir les crashs** : Au lieu de laisser le programme s'arrêter brutalement, vous pouvez "attraper" l'erreur et la gérer proprement.
- **Gérer les cas inattendus** : Permet de gérer les erreurs d'entrées utilisateur, les problèmes de réseau, les fichiers manquants, etc.
- **Séparer la logique métier du code de gestion d'erreur** : Le code "heureux" (le cas où tout se passe bien) est séparé du code qui gère les problèmes.

## 3. Comment : Le bloc `try...except`

### A. Syntaxe de base

- `try` : Le code susceptible de lever une exception est placé dans le bloc `try`.
- `except` : Si une exception se produit dans le bloc `try`, Python arrête l'exécution de ce bloc et cherche un bloc `except` correspondant. Si un `except` correspond au type de l'exception, son code est exécuté.

```python
try:
    # Code potentiellement dangereux
    number = int(input("Enter a number: "))
    result = 10 / number
    print(f"Result is {result}")
except ValueError:
    # Ce bloc s'exécute si int() échoue (ex: l'utilisateur tape "abc")
    print("Invalid input. Please enter a valid number.")
except ZeroDivisionError:
    # Ce bloc s'exécute si l'utilisateur tape 0
    print("You cannot divide by zero.")
```

### B. Attraper plusieurs exceptions

Vous pouvez attraper plusieurs types d'exceptions dans un seul bloc `except` en les regroupant dans un tuple.

```python
try:
    # ...
except (ValueError, ZeroDivisionError):
    print("Invalid input or division by zero.")
```

### C. Le bloc `except` générique

Un bloc `except` sans type d'exception spécifié attrapera **toutes** les exceptions. C'est généralement une **mauvaise pratique**, car cela peut masquer des erreurs inattendues que vous n'aviez pas prévues.

```python
# ❌ Mauvais : Trop large, peut cacher des bugs
try:
    # ...
except:
    print("An unknown error occurred.")
```

Il est préférable d'utiliser `except Exception as e`, qui attrape la plupart des exceptions standards et vous donne accès à l'objet exception.

```python
# ✅ Bon : Attrape la plupart des erreurs et donne des détails
try:
    # ...
except Exception as e:
    print(f"An error occurred: {e}")
    print(f"Error type: {type(e)}")
```

### D. Le bloc `else`

Le bloc `else` est optionnel et s'exécute **uniquement si aucune exception n'a été levée** dans le bloc `try`. C'est utile pour placer le code qui ne doit s'exécuter que si le bloc `try` a réussi.

```python
try:
    file = open("data.txt", "r")
except FileNotFoundError:
    print("File not found.")
else:
    # Ce code s'exécute seulement si le fichier a été ouvert avec succès
    print("File opened successfully.")
    content = file.read()
    file.close()
    print(f"File content: {content}")
```

### E. Le bloc `finally`

Le bloc `finally` est optionnel et s'exécute **toujours**, qu'une exception ait été levée ou non. C'est l'endroit idéal pour le code de "nettoyage" qui doit absolument être exécuté, comme la fermeture d'un fichier ou d'une connexion réseau.

```python
file = None # On définit la variable avant le try
try:
    file = open("data.txt", "r")
    # ... opérations sur le fichier
except FileNotFoundError:
    print("File not found.")
finally:
    # Ce bloc s'exécute dans tous les cas
    if file: # On vérifie que le fichier a bien été ouvert
        print("Closing the file.")
        file.close()
```

## 4. Lever une exception (`raise`)

Vous pouvez aussi déclencher manuellement une exception avec le mot-clé `raise`. C'est utile pour signaler une erreur dans votre propre code.

```python
def set_age(age):
    if age < 0:
        # On lève une exception si la condition n'est pas respectée
        raise ValueError("Age cannot be negative.")
    print(f"Age set to {age}")

try:
    set_age(25)  # OK
    set_age(-5)  # Lèvera une ValueError
except ValueError as e:
    print(f"Error: {e}")
```
