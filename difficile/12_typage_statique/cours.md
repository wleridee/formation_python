# Module : Le Typage Statique avec `typing`

## Objectif

Ce module a pour but de vous introduire au typage statique en Python (aussi appelé "Type Hinting"). Vous apprendrez à annoter votre code avec des indications de type, à utiliser le module `typing` pour des types complexes, et à comprendre les avantages de cette pratique pour la robustesse et la lisibilité du code.

## 1. Qu'est-ce que le Typage Statique ?

Python est un langage à **typage dynamique**. Cela signifie que le type d'une variable est déterminé à l'exécution, et qu'une même variable peut contenir des objets de types différents au cours de la vie du programme.

```python
x = 10      # x est un int
x = "hello" # Maintenant, x est un str
```

Le **typage statique**, à l'inverse, est un système où les types des variables sont connus avant l'exécution (à la "compilation").

Python, depuis la version 3.5, permet d'ajouter des **indications de type (type hints)** optionnelles au code. C'est une forme de typage statique graduel.

**Important :** L'interpréteur Python, par défaut, **ignore complètement ces annotations**. Elles n'ont aucun impact sur l'exécution du code. Leur utilité vient d'outils externes appelés **analyseurs de type statique** (comme `mypy`, `pyright`, `pyre`) qui lisent ces annotations pour détecter des erreurs de type avant même que vous ne lanciez le programme.

## 2. Syntaxe de Base

### a. Annoter des Variables

On utilise les deux-points (`:`) après le nom de la variable, suivi du type.

```python
nom: str = "Alice"
age: int = 30
est_majeur: bool = True
prix: float = 19.99
```

### b. Annoter des Fonctions

- Pour les arguments : `nom_argument: type`
- Pour la valeur de retour : `-> type_retour`

```python
def saluer(nom: str) -> str:
    return f"Bonjour, {nom}"

def addition(a: int, b: int) -> int:
    return a + b

# Pour une fonction qui ne retourne rien, on utilise -> None
def afficher_message(message: str) -> None:
    print(message)
```

## 3. Le Module `typing`

Pour des types plus complexes que `int`, `str`, etc., on utilise le module `typing`.

### a. Types Composés (`List`, `Dict`, `Tuple`, `Set`)

Depuis Python 3.9+, on peut utiliser les types standards (`list`, `dict`) directement. Pour les versions antérieures, il faut importer `List`, `Dict`, etc., depuis `typing`.

```python
# Python 3.9+
nombres: list[int] = [1, 2, 3]
scores: dict[str, int] = {"Alice": 10, "Bob": 8}
coordonnees: tuple[int, float, int] = (10, 20.5, 5)

# Python < 3.9 (toujours valide)
from typing import List, Dict, Tuple, Set

nombres: List[int] = [1, 2, 3]
scores: Dict[str, int] = {"Alice": 10, "Bob": 8}
```

### b. `Union` et `Optional`

- `Union[type1, type2, ...]` : Indique qu'une variable peut être de l'un des types listés.
- `Optional[type]` : Un raccourci pour `Union[type, None]`. Indique qu'une variable peut être du type spécifié ou `None`.

```python
from typing import Union, Optional

# id peut être un int ou un str
identifiant: Union[int, str] = 123
identifiant = "user-abc"

# nom_utilisateur peut être une chaîne ou None
nom_utilisateur: Optional[str] = "Alice"
nom_utilisateur = None
```

### c. `Any`

`Any` est un type spécial qui indique à l'analyseur de type que n'importe quel type est autorisé. C'est une "porte de sortie" du système de typage. On l'utilise quand on ne peut vraiment pas connaître le type ou pour migrer progressivement un projet vers le typage statique.

```python
from typing import Any

def traiter_donnees_inconnues(data: Any) -> Any:
    # L'analyseur de type n'émettra aucune alerte ici
    print(data)
    return data
```

### d. `Callable`

Pour annoter des fonctions passées en argument. La syntaxe est `Callable[[type_arg1, type_arg2], type_retour]`.

```python
from typing import Callable

def appliquer_operation(a: int, b: int, operation: Callable[[int, int], int]) -> int:
    return operation(a, b)

def addition(x: int, y: int) -> int:
    return x + y

resultat = appliquer_operation(10, 5, addition) # OK
```

## 4. Utiliser un Analyseur de Type : `mypy`

`mypy` est l'analyseur de type statique le plus populaire pour Python.

1.  **Installation :**

    ```bash
    pip install mypy
    ```

2.  **Créer un fichier de test `test_types.py` :**

    ```python
    def saluer(nom: str) -> str:
        return f"Bonjour, {nom}"

    # Appel correct
    saluer("Alice")

    # Appel incorrect
    # mypy va détecter que 123 n'est pas un str
    saluer(123)
    ```

3.  **Lancer `mypy` :**

    ```bash
    mypy test_types.py
    ```

4.  **Résultat de `mypy` :**
    ```
    test_types.py:8: error: Argument 1 to "saluer" has incompatible type "int"; expected "str"
    Found 1 error in 1 file (checked 1 source file)
    ```
    `mypy` a trouvé l'erreur avant même l'exécution du code !

## 5. Avantages du Typage Statique

1.  **Détection précoce des bugs** : C'est l'avantage principal. Les erreurs de type sont parmi les plus courantes en programmation.
2.  **Amélioration de la lisibilité et de la documentation** : Les annotations de type servent de documentation. On sait immédiatement ce qu'une fonction attend comme arguments et ce qu'elle retourne.
3.  **Meilleur support des IDE** : Les éditeurs de code comme VS Code utilisent les annotations pour fournir une auto-complétion plus intelligente, des refactorings plus sûrs et une meilleure navigation dans le code.
4.  **Architecture plus robuste** : Le fait de penser aux types dès la conception pousse à créer des interfaces de données plus claires et une architecture plus solide.

## 6. Annotations pour les Classes Personnalisées

On peut utiliser les classes que l'on définit comme des types.

```python
class Personne:
    def __init__(self, nom: str, age: int):
        self.nom = nom
        self.age = age

def souhaiter_anniversaire(p: Personne) -> None:
    p.age += 1
    print(f"Joyeux anniversaire {p.nom} ! Vous avez maintenant {p.age} ans.")

# mypy vérifiera que l'objet passé à la fonction est bien une instance de Personne
# (ou une sous-classe compatible).
alice = Personne("Alice", 30)
souhaiter_anniversaire(alice)
```

### Références circulaires et `Forward References`

Parfois, une classe a besoin de s'annoter elle-même (ex: une méthode qui retourne une nouvelle instance de la classe), ou deux classes se référencent mutuellement. Si le type n'est pas encore défini au moment où Python lit l'annotation, cela cause une `NameError`.

La solution est d'utiliser une **"forward reference"**, en mettant le nom du type entre guillemets.

```python
class Node:
    def __init__(self, valeur: int):
        self.valeur = valeur
        # Le type 'Node' n'est pas encore complètement défini ici.
        # On utilise donc des guillemets.
        self.suivant: Optional['Node'] = None

    def set_suivant(self, autre_node: 'Node') -> None:
        self.suivant = autre_node
```

## Conclusion

Le typage statique est une addition majeure à l'écosystème Python qui comble de nombreuses lacunes du typage dynamique, en particulier pour les projets de grande taille. En ajoutant des annotations de type et en utilisant un analyseur comme `mypy`, vous rendez votre code plus sûr, plus lisible et plus facile à maintenir, sans perdre la flexibilité qui fait la force de Python. C'est aujourd'hui une pratique standard dans le développement Python professionnel.
