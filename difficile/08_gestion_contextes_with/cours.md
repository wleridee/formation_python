# Module : Gestionnaires de Contexte et l'instruction `with`

## 1. Quoi : L'instruction `with`

L'instruction `with` en Python est utilisée pour envelopper l'exécution d'un bloc de code. Elle simplifie la gestion des ressources (comme les fichiers, les connexions réseau ou les verrous de thread) en garantissant que les opérations de "nettoyage" sont effectuées, même si des erreurs se produisent.

Vous l'avez probablement déjà rencontrée pour la manipulation de fichiers :

```python
with open('my_file.txt', 'w') as f:
    f.write('Hello, World!')
# À la sortie de ce bloc, le fichier 'f' est automatiquement fermé.
```

Cette syntaxe est plus propre et plus sûre que la gestion manuelle avec un bloc `try...finally`.

## 2. Pourquoi : Une gestion de ressources robuste

Le principal avantage de `with` est de garantir que les ressources sont correctement libérées.

- **Fermeture automatique** : Les fichiers sont fermés, les connexions réseau sont terminées, les verrous sont libérés.
- **Gestion des erreurs** : Le nettoyage a lieu même si une exception est levée à l'intérieur du bloc `with`.
- **Code plus lisible** : La logique d'acquisition et de libération de la ressource est encapsulée, rendant le code principal plus clair.

## 3. Comment : Le Protocole de Gestion de Contexte

Pour qu'un objet puisse être utilisé avec l'instruction `with`, il doit implémenter le **protocole de gestion de contexte**, qui consiste en deux méthodes spéciales :

- `__enter__(self)` : Cette méthode est appelée au début du bloc `with`. Sa valeur de retour est généralement assignée à la variable après le `as` (mais c'est optionnel).
- `__exit__(self, exc_type, exc_val, exc_tb)` : Cette méthode est appelée à la fin du bloc `with`, que ce soit normalement ou à cause d'une exception.
  - Si le bloc se termine sans erreur, les trois arguments `exc_*` seront `None`.
  - Si une exception se produit, `exc_type`, `exc_val`, et `exc_tb` contiendront les informations sur l'exception.
  - Si la méthode `__exit__` retourne `True`, l'exception est "supprimée" (elle ne sera pas propagée). Si elle retourne `False` (ou `None`), l'exception continue sa propagation après le nettoyage.

### A. Créer un gestionnaire de contexte avec une classe

**Exemple : Un minuteur simple**

Créons un gestionnaire de contexte qui mesure le temps passé à l'intérieur du bloc `with`.

```python
import time

class Timer:
    def __init__(self, description):
        self.description = description

    def __enter__(self):
        print(f"Starting '{self.description}'...")
        self.start_time = time.time()
        # On ne retourne rien, donc pas de 'as' nécessaire
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        end_time = time.time()
        elapsed = end_time - self.start_time
        print(f"'{self.description}' finished in {elapsed:.4f} seconds.")
        # On ne gère pas les exceptions, donc on retourne None (implicitement)

# Utilisation
with Timer("Long operation"):
    # Simule un travail
    time.sleep(2)

# Affiche:
# Starting 'Long operation'...
# 'Long operation' finished in 2.00xx seconds.
```

### B. Créer un gestionnaire de contexte avec `contextlib`

Écrire une classe complète peut être verbeux. Le module `contextlib` de la bibliothèque standard fournit un décorateur, `@contextmanager`, qui permet de créer un gestionnaire de contexte à partir d'une simple fonction génératrice.

- Le code **avant** l'instruction `yield` correspond à `__enter__`.
- L'instruction `yield` produit la valeur qui sera assignée à la variable `as`.
- Le code **après** l'instruction `yield` correspond à `__exit__`.

**Exemple : Le minuteur réécrit avec `@contextmanager`**

```python
import time
from contextlib import contextmanager

@contextmanager
def timer(description):
    print(f"Starting '{description}'...")
    start_time = time.time()
    try:
        # Le contrôle est passé au bloc 'with' ici
        yield
    finally:
        # Ce code est exécuté à la sortie du bloc 'with'
        end_time = time.time()
        elapsed = end_time - start_time
        print(f"'{description}' finished in {elapsed:.4f} seconds.")

# Utilisation
with timer("Another long operation"):
    time.sleep(1.5)
```

Le `try...finally` est crucial pour garantir que le code de nettoyage s'exécute même en cas d'exception dans le bloc `with`. Cette version est souvent considérée comme plus concise et plus lisible.

## 4. Cas d'usage

- **Gestion de fichiers** : Le cas le plus courant.
- **Connexions à des bases de données** : `__enter__` ouvre la connexion et démarre une transaction, `__exit__` la ferme ou la valide/annule.
- **Verrous de concurrence (`threading.Lock`)** : `with my_lock:` acquiert le verrou au début et le libère à la fin, quoi qu'il arrive.
- **Changement temporaire de contexte** : Changer temporairement un paramètre de configuration et le restaurer à sa valeur d'origine à la fin.
- **Suppression d'exceptions**.

L'instruction `with` est un outil puissant pour écrire du code Python plus sûr et plus déclaratif.
