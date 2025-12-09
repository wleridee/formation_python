# Exercice 01 : Profilage et Optimisation d'un Calcul de Fibonacci

## Objectif

Cet exercice a pour but de vous faire utiliser `cProfile` et `pstats` pour profiler une fonction inefficace, identifier le goulot d'étranglement, l'optimiser, et vérifier le gain de performance en profilant à nouveau.

## Contexte

La suite de Fibonacci est un exemple classique en informatique. Une implémentation récursive naïve est très simple à écrire, mais terriblement inefficace car elle recalcule les mêmes valeurs des milliers de fois.

`fib(n) = fib(n-1) + fib(n-2)`

Par exemple, pour calculer `fib(5)`, on calcule `fib(4)` et `fib(3)`. Mais pour `fib(4)`, on recalcule `fib(3)` et `fib(2)`. `fib(3)` est donc calculé deux fois. Ce problème s'aggrave de manière exponentielle.

Vous allez profiler cette fonction, puis l'optimiser en utilisant la **mémoïsation** (une forme de cache), et comparer les résultats.

## Énoncé

### Partie 1 : Profilage de la version inefficace

1.  **Créez un nouveau fichier Python** nommé `profile_fib.py`.

2.  **Écrivez la fonction Fibonacci récursive naïve**.

    ```python
    def fib(n):
        if n < 2:
            return n
        return fib(n - 1) + fib(n - 2)
    ```

3.  **Écrivez une fonction principale `main`** qui appelle `fib(35)`. Le nombre 35 est assez grand pour que la fonction prenne quelques secondes.

    ```python
    def main():
        result = fib(35)
        print(f"Fib(35) = {result}")
    ```

4.  **Profilez le script depuis la ligne de commande**.

    - Exécutez la commande : `python -m cProfile -o fib_stats.prof profile_fib.py`
    - Cela va prendre un certain temps.

5.  **Analysez les résultats avec `pstats`**.
    - Lancez `pstats` : `python -m pstats fib_stats.prof`
    - Triez par `ncalls` (nombre d'appels) et affichez les 5 premières lignes (`sort ncalls`, `stats 5`).
    - Triez par `tottime` (temps total) et affichez les 5 premières lignes (`sort tottime`, `stats 5`).
    - Notez le nombre total d'appels à la fonction `fib` et le temps total d'exécution.

### Partie 2 : Optimisation et nouveau profilage

1.  **Optimisez la fonction `fib` avec la mémoïsation**.

    - La mémoïsation consiste à stocker les résultats des appels de fonction dans un cache (un dictionnaire) pour éviter de les recalculer.
    - Le module `functools` fournit un décorateur parfait pour cela : `@functools.lru_cache`.

2.  **Modifiez votre fichier `profile_fib.py`**.

    - Importez `functools`.
    - Ajoutez le décorateur `@functools.lru_cache(maxsize=None)` juste au-dessus de votre fonction `fib`.

    ```python
    import functools

    @functools.lru_cache(maxsize=None)
    def fib(n):
        # ... même code qu'avant
    ```

3.  **Profilez la version optimisée**.

    - Exécutez à nouveau le profilage, mais en sauvegardant dans un autre fichier : `python -m cProfile -o fib_optimized_stats.prof profile_fib.py`
    - Notez que l'exécution est maintenant quasi instantanée.

4.  **Analysez les nouveaux résultats**.
    - Lancez `pstats` sur le nouveau fichier : `python -m pstats fib_optimized_stats.prof`
    - Triez par `ncalls` et `tottime`.
    - Comparez le nombre d'appels à `fib` et le temps d'exécution avec la version précédente.

## Résultat Attendu

### Analyse de la version 1 (naïve)

- `sort ncalls` / `stats 5` devrait montrer un nombre d'appels à `fib` extraordinairement élevé (plusieurs dizaines de millions).
  ```
  ncalls
  29860703    ... profile_fib.py:4(fib)
  ```
- `sort tottime` / `stats 5` devrait montrer que la quasi-totalité du temps est passée dans la fonction `fib`.
  ```
  tottime
  3.512    ... profile_fib.py:4(fib)
  ```

### Analyse de la version 2 (optimisée)

- `sort ncalls` / `stats 5` devrait montrer que la fonction `fib` n'est appelée qu'environ 36 fois (une fois pour chaque nombre de 0 à 35).
  ```
  ncalls
  36    ... profile_fib.py:7(fib)
  ```
- `sort tottime` / `stats 5` devrait montrer un temps d'exécution négligeable (proche de 0.000s).

Cette comparaison démontre de manière spectaculaire comment le profilage permet d'identifier une fonction inefficace et de valider l'impact d'une optimisation.

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# profile_fib.py

import functools

# Pour tester la version non optimisée, commentez la ligne @functools.lru_cache
@functools.lru_cache(maxsize=None)
def fib(n: int) -> int:
    """
    Calcule le n-ième nombre de Fibonacci.
    Version optimisée avec mémoïsation via lru_cache.
    """
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)

def main():
    """Fonction principale pour lancer le calcul."""
    print("Calcul de Fib(35)...")
    result = fib(35)
    print(f"Fib(35) = {result}")

if __name__ == "__main__":
    main()

```

**Instructions d'exécution (rappel)**

1.  **Version non optimisée :**

    - Commentez la ligne `@functools.lru_cache(maxsize=None)`.
    - `python -m cProfile -o fib_stats.prof profile_fib.py`
    - `python -m pstats fib_stats.prof`
    - Dans pstats, tapez `sort ncalls` puis `stats 5`.

2.  **Version optimisée :**
    - Décommentez la ligne `@functools.lru_cache(maxsize=None)`.
    - `python -m cProfile -o fib_optimized_stats.prof profile_fib.py`
    - `python -m pstats fib_optimized_stats.prof`
    - Dans pstats, tapez `sort ncalls` puis `stats 5`.

</details>
