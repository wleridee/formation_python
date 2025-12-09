# Module : Le Parallélisme avec `multiprocessing`

## Objectif

Ce module a pour but de vous apprendre à utiliser le module `multiprocessing` pour exécuter du code en parallèle et ainsi contourner les limitations du GIL (Global Interpreter Lock) pour les tâches gourmandes en CPU. Vous découvrirez comment créer et gérer des processus, et comment utiliser des `Pool` de workers pour paralléliser des tâches simplement.

## 1. `threading` vs `multiprocessing` : Le Rappel

- **`threading`** : Concurrence. Utile pour les tâches **I/O-bound**. Plusieurs threads s'exécutent dans le même processus, partagent la même mémoire, mais sont limités par le GIL (un seul thread exécute du code Python à la fois).
- **`multiprocessing`** : Parallélisme. La solution pour les tâches **CPU-bound**. Plusieurs processus sont créés. Chaque processus a son propre interpréteur Python, sa propre mémoire et son propre GIL. Ils peuvent donc s'exécuter en véritable parallèle sur plusieurs cœurs de CPU.

**Inconvénient :** La communication entre processus est plus complexe et plus lente que la communication entre threads, car les données doivent être **sérialisées** (avec `pickle`) pour passer d'un processus à l'autre.

## 2. La Classe `Process`

La classe `multiprocessing.Process` est l'équivalent de `threading.Thread`. Elle permet de lancer une fonction dans un nouveau processus.

```python
import multiprocessing
import time
import os

def info_processus(titre):
    print(titre)
    print('Nom du module:', __name__)
    # os.getppid() : ID du processus parent
    print('ID du processus parent:', os.getppid())
    # os.getpid() : ID du processus courant
    print('ID du processus:', os.getpid())
    print("-" * 20)

def worker_function(nom):
    info_processus(f'Fonction worker {nom}')
    print(f'Worker {nom}: Démarrage')
    time.sleep(2)
    print(f'Worker {nom}: Fin')

if __name__ == '__main__':
    info_processus('Fonction principale')

    # Crée un objet Process
    p = multiprocessing.Process(target=worker_function, args=('P1',))

    # Démarre le processus enfant
    p.start()

    print("Processus principal attend la fin du worker...")
    # Attend que le processus enfant se termine
    p.join()

    print("Processus worker terminé.")
```

### L'importance de `if __name__ == '__main__':`

Lorsque vous créez un nouveau processus, celui-ci importe votre script. Si le code de création du processus n'était pas protégé par ce bloc, chaque processus enfant essaierait de créer ses propres processus enfants, menant à une boucle infinie de création de processus.

Ce bloc garantit que le code de création des processus n'est exécuté que par le script principal, et non par les processus enfants importés. **C'est obligatoire sur Windows et macOS (avec le mode "spawn"), et une très bonne pratique sur Linux.**

## 3. Les `Pool` de Workers

Gérer manuellement des dizaines de processus peut être fastidieux. Le `multiprocessing.Pool` est une abstraction de haut niveau qui simplifie grandement la parallélisation de tâches.

Un `Pool` gère un ensemble de processus "workers". Vous lui soumettez des tâches, et il les distribue automatiquement aux workers disponibles.

```python
import multiprocessing
import time

def carre(x):
    """Calcule le carré d'un nombre après une petite pause."""
    time.sleep(0.1)
    return x * x

if __name__ == '__main__':
    nombres = range(10)

    start_time = time.time()

    # Crée un pool avec un nombre de workers.
    # Si aucun argument n'est donné, il utilise os.cpu_count().
    with multiprocessing.Pool(processes=4) as pool:
        # pool.map applique la fonction 'carre' à chaque élément de 'nombres'.
        # C'est une opération bloquante : elle attend que tous les résultats soient prêts.
        resultats = pool.map(carre, nombres)

    end_time = time.time()

    print(f"Résultats : {resultats}")
    print(f"Temps écoulé : {end_time - start_time:.2f} secondes")
```

**Analyse de l'exécution :**

