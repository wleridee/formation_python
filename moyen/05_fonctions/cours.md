# Module : Les Fonctions

## 1. Quoi : Les Fonctions

Une **fonction** est un bloc de code réutilisable qui effectue une action spécifique. Les fonctions permettent de structurer le code en le décomposant en tâches plus petites et plus gérables.

Une fonction peut :

- Prendre des **paramètres** (ou arguments) en entrée.
- Effectuer une série d'opérations.
- **Retourner** une valeur en sortie.

## 2. Pourquoi : Le Principe DRY (Don't Repeat Yourself)

Les fonctions sont au cœur du principe **DRY** ("Ne vous répétez pas").

- **Réutilisabilité** : Au lieu de copier-coller le même code à plusieurs endroits, vous l'écrivez une seule fois dans une fonction et l'appelez autant de fois que nécessaire.
- **Modularité** : Elles décomposent un problème complexe en sous-problèmes plus simples. Chaque fonction a une responsabilité unique.
- **Lisibilité** : Un code bien structuré avec des fonctions aux noms clairs est beaucoup plus facile à lire et à comprendre.
- **Maintenance** : Si vous devez corriger un bug ou modifier une logique, vous ne le faites qu'à un seul endroit : dans la fonction elle-même.

## 3. Comment : Définir et Appeler une Fonction

### A. Définition et Appel

- On définit une fonction avec le mot-clé `def`, suivi du nom de la fonction, de parenthèses `()` et de deux-points `:`.
- Le code à l'intérieur de la fonction doit être indenté.
- On **appelle** la fonction en utilisant son nom suivi des parenthèses.

```python
# Définition de la fonction
def greet():
    print("Hello, World!")

# Appel de la fonction
greet() # Affiche "Hello, World!"
greet() # Affiche "Hello, World!"
```

### B. Paramètres et Arguments

- Un **paramètre** est la variable listée à l'intérieur des parenthèses dans la définition de la fonction.
- Un **argument** est la valeur qui est envoyée à la fonction lorsque vous l'appelez.

```python
# 'name' est un paramètre
def greet_user(name):
    print(f"Hello, {name}!")

# "Alice" et "Bob" sont des arguments
greet_user("Alice") # Affiche "Hello, Alice!"
greet_user("Bob")   # Affiche "Hello, Bob!"
```

### C. L'instruction `return`

L'instruction `return` est utilisée pour qu'une fonction renvoie une valeur. Lorsqu'une instruction `return` est exécutée, la fonction se termine immédiatement.

Une fonction qui ne contient pas d'instruction `return` renvoie implicitement `None`.

```python
# Cette fonction prend deux nombres et retourne leur somme
def add(a, b):
    result = a + b
    return result

# On appelle la fonction et on stocke la valeur de retour dans une variable
sum_result = add(5, 3)
print(sum_result) # Affiche 8
```

### D. Paramètres par Défaut

Vous pouvez assigner une valeur par défaut à un paramètre. Si un argument pour ce paramètre n'est pas fourni lors de l'appel, la valeur par défaut sera utilisée.

Les paramètres avec une valeur par défaut doivent être placés **après** les paramètres sans valeur par défaut.

```python
def greet_with_message(name, message="Welcome"):
    print(f"{message}, {name}!")

greet_with_message("Alice") # Utilise la valeur par défaut -> "Welcome, Alice!"
greet_with_message("Bob", "Good morning") # Fournit une valeur -> "Good morning, Bob!"
```

### E. Arguments Nommés (Keyword Arguments)

Lorsque vous appelez une fonction, vous pouvez spécifier les arguments par leur nom. Cela améliore la lisibilité et vous permet de ne pas vous soucier de l'ordre des arguments.

```python
def create_user(name, age, is_admin):
    print(f"Creating user {name}, age {age}, admin status: {is_admin}")

# Appel avec des arguments positionnels (l'ordre compte)
create_user("Alice", 30, False)

# Appel avec des arguments nommés (l'ordre ne compte pas)
create_user(age=30, is_admin=False, name="Alice")
```

### F. Docstrings (Chaînes de Documentation)

C'est une bonne pratique d'inclure une chaîne de caractères (docstring) juste après la ligne de définition de la fonction pour expliquer ce qu'elle fait.

```python
def calculate_area(radius):
    """
    Calculates the area of a circle given its radius.

    Args:
        radius (float): The radius of the circle.

    Returns:
        float: The area of the circle.
    """
    pi = 3.14159
    return pi * (radius ** 2)

# Les outils et les IDE peuvent afficher cette documentation.
help(calculate_area)
```

## 4. Portée des Variables (Scope)

- **Variable locale** : Une variable créée à l'intérieur d'une fonction n'est accessible qu'à l'intérieur de cette fonction.
- **Variable globale** : Une variable créée à l'extérieur de toute fonction est globale et peut être lue de n'importe où dans le script.

```python
global_var = "I am global"

def my_function():
    local_var = "I am local"
    print(global_var) # OK pour lire une variable globale
    print(local_var)

my_function()
# print(local_var) # ❌ Erreur ! NameError: name 'local_var' is not defined
```

✅ **Bonne pratique** : Les fonctions doivent, autant que possible, dépendre de leurs paramètres d'entrée plutôt que de variables globales. Cela les rend plus autonomes et plus faciles à tester.
