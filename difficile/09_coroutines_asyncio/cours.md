# Module : Introduction aux Coroutines et à `asyncio`

## Objectif

Ce module a pour but de vous initier à la programmation asynchrone en Python. Vous découvrirez les concepts de coroutines, de boucles d'événements et comment utiliser la bibliothèque `asyncio` pour écrire du code concurrent non bloquant, particulièrement adapté aux opérations d'Entrée/Sortie (I/O).

## 1. Le Problème : Les Opérations Bloquantes

En programmation traditionnelle (synchrone), lorsqu'une tâche attend une opération longue (comme une requête réseau, une lecture de fichier ou un appel à une base de données), tout le programme est bloqué. Il ne peut rien faire d'autre en attendant la fin de cette opération.

Imaginez un serveur web qui doit gérer plusieurs clients. Si une requête nécessite 2 secondes pour aller chercher des données en base, le serveur est incapable de répondre à d'autres clients pendant ce temps. C'est très inefficace.

La programmation asynchrone résout ce problème en permettant au programme de "mettre en pause" une tâche en attente et de travailler sur une autre tâche pendant ce temps.

## 2. Concepts Clés de l'Asynchrone

### a. Coroutines

Une **coroutine** est une fonction spéciale qui peut être mise en pause et reprise plus tard. En Python, on les définit avec la syntaxe `async def`.

```python
import asyncio

async def ma_coroutine():
    print("Début de la coroutine")
    # Simule une opération I/O longue (ex: requête réseau)
    await asyncio.sleep(1)
    print("Fin de la coroutine")

# Appeler ma_coroutine() ne l'exécute PAS directement.
# Cela retourne un objet coroutine.
coro_obj = ma_coroutine()
print(coro_obj)
# <coroutine object ma_coroutine at 0x...>
```

- `async def` : Déclare une fonction comme étant une coroutine.
- `await` : Met en pause l'exécution de la coroutine actuelle et rend la main à la boucle d'événements. Le programme peut alors exécuter une autre tâche. L'expression après `await` doit être un objet "awaitable" (généralement une autre coroutine ou un objet spécial).

### b. Boucle d'Événements (Event Loop)

La **boucle d'événements** est le cœur de `asyncio`. C'est elle qui gère et distribue l'exécution des différentes tâches. Son rôle est simple :

1.  Prendre une tâche à exécuter.
2.  L'exécuter jusqu'à ce qu'elle rencontre un `await`.
3.  Mettre cette tâche en pause.
4.  Chercher une autre tâche prête à être reprise.
5.  Recommencer.

Pour exécuter une coroutine, on doit la lancer dans une boucle d'événements.

```python
import asyncio

async def ma_coroutine():
    print("Début de la coroutine")
    await asyncio.sleep(1)
    print("Fin de la coroutine")

# asyncio.run() est la manière moderne de démarrer une coroutine.
# Elle crée une boucle d'événements, y exécute la coroutine,
# et ferme la boucle à la fin.
asyncio.run(ma_coroutine())

# Output:
# Début de la coroutine
# (pause de 1 seconde)
# Fin de la coroutine
```

## 3. Exécuter Plusieurs Tâches en Concurrence

L'intérêt principal de l'asynchrone est de faire plusieurs choses "en même temps" (de manière concurrente, pas parallèle). Pour cela, on utilise `asyncio.gather`.

`asyncio.gather` prend plusieurs coroutines et les exécute de manière concurrente. Il attend que toutes soient terminées.

```python
import asyncio
import time

async def dire_apres(delai, mot):
    print(f"Début de '{mot}'")
    await asyncio.sleep(delai)
    print(mot)
    return len(mot)

async def main():
    debut = time.time()

    # Crée les tâches à exécuter
    tache1 = dire_apres(1, "hello")
    tache2 = dire_apres(2, "world")
    tache3 = dire_apres(0.5, "async")

    # Exécute les tâches de manière concurrente
    # gather attend que toutes les coroutines soient finies
    resultats = await asyncio.gather(tache1, tache2, tache3)

    fin = time.time()
    print(f"Programme terminé en {fin - debut:.2f} secondes.")
    print(f"Résultats : {resultats}")

asyncio.run(main())
```

**Analyse de l'exécution :**

1.  `main` démarre.
2.  `gather` lance les 3 coroutines `dire_apres`.
3.  `dire_apres(0.5, "async")` est la première à se terminer après 0.5s.
4.  `dire_apres(1, "hello")` se termine ensuite.
5.  `dire_apres(2, "world")` se termine en dernier.

Le temps total d'exécution sera d'environ **2 secondes**, soit la durée de la tâche la plus longue. En mode synchrone, cela aurait pris `1 + 2 + 0.5 = 3.5` secondes. C'est là tout le gain de la programmation asynchrone !

**Résultat attendu :**

```
Début de 'hello'
Début de 'world'
Début de 'async'
async
hello
world
Programme terminé en 2.00 secondes.
Résultats : [5, 5, 5]
```

## 4. Quand utiliser `asyncio` ?

La programmation asynchrone est particulièrement efficace pour les programmes **limités par les I/O (I/O-bound)**. C'est-à-dire les programmes qui passent beaucoup de temps à attendre :

- Des requêtes réseau (API, web scraping).
- Des réponses de bases de données.
- Des lectures/écritures sur le disque.
- Des communications avec d'autres processus.

Elle n'est **pas adaptée** pour les programmes **limités par le CPU (CPU-bound)**, qui effectuent des calculs intensifs (ex: traitement d'image, calculs mathématiques lourds). Pour ces cas, le `multiprocessing` est plus approprié car il utilise plusieurs cœurs de processeur.

## 5. `asyncio.create_task`

Une autre façon de lancer des coroutines en arrière-plan sans attendre immédiatement leur résultat est `asyncio.create_task`. Cela transforme une coroutine en une `Task` qui est immédiatement planifiée sur la boucle d'événements.

```python
import asyncio

async def operation_longue():
    print("La tâche de fond commence...")
    await asyncio.sleep(2)
    print("La tâche de fond est terminée.")

async def main():
    print("Lancement de la tâche de fond.")
    # La tâche est lancée mais on n'attend pas ici
    task = asyncio.create_task(operation_longue())

    # On peut faire autre chose pendant que la tâche s'exécute
    print("Le programme principal continue...")
    await asyncio.sleep(1)
    print("Le programme principal a fait une pause.")

    # On peut attendre la fin de la tâche plus tard si besoin
    await task
    print("Le programme principal a attendu la fin de la tâche.")

asyncio.run(main())
```

**Résultat attendu :**

```
Lancement de la tâche de fond.
La tâche de fond commence...
Le programme principal continue...
(pause de 1s)
Le programme principal a fait une pause.
(pause de 1s)
La tâche de fond est terminée.
Le programme principal a attendu la fin de la tâche.
```

## Conclusion

`asyncio` est un outil puissant mais complexe. Il change radicalement la manière de penser la structure d'un programme. En maîtrisant `async def`, `await`, `asyncio.run()` et `asyncio.gather`, vous avez les bases pour écrire des applications I/O-bound très performantes et capables de gérer des milliers d'opérations concurrentes avec une seule thread.
