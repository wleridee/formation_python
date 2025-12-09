# Exercice 01 : Observer l'Effet du GIL sur les Tâches CPU-Bound

## Objectif

Cet exercice a pour but de démontrer de manière pratique l'effet du Global Interpreter Lock (GIL) sur les performances des tâches limitées par le CPU (`CPU-bound`) en utilisant le module `threading`. Vous comparerez le temps d'exécution d'une tâche de calcul intensif en mode séquentiel et en mode multi-thread.

## Contexte

Une tâche `CPU-bound` est une tâche qui passe la majorité de son temps à effectuer des calculs sur le processeur. Un exemple simple est de compter jusqu'à un très grand nombre.

Si Python permettait un vrai parallélisme avec les threads, exécuter deux tâches de ce type sur deux threads devrait prendre à peu près le même temps qu'en exécuter une seule sur une machine multi-cœur. Cependant, à cause du GIL, un seul thread peut exécuter du code Python à la fois. Cet exercice vise à visualiser ce phénomène.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `gil_demonstration.py`.

2.  **Importez les modules nécessaires** : `time` et `threading`.

3.  **Définissez la tâche de calcul intensif**.

    - Créez une fonction `countdown(n)` qui prend un grand nombre `n` en argument.
    - Dans cette fonction, utilisez une boucle `while` pour décrémenter `n` jusqu'à ce qu'il atteigne 0.
    - Cette fonction ne doit rien retourner. Son seul but est de consommer du temps CPU.

4.  **Définissez une constante** pour le nombre à décompter, par exemple `COUNT = 100_000_000`.

5.  **Mesurez le temps d'exécution en mode séquentiel**.

    - Enregistrez le temps de début avec `time.time()`.
    - Appelez `countdown(COUNT)` deux fois, l'une après l'autre.
    - Enregistrez le temps de fin et affichez la durée totale.

6.  **Mesurez le temps d'exécution en mode multi-thread**.

    - Enregistrez le temps de début.
    - Créez deux threads. Chaque thread doit avoir pour cible la fonction `countdown` avec `COUNT` comme argument.
    - Démarrez les deux threads.
    - Utilisez `thread.join()` pour attendre que les deux threads aient terminé leur exécution.
    - Enregistrez le temps de fin et affichez la durée totale.

7.  **Comparez les résultats**.
    - Analysez les temps d'exécution. Le temps de la version multi-thread devrait être approximativement le double de celui de la version séquentielle (ou du moins, significativement plus long qu'un seul appel), démontrant que les threads n'ont pas pu s'exécuter en parallèle.

## Résultat Attendu

Les temps exacts varieront en fonction de votre machine, mais la conclusion devrait être la même. La version multi-thread n'est pas plus rapide que la version séquentielle, et elle est souvent même un peu plus lente à cause du surcoût lié à la gestion des threads.

```
--- Exécution Séquentielle ---
Début de l'exécution séquentielle...
Durée de l'exécution séquentielle : 8.52 secondes

--- Exécution Multi-thread ---
Début de l'exécution avec des threads...
Durée de l'exécution avec des threads : 8.61 secondes

Conclusion : La version multi-thread n'apporte aucun gain de performance pour cette tâche CPU-bound à cause du GIL.
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# gil_demonstration.py

import time
import threading

# Un grand nombre pour s'assurer que la tâche prend du temps
COUNT = 100_000_000

def countdown(n):
    """Une fonction simple et gourmande en CPU."""
    while n > 0:
        n -= 1

# --- 1. Exécution Séquentielle ---
print("--- Exécution Séquentielle ---")
print("Début de l'exécution séquentielle...")
start_time_seq = time.time()

# Exécute la tâche deux fois de suite dans le thread principal
countdown(COUNT)
countdown(COUNT)

end_time_seq = time.time()
duration_seq = end_time_seq - start_time_seq
print(f"Durée de l'exécution séquentielle : {duration_seq:.2f} secondes\n")


# --- 2. Exécution Multi-thread ---
print("--- Exécution Multi-thread ---")
print("Début de l'exécution avec des threads...")
start_time_thread = time.time()

# Crée deux threads pour exécuter la même tâche
thread1 = threading.Thread(target=countdown, args=(COUNT,))
thread2 = threading.Thread(target=countdown, args=(COUNT,))

# Démarre les threads
thread1.start()
thread2.start()

# Attend que les deux threads aient fini
thread1.join()
thread2.join()

end_time_thread = time.time()
duration_thread = end_time_thread - start_time_thread
print(f"Durée de l'exécution avec des threads : {duration_thread:.2f} secondes\n")

# --- 3. Conclusion ---
print("--- Conclusion ---")
if duration_thread >= duration_seq:
    print("La version multi-thread n'est pas plus rapide, voire plus lente.")
    print("Cela démontre que le GIL empêche les threads d'exécuter le code Python en parallèle sur les cœurs de CPU.")
else:
    # Ce cas ne devrait normalement pas se produire de manière significative
    print("La version multi-thread est légèrement plus rapide, ce qui peut arriver à cause des subtilités du scheduling de l'OS.")

```

**Note pour l'exécuteur :** Si vous exécutez ce code, vous remarquerez peut-être que le temps de la version multi-thread est presque exactement le même que celui de la version séquentielle, et non le double. Cela est dû à la manière dont le GIL est libéré et ré-acquis. Python force un changement de thread après un certain intervalle, donnant l'illusion que les deux threads progressent. Cependant, le temps CPU total utilisé est bien le double, et le temps réel écoulé est au moins égal à la somme des temps d'exécution individuels, prouvant l'absence de parallélisme.

</details>