- Le `Pool` a 4 workers.
- Il distribue les 10 tâches (`carre(0)`, `carre(1)`, ..., `carre(9)`) aux 4 workers.
- Chaque tâche prend 0.1s.
- Les 4 premiers nombres sont traités en parallèle (temps ~0.1s).
- Les 4 suivants sont traités en parallèle (temps total ~0.2s).
- Les 2 derniers sont traités en parallèle (temps total ~0.3s).
- Le temps total sera donc d'environ 0.3s, bien plus rapide que les 1.0s d'une exécution séquentielle.

### Méthodes utiles du `Pool`

- `pool.map(func, iterable)` : Bloquant. Applique `func` à chaque élément de `iterable`. Retourne une liste de résultats dans le même ordre.
- `pool.starmap(func, iterable_de_tuples)` : Similaire à `map`, mais pour les fonctions qui prennent plusieurs arguments. Chaque élément de l'itérable est un tuple d'arguments.
- `pool.apply(func, args)` : Bloquant. Appelle `func` avec les arguments `args`. Utile pour une seule tâche.
- `pool.apply_async(func, args)` : **Non-bloquant**. Soumet la tâche et retourne immédiatement un objet `AsyncResult`. On peut récupérer le résultat plus tard avec `result.get()`.
- `pool.map_async(func, iterable)` : Version non-bloquante de `map`.

### Exemple avec `apply_async`

`apply_async` est utile quand on veut lancer des tâches en arrière-plan et faire autre chose en attendant.

```python
import multiprocessing
import time

def worker(duree):
    print(f"Début d'une tâche de {duree}s")
    time.sleep(duree)
    return f"Tâche de {duree}s terminée"

if __name__ == '__main__':
    with multiprocessing.Pool(processes=2) as pool:
        # Soumet deux tâches. L'appel est non-bloquant.
        result1 = pool.apply_async(worker, (2,))
        result2 = pool.apply_async(worker, (3,))

        print("Le programme principal continue pendant que les workers travaillent...")

        # .get() est bloquant. Il attend que le résultat soit disponible.
        print(result1.get())
        print(result2.get())

    print("Toutes les tâches sont finies.")
```

## 4. Communication entre Processus

Puisque les processus ne partagent pas la mémoire, ils ont besoin de mécanismes spécifiques pour communiquer.

- **`multiprocessing.Queue`** : Une file d'attente thread-safe et process-safe. Les objets qui y sont placés sont sérialisés (pickled).
- **`multiprocessing.Pipe`** : Crée une paire de connexions bidirectionnelles. Plus rapide qu'une `Queue` mais ne peut être utilisé qu'entre deux processus.
- **Mémoire partagée (`Value`, `Array`)** : Pour partager des données simples (comme un nombre ou un tableau de nombres) sans avoir à les sérialiser. L'accès doit être protégé par des verrous (`Lock`).

### Exemple avec `Queue`

```python
import multiprocessing

def producteur(q):
    print("Le producteur envoie des données...")
    for i in range(5):
        q.put(f"item {i}")
    q.put(None) # Signal de fin

def consommateur(q):
    print("Le consommateur attend des données...")
    while True:
        item = q.get()
        if item is None: # Fin du travail
            break
        print(f"Consommé : {item}")

if __name__ == '__main__':
    # La Queue doit être créée avant les processus
    queue = multiprocessing.Queue()

    p_prod = multiprocessing.Process(target=producteur, args=(queue,))
    p_cons = multiprocessing.Process(target=consommateur, args=(queue,))

    p_prod.start()
    p_cons.start()

    p_prod.join()
    p_cons.join()
```

## Conclusion

Le module `multiprocessing` est l'outil indispensable en Python pour atteindre un vrai parallélisme et exploiter pleinement les processeurs multi-cœurs. Il est la solution de choix pour accélérer les applications `CPU-bound`. Bien que la communication entre processus soit plus complexe que pour les threads, des abstractions de haut niveau comme les `Pool` rendent la parallélisation de nombreuses tâches étonnamment simple. Pour des applications de calcul scientifique, d'analyse de données ou de traitement d'images, la maîtrise de `multiprocessing` est une compétence essentielle.
