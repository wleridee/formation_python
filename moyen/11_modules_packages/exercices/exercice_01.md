# Exercice 01 : Création et Utilisation d'un Package Simple

## Objectif

Cet exercice a pour but de vous faire créer une structure de package simple, d'y définir des modules avec des fonctions, et de les utiliser depuis un script principal. Cela vous permettra de mettre en pratique la création de packages, les imports absolus et le rôle du fichier `__init__.py`.

## Contexte

Vous allez créer un petit package de "calculs" qui sera structuré en deux sous-packages : `operations` pour les calculs arithmétiques de base et `scientifique` pour des calculs plus avancés.

## Énoncé

### Étape 1 : Créer la structure du projet

Créez l'arborescence de fichiers et de dossiers suivante :

```
mon_projet/
├── main.py
└── calculs/
    ├── __init__.py
    ├── operations/
    │   ├── __init__.py
    │   └── arithmetique.py
    └── scientifique/
        ├── __init__.py
        └── stats.py
```

Tous les fichiers `__init__.py` peuvent être vides au début.

### Étape 2 : Remplir les modules

1.  **Dans `calculs/operations/arithmetique.py` :**
    Définissez deux fonctions simples :

    - `addition(a, b)` qui retourne `a + b`.
    - `soustraction(a, b)` qui retourne `a - b`.

2.  **Dans `calculs/scientifique/stats.py` :**
    Définissez une fonction :
    - `moyenne(nombres)` qui prend une liste de nombres et retourne leur moyenne.

### Étape 3 : Utiliser les modules dans `main.py`

Ouvrez `main.py` et, en utilisant des **imports absolus**, importez et utilisez les fonctions que vous venez de créer.

```python
# Dans main.py

# Importer les fonctions nécessaires
from calculs.operations.arithmetique import addition, soustraction
from calculs.scientifique.stats import moyenne

# Utiliser les fonctions
somme = addition(10, 5)
difference = soustraction(10, 5)
moy = moyenne([10, 20, 30, 40])

print(f"10 + 5 = {somme}")
print(f"10 - 5 = {difference}")
print(f"La moyenne est : {moy}")
```

À ce stade, exécutez `main.py` et assurez-vous que tout fonctionne correctement.

### Étape 4 : Simplifier les imports avec `__init__.py`

Votre objectif est de rendre les imports plus courts et plus pratiques. Vous voulez pouvoir importer les fonctions directement depuis `calculs.operations` et `calculs.scientifique`.

1.  **Modifiez `calculs/operations/__init__.py` :**

    - Ajoutez la ligne suivante pour "promouvoir" les fonctions du module `arithmetique` au niveau du package `operations` :

    ```python
    from .arithmetique import addition, soustraction
    ```

    (Le `.` indique un import relatif depuis le même package).

2.  **Modifiez `calculs/scientifique/__init__.py` :**
    - Faites de même pour la fonction `moyenne` :
    ```python
    from .stats import moyenne
    ```

### Étape 5 : Mettre à jour `main.py`

Maintenant que les `__init__.py` sont configurés, modifiez `main.py` pour utiliser les imports simplifiés.

```python
# Dans main.py (version mise à jour)

# Les imports sont maintenant plus courts
from calculs.operations import addition, soustraction
from calculs.scientifique import moyenne

# Le reste du code ne change pas
somme = addition(10, 5)
difference = soustraction(10, 5)
moy = moyenne([10, 20, 30, 40])

print(f"10 + 5 = {somme}")
print(f"10 - 5 = {difference}")
print(f"La moyenne est : {moy}")
```

Exécutez à nouveau `main.py` pour confirmer que tout fonctionne toujours comme prévu.

## Résultat Attendu

L'exécution de `main.py` (dans sa version finale) doit produire la sortie suivante sans erreur :

```
10 + 5 = 15
10 - 5 = 5
La moyenne est : 25.0
```

Cet exercice démontre comment les packages et les fichiers `__init__.py` permettent de créer une structure de projet propre et de fournir une API simple et accessible pour les utilisateurs de votre package.

<details>
<summary>Cliquez ici pour voir un exemple de la structure finale des fichiers</summary>

**`calculs/operations/arithmetique.py`**

```python
def addition(a, b):
    return a + b

def soustraction(a, b):
    return a - b
```

**`calculs/scientifique/stats.py`**

```python
def moyenne(nombres):
    if not nombres:
        return 0
    return sum(nombres) / len(nombres)
```

**`calculs/operations/__init__.py`**

```python
from .arithmetique import addition, soustraction
```

**`calculs/scientifique/__init__.py`**

```python
from .stats import moyenne
```

**`main.py`**

```python
from calculs.operations import addition, soustraction
from calculs.scientifique import moyenne

somme = addition(10, 5)
difference = soustraction(10, 5)
moy = moyenne([10, 20, 30, 40])

print(f"10 + 5 = {somme}")
print(f"10 - 5 = {difference}")
print(f"La moyenne est : {moy}")
```

Les autres fichiers `__init__.py` peuvent rester vides.

</details>
