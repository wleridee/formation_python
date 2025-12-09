# Exercice 01 : Générateur de la Suite de Fibonacci

## Objectif

Cet exercice a pour but de vous faire implémenter une fonction génératrice pour produire les nombres de la suite de Fibonacci, en illustrant l'avantage des générateurs pour créer des séquences potentiellement infinies avec une consommation mémoire minimale.

## Contexte

La suite de Fibonacci est une suite de nombres où chaque nombre est la somme des deux précédents. Elle commence généralement par 0 et 1.
`0, 1, 1, 2, 3, 5, 8, 13, 21, ...`

Créer une liste de tous les nombres de Fibonacci jusqu'à une très grande limite pourrait consommer beaucoup de mémoire. Un générateur est donc la solution parfaite.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `fibonacci_generator.py`.

2.  **Définissez une fonction génératrice** nommée `fibonacci_sequence` qui accepte un argument `limit`.

    - Cette fonction générera les nombres de Fibonacci tant qu'ils sont inférieurs à `limit`.

3.  **À l'intérieur de la fonction** :
    a. Initialisez les deux premiers nombres de la suite, par exemple `a, b = 0, 1`.
    b. Utilisez une boucle `while` qui continue tant que le nombre actuel (`a`) est inférieur à `limit`.
    c. Dans la boucle : - Utilisez `yield a` pour produire le nombre actuel de la suite. - Mettez à jour les variables pour calculer le nombre suivant. L'ancien `b` devient le nouveau `a`, et la somme `a + b` devient le nouveau `b`. - _Astuce :_ L'assignation multiple en Python, `a, b = b, a + b`, est parfaite pour cela.

4.  **Testez votre générateur** :
    - Créez une boucle `for` pour itérer sur les nombres produits par `fibonacci_sequence(100)` et affichez chaque nombre.
    - Utilisez une expression génératrice et la fonction `sum()` pour calculer la somme des nombres de Fibonacci inférieurs à 1000.

## Résultat Attendu

```
Fibonacci numbers up to 100:
0
1
1
2
3
5
8
13
21
34
55
89

Sum of Fibonacci numbers up to 1000: 2583
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# fibonacci_generator.py

def fibonacci_sequence(limit):
    """
    A generator function that yields Fibonacci numbers up to a given limit.
    """
    # Initialize the first two numbers
    a, b = 0, 1

    # Loop as long as the current number is less than the limit
    while a < limit:
        # Yield the current number, pausing execution
        yield a
        # Update the numbers for the next iteration
        a, b = b, a + b

# --- Testing ---

# Test 1: Print Fibonacci numbers up to 100
print("Fibonacci numbers up to 100:")
# The for loop automatically handles the generator
for number in fibonacci_sequence(100):
    print(number)

print("\n" + "-"*20 + "\n")

# Test 2: Sum of Fibonacci numbers up to 1000 using a generator expression
# This is equivalent to sum(fibonacci_sequence(1000))
fib_gen_expr = (n for n in fibonacci_sequence(1000))
total_sum = sum(fib_gen_expr)

print(f"Sum of Fibonacci numbers up to 1000: {total_sum}")

```

</details>
