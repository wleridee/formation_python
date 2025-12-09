# Module : Modules et Packages

## Objectif

Ce module a pour but de vous apprendre à structurer votre code Python en modules et packages. Vous découvrirez comment importer du code depuis d'autres fichiers, comment organiser un projet en plusieurs dossiers, et les bonnes pratiques pour créer des applications maintenables.

## 1. Qu'est-ce qu'un Module ?

Un **module** est tout simplement un fichier Python (`.py`). L'intérêt des modules est de pouvoir diviser un programme complexe en plusieurs fichiers logiques, ce qui améliore l'organisation et la réutilisabilité du code.

Chaque module a sa propre portée globale. Les variables, fonctions et classes définies dans un module ne sont pas directement accessibles depuis un autre. Pour les utiliser, il faut les **importer**.

### Exemple

Imaginons la structure de fichiers suivante :

```
projet/
├── main.py
└── mon_module.py
```

**Fichier `mon_module.py` :**

```python
print("Le module 'mon_module' est en cours d'importation.")

PI = 3.14159

def saluer(nom):
    return f"Bonjour, {nom} !"

class MaClasse:
    def __init__(self):
        print("Instance de MaClasse créée.")
```

**Fichier `main.py` :**

```python
# Importe le module entier
import mon_module

print(f"La valeur de PI est {mon_module.PI}")

message = mon_module.saluer("Monde")
print(message)

instance = mon_module.MaClasse()
```

**Exécution de `main.py` :**

```
Le module 'mon_module' est en cours d'importation.
La valeur de PI est 3.14159
Bonjour, Monde !
Instance de MaClasse créée.
```

Le code dans `mon_module.py` est exécuté **une seule fois**, lors du premier import.

## 2. Les Différentes Syntaxes d'Importation

### a. `import <module>`

Importe le module entier. Pour accéder à ses membres, on doit utiliser la notation `module.membre`. C'est la méthode la plus claire et la plus recommandée car elle évite les conflits de noms.

```python
import math
print(math.sqrt(16))
```

### b. `from <module> import <membre>`

Importe un membre spécifique (fonction, classe, variable) directement dans la portée actuelle.

```python
from math import sqrt, pi

# On peut maintenant utiliser sqrt et pi directement
print(sqrt(16))
print(pi)
```

### c. `from <module> import <membre> as <alias>`

Importe un membre et lui donne un alias (un autre nom). C'est utile pour éviter les conflits de noms ou pour raccourcir un nom long.

```python
from math import sqrt as racine_carree
print(racine_carree(16))
```

On peut aussi donner un alias à un module entier :

```python
import mon_module as mm
print(mm.PI)
```

### d. `from <module> import *` (À ÉVITER)

Importe **tous** les noms publics d'un module dans la portée actuelle. Cette pratique est fortement déconseillée car :

- Elle pollue la portée actuelle avec des noms potentiellement inconnus.
- Elle rend le code très difficile à lire (on ne sait pas d'où vient une fonction).
- Elle peut masquer des variables ou fonctions existantes.

## 3. Qu'est-ce qu'un Package ?

Lorsque votre projet grandit, vous pouvez avoir besoin de plus de structure qu'une simple liste de fichiers. Un **package** est un dossier qui contient d'autres modules (et potentiellement d'autres packages).

Pour que Python reconnaisse un dossier comme un package, il doit contenir un fichier spécial nommé `__init__.py`. Ce fichier peut être vide.

### Exemple de structure de package

```
shop/
├── __init__.py
├── main.py
├── products/
│   ├── __init__.py
│   └── database.py
└── users/
    ├── __init__.py
    └── auth.py
```

Dans cet exemple, `shop`, `products` et `users` sont des packages.

### Importer depuis des packages

Pour importer le module `database` depuis `main.py`, on utilise la notation avec des points :

**Fichier `main.py` :**

```python
# Importation absolue
from products.database import find_product
from users.auth import login

produit = find_product(101)
user = login("test", "password")
```

### Le fichier `__init__.py`

Ce fichier a deux rôles principaux :

1.  **Marquer un dossier comme un package Python.**
2.  **Exécuter du code d'initialisation pour le package.** On peut y définir des variables pour le package ou importer automatiquement certains modules.

Par exemple, pour simplifier les imports, on peut modifier `products/__init__.py` :

**Fichier `products/__init__.py` :**

```python
# Rend la fonction find_product directement accessible depuis le package products
from .database import find_product
```

Maintenant, depuis `main.py`, l'import devient plus court :

```python
# L'import est plus simple grâce à __init__.py
from products import find_product
```

## 4. Imports Absolus vs. Relatifs

- **Import absolu** : Spécifie le chemin depuis la racine du projet. C'est la méthode la plus claire et la plus recommandée.

  ```python
  from products.database import find_product
  ```

- **Import relatif** : Spécifie le chemin par rapport au module actuel. Utile à l'intérieur d'un package pour se référer à des modules frères.

  - `.` : Le package actuel.
  - `..` : Le package parent.

  Imaginons que `users/auth.py` ait besoin d'une fonction de `products/database.py`. Ce n'est pas une bonne conception, mais pour l'exemple :

  ```python
  # Dans users/auth.py
  from ..products.database import find_product # Remonte d'un niveau (..) puis descend dans products
  ```

  Les imports relatifs peuvent rendre la structure du projet plus difficile à comprendre et sont à utiliser avec parcimonie.

## 5. Le Piège du `if __name__ == '__main__':`

Quand un fichier Python est exécuté directement, Python lui assigne la valeur spéciale `"__main__"` à sa variable globale `__name__`.
Quand un fichier est importé comme un module, `__name__` prend la valeur du nom du module (le nom du fichier).

Cela permet d'écrire du code qui ne s'exécute que lorsque le fichier est le script principal.

**Fichier `mon_module.py` :**

```python
def ma_fonction():
    print("Fonction du module.")

print(f"La valeur de __name__ dans mon_module.py est : {__name__}")

# Ce bloc ne s'exécutera que si on lance 'python mon_module.py'
if __name__ == '__main__':
    print("Ce code s'exécute car le module est le script principal.")
    ma_fonction()
```

**Fichier `main.py` :**

```python
import mon_module

print("Appel depuis main.py")
mon_module.ma_fonction()
```

**Exécution de `main.py` :**

```
La valeur de __name__ dans mon_module.py est : mon_module
Appel depuis main.py
Fonction du module.
```

Le code dans le `if` de `mon_module.py` n'est pas exécuté. C'est l'endroit idéal pour mettre des tests, des exemples d'utilisation ou le point de départ d'une application.

## Conclusion

Les modules et les packages sont les outils fondamentaux pour organiser des projets Python de toute taille. En divisant votre code en unités logiques, vous améliorez sa lisibilité, sa maintenabilité et sa réutilisabilité. La maîtrise des différentes syntaxes d'import et du rôle du fichier `__init__.py` est essentielle pour construire des applications robustes et bien structurées.
