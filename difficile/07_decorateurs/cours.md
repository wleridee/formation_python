# Module : Les Décorateurs

## 1. Quoi : Les Décorateurs

Un **décorateur** en Python est une fonction qui prend une autre fonction en argument, lui ajoute des fonctionnalités, et retourne une nouvelle fonction (ou la fonction originale modifiée) sans altérer le code source de la fonction de départ.

C'est un concept de **méta-programmation** : du code qui manipule du code.

La syntaxe avec le `@` (le "sucre syntaxique") est la manière la plus courante de les utiliser.

```python
@my_decorator
def my_function():
    print("Hello")
```

est équivalent à :

```python
def my_function():
    print("Hello")

my_function = my_decorator(my_function)
```

## 2. Pourquoi : Ajouter des fonctionnalités transversales

Les décorateurs sont parfaits pour ajouter des fonctionnalités dites "transversales" (_cross-cutting concerns_) à de multiples fonctions sans dupliquer de code.

- **Logging** : Journaliser les appels de fonction, leurs arguments et leurs résultats.
- **Mesure de performance** : Calculer le temps d'exécution d'une fonction.
- **Mise en cache** : Mémoriser le résultat d'une fonction pour des appels futurs avec les mêmes arguments.
- **Contrôle d'accès** : Vérifier si un utilisateur a les droits nécessaires avant d'exécuter une fonction (très courant dans les frameworks web comme Flask ou Django).
- **Validation de données**.

## 3. Comment : Créer un Décorateur

Un décorateur est généralement implémenté comme une fonction qui définit et retourne une autre fonction (une "closure").

**Structure d'un décorateur simple :**

1.  Une fonction externe (le décorateur) qui prend une fonction `func` comme argument.
2.  Une fonction interne (`wrapper`) qui sera la nouvelle fonction. C'est ici qu'on ajoute la nouvelle logique.
3.  Le `wrapper` appelle la fonction originale `func`.
4.  Le décorateur retourne le `wrapper`.

**Exemple : Un décorateur qui mesure le temps d'exécution**

```python
import time

# 1. La fonction décorateur
def timer_decorator(func):
    # 2. La fonction wrapper
    # *args et **kwargs permettent au wrapper d'accepter n'importe quels arguments
    def wrapper(*args, **kwargs):
        start_time = time.time()
        # 3. Appel de la fonction originale
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"Function '{func.__name__}' took {end_time - start_time:.4f} seconds to run.")
        return result
    # 4. Le décorateur retourne le wrapper
    return wrapper

# Utilisation du décorateur
@timer_decorator
def long_running_function(n):
    """Une fonction qui prend un peu de temps."""
    total = 0
    for i in range(n):
        total += i
    return total

# Quand on appelle long_running_function, c'est en fait le wrapper qui est appelé.
result = long_running_function(10000000)
# Affiche: Function 'long_running_function' took 0.3456 seconds to run.
```

### Préserver les métadonnées de la fonction : `functools.wraps`

Un problème avec l'exemple ci-dessus est que le décorateur modifie les métadonnées de la fonction originale.

```python
print(long_running_function.__name__) # Affiche 'wrapper', pas 'long_running_function'
```

Pour corriger cela, on utilise le décorateur `@functools.wraps` sur la fonction `wrapper`. Il copie les métadonnées (nom, docstring, etc.) de la fonction originale sur le wrapper.

**Version améliorée du décorateur `timer` :**

```python
import time
import functools

def timer_decorator(func):
    @functools.wraps(func) # On décore le wrapper
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"Function '{func.__name__}' took {end_time - start_time:.4f} seconds.")
        return result
    return wrapper

@timer_decorator
def another_function():
    """This is a docstring."""
    pass

print(another_function.__name__) # Affiche 'another_function'
print(another_function.__doc__)  # Affiche 'This is a docstring.'
```

## 4. Décorateurs avec arguments

Parfois, on veut pouvoir configurer un décorateur. Pour cela, on doit ajouter un niveau de fonction supplémentaire.

**Exemple : Un décorateur qui répète une fonction `n` fois**

```python
import functools

# 1. La fonction externe prend les arguments du décorateur
def repeat(num_times):
    # 2. Elle retourne le décorateur lui-même
    def decorator_repeat(func):
        @functools.wraps(func)
        # 3. Le wrapper a accès à num_times
        def wrapper(*args, **kwargs):
            for _ in range(num_times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator_repeat

# Utilisation du décorateur avec un argument
@repeat(num_times=3)
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")
# Affiche:
# Hello, Alice!
# Hello, Alice!
# Hello, Alice!
```

## 5. Décorateurs de classe

Il est aussi possible de créer des décorateurs en utilisant des classes (en implémentant la méthode `__call__`), mais les décorateurs basés sur des fonctions sont plus courants et souvent plus simples à comprendre pour commencer. Les décorateurs que nous avons déjà vus (`@property`, `@classmethod`, `@staticmethod`) sont des exemples de décorateurs intégrés au langage.
