# Exercice 01 : Décorateur de Journalisation (Logging)

## Objectif

Cet exercice a pour but de vous faire créer un décorateur simple qui journalise (log) les informations sur l'appel d'une fonction : son nom, les arguments avec lesquels elle a été appelée, et la valeur qu'elle a retournée.

## Contexte

Le logging est un cas d'usage parfait pour les décorateurs. Vous voulez souvent savoir quand une fonction est appelée et avec quels paramètres, surtout lors du débogage. Au lieu d'ajouter des `print()` dans chaque fonction, vous pouvez simplement la décorer.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `logger_decorator.py`.
2.  **Importez le module `functools`**.

3.  **Définissez votre décorateur** nommé `log_function_call`.

    - Il doit accepter une fonction `func` comme argument.
    - À l'intérieur, définissez une fonction `wrapper` qui accepte `*args` et `**kwargs` pour être compatible avec n'importe quelle fonction.
    - N'oubliez pas de décorer votre `wrapper` avec `@functools.wraps(func)` pour préserver les métadonnées de la fonction originale.

4.  **Implémentez la logique du `wrapper`** :
    a. Avant d'appeler la fonction originale, affichez un message indiquant que la fonction est sur le point d'être appelée, incluant son nom (`func.__name__`), ses arguments positionnels (`args`) et ses arguments nommés (`kwargs`).
    b. Appelez la fonction originale `func` avec `*args` et `**kwargs` et stockez le résultat dans une variable `result`.
    c. Après l'appel, affichez un message indiquant que la fonction a terminé, et montrez la valeur de retour `result`.
    d. Le `wrapper` doit retourner `result`.

5.  **Le décorateur `log_function_call` doit retourner la fonction `wrapper`**.

6.  **Testez votre décorateur** :
    - Créez une fonction simple, par exemple `add(a, b)`, qui retourne la somme de deux nombres.
    - Décorez-la avec `@log_function_call`.
    - Appelez la fonction décorée `add(3, 5)` et observez la sortie.
    - Créez une autre fonction, par exemple `greet(name, greeting="Hello")`, qui retourne une chaîne de salutation.
    - Décorez-la également et appelez-la avec des arguments positionnels et nommés, comme `greet("Alice", greeting="Hi")`.

## Résultat Attendu

```
Calling function 'add' with args=(3, 5) and kwargs={}
Function 'add' returned 8
Result of add(3, 5): 8
---
Calling function 'greet' with args=('Alice',) and kwargs={'greeting': 'Hi'}
Function 'greet' returned 'Hi, Alice!'
Result of greet("Alice", greeting="Hi"): Hi, Alice!
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# logger_decorator.py
import functools

def log_function_call(func):
    """A decorator that logs a function's name, arguments, and return value."""
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        # Log before the call
        print(f"Calling function '{func.__name__}' with args={args} and kwargs={kwargs}")

        # Call the original function
        result = func(*args, **kwargs)

        # Log after the call
        print(f"Function '{func.__name__}' returned {result!r}") # Using !r to get the repr() of the result

        return result
    return wrapper

# --- Testing ---

@log_function_call
def add(a, b):
    """Returns the sum of two numbers."""
    return a + b

@log_function_call
def greet(name, greeting="Hello"):
    """Returns a greeting string."""
    return f"{greeting}, {name}!"

# Test 1
result1 = add(3, 5)
print(f"Result of add(3, 5): {result1}")

print("---")

# Test 2
result2 = greet("Alice", greeting="Hi")
print(f"Result of greet(\"Alice\", greeting=\"Hi\"): {result2}")

```

</details>
