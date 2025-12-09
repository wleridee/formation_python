# Exercice 01 : Calculateur de Moyenne Sécurisé

## Objectif

Cet exercice a pour but de vous faire utiliser un bloc `try...except` pour gérer les erreurs potentielles lors du calcul de la moyenne d'une liste de nombres, notamment la division par zéro.

## Contexte

Vous devez écrire une fonction qui calcule la moyenne d'une liste de nombres. Cette fonction doit être robuste et ne pas planter si on lui passe une liste vide (ce qui entraînerait une division par zéro).

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `safe_average.py`.

2.  **Définissez une fonction** nommée `calculate_average` qui accepte un seul paramètre, `numbers_list`.

    - Ajoutez une docstring pour expliquer ce que fait la fonction et quelles erreurs elle peut gérer.

3.  **À l'intérieur de la fonction**, utilisez un bloc `try...except` :
    a. Dans le bloc `try` : - Calculez la somme des nombres de la liste avec `sum()`. - Calculez la longueur de la liste avec `len()`. - Calculez la moyenne en divisant la somme par la longueur. - Retournez la moyenne.

    b. Dans le bloc `except` : - Attrapez spécifiquement l'exception `ZeroDivisionError`. - Affichez un message d'avertissement indiquant que la liste est vide et que la moyenne ne peut pas être calculée. - Retournez `0` comme valeur par défaut pour la moyenne d'une liste vide.

4.  **Testez votre fonction** :
    - Créez deux listes : une avec des nombres et une autre qui est vide.
    - Appelez votre fonction `calculate_average` sur chaque liste et affichez le résultat de manière claire.

## Résultat Attendu

```
Calculating average for [10, 20, 30, 40, 50]...
Average: 30.0

Calculating average for []...
Warning: The list is empty, cannot calculate average.
Average: 0
```

## Bonus (Optionnel)

Modifiez votre fonction pour qu'elle gère également le cas où la liste contient des éléments qui ne sont pas des nombres (ce qui lèverait une `TypeError` avec `sum()`).

- Ajoutez un autre bloc `except TypeError`.
- Affichez un message d'erreur approprié et retournez `None` pour indiquer qu'un calcul est impossible.
- Testez ce nouveau cas avec une liste comme `[10, 20, "thirty"]`.

### Résultat Attendu (avec bonus)

```
Calculating average for [10, 20, 30, 40, 50]...
Average: 30.0

Calculating average for []...
Warning: The list is empty, cannot calculate average.
Average: 0

Calculating average for [10, 20, 'thirty']...
Error: The list contains non-numeric values.
Average: None
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# safe_average.py

def calculate_average(numbers_list):
    """
    Calculates the average of a list of numbers.

    Handles ZeroDivisionError for empty lists and TypeError for non-numeric lists.

    Args:
        numbers_list (list): A list of numbers.

    Returns:
        float or int: The average of the numbers.
        Returns 0 for an empty list.
        Returns None if the list contains non-numeric values.
    """
    try:
        total = sum(numbers_list)
        count = len(numbers_list)
        average = total / count
        return average
    except ZeroDivisionError:
        print("Warning: The list is empty, cannot calculate average.")
        return 0
    except TypeError:
        print("Error: The list contains non-numeric values.")
        return None

# --- Tests ---

# Test 1: Liste normale
list1 = [10, 20, 30, 40, 50]
print(f"Calculating average for {list1}...")
avg1 = calculate_average(list1)
print(f"Average: {avg1}\n")

# Test 2: Liste vide
list2 = []
print(f"Calculating average for {list2}...")
avg2 = calculate_average(list2)
print(f"Average: {avg2}\n")

# Test 3: Liste avec des non-nombres (pour le bonus)
list3 = [10, 20, "thirty"]
print(f"Calculating average for {list3}...")
avg3 = calculate_average(list3)
print(f"Average: {avg3}\n")

```

</details>
