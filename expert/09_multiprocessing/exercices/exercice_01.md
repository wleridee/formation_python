# Exercice 01 : Parallélisation d'un Calcul de Factorielles avec `Pool.map`

## Objectif

Cet exercice a pour but de vous faire utiliser un `Pool` de workers du module `multiprocessing` pour paralléliser une tâche `CPU-bound`. Vous comparerez le temps d'exécution de la version parallèle avec une version séquentielle pour visualiser le gain de performance.

## Contexte

Le calcul de la factorielle d'un grand nombre est une opération `CPU-bound`. Si vous devez calculer la factorielle pour une longue liste de nombres, le faire séquentiellement peut prendre beaucoup de temps. C'est un cas d'usage parfait pour la parallélisation avec `multiprocessing`.

Vous allez écrire une fonction qui calcule la factorielle, puis l'appliquer à une liste de nombres en utilisant `pool.map()` pour distribuer le travail sur plusieurs cœurs de CPU.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `parallel_factorial.py`.

2.  **Importez les modules nécessaires** : `multiprocessing`, `time`, et `math` (pour `math.factorial`).

3.  **Définissez la fonction de travail**.

    - Créez une fonction `calculer_factorielle(n)` qui prend un entier `n`.
    - Cette fonction doit calculer la factorielle de `n`. Pour rendre la tâche plus longue et l'effet de la parallélisation plus visible, vous pouvez ajouter une boucle qui consomme du CPU avant le calcul, ou simplement utiliser `math.factorial` sur de grands nombres.

    ```python
    import math

    def calculer_factorielle(n):
        # math.factorial est déjà très rapide car implémenté en C.
        # Pour simuler une charge de travail plus lourde en Python pur,
        # on pourrait faire une boucle, mais pour cet exercice,
        # l'utilisation de grands nombres suffira à montrer le principe.
        return math.factorial(n)
    ```

4.  **Préparez les données**.

    - Créez une liste de nombres pour lesquels vous voulez calculer la factorielle. Utilisez des nombres assez grands pour que le calcul prenne un temps mesurable, par exemple `list(range(20000, 20020))`.

5.  **Implémentez et mesurez la version séquentielle**.

    - Protégez votre code principal avec `if __name__ == '__main__':`.
    - Enregistrez le temps de début.
    - Utilisez une compréhension de liste ou une boucle `for` pour appeler `calculer_factorielle` pour chaque nombre de votre liste.
    - Enregistrez le temps de fin et affichez la durée.

6.  **Implémentez et mesurez la version parallèle**.

    - Juste après la version séquentielle, réinitialisez le temps de début.
    - Créez un `Pool` de workers en utilisant `with multiprocessing.Pool() as pool:`. Laisser le nombre de processus vide pour qu'il soit déterminé automatiquement (`os.cpu_count()`).
    - Utilisez `pool.map(calculer_factorielle, nombres)` pour distribuer le calcul sur les workers.
    - Enregistrez le temps de fin et affichez la durée.

7.  **Comparez les résultats**.
    - Affichez les durées des deux versions pour comparer. La version parallèle devrait être significativement plus rapide sur une machine multi-cœur.

## Résultat Attendu

Les temps exacts dépendront du nombre de cœurs de votre CPU et de sa vitesse, mais la version parallèle devrait montrer un gain de performance clair.

```
Calcul des factorielles pour 20 nombres...

--- Version Séquentielle ---
Durée : 1.52 secondes

--- Version Parallèle avec Pool ---
Durée : 0.45 secondes

Gain de performance : La version parallèle est environ 3.4 fois plus rapide.
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# parallel_factorial.py

import multiprocessing
import time
import math

def calculer_factorielle(n):
    """
    Calcule la factorielle d'un nombre.
    C'est une tâche gourmande en CPU pour de grands nombres.
    """
    return math.factorial(n)

if __name__ == '__main__':
    # Utiliser des nombres assez grands pour que le calcul soit non trivial
    NUMBERS = list(range(30000, 30025))
    print(f"Calcul des factorielles pour {len(NUMBERS)} nombres (de {NUMBERS[0]} à {NUMBERS[-1]})...")

    # --- 1. Version Séquentielle ---
    print("\n--- Version Séquentielle ---")
    start_time_seq = time.time()

    # Utilise map() de base, qui est séquentiel
    resultats_seq = list(map(calculer_factorielle, NUMBERS))

    end_time_seq = time.time()
    duration_seq = end_time_seq - start_time_seq
    print(f"Durée : {duration_seq:.2f} secondes")

    # --- 2. Version Parallèle ---
    print("\n--- Version Parallèle avec Pool ---")
    start_time_parallel = time.time()

    # Crée un pool de processus. Le nombre de workers est géré automatiquement.
    with multiprocessing.Pool() as pool:
        # pool.map distribue les éléments de NUMBERS aux processus workers
        # et applique la fonction calculer_factorielle sur chacun.
        resultats_parallel = pool.map(calculer_factorielle, NUMBERS)

    end_time_parallel = time.time()
    duration_parallel = end_time_parallel - start_time_parallel
    print(f"Durée : {duration_parallel:.2f} secondes")

    # --- 3. Conclusion ---
    print("\n--- Conclusion ---")
    if duration_parallel < duration_seq:
        gain = duration_seq / duration_parallel
        print(f"La version parallèle est environ {gain:.1f} fois plus rapide.")
    else:
        print("La version parallèle n'a pas montré de gain de performance.")

    # Vérification que les résultats sont identiques
    assert resultats_seq == resultats_parallel
    print("Les résultats des deux versions sont identiques.")

```

</details>
