# Exercice 01 : Duck Typing avec des Formes Géométriques

## Objectif

Cet exercice a pour but de vous faire mettre en pratique le concept de Duck Typing en créant plusieurs classes de formes géométriques qui, bien que n'ayant aucun lien d'héritage, peuvent toutes être utilisées par une même fonction grâce à leur interface commune.

## Contexte

Imaginez que vous travaillez sur un programme de dessin. Vous avez besoin de calculer l'aire de différentes formes (carrés, cercles, etc.). Au lieu de forcer toutes vos formes à hériter d'une classe `Forme` de base, vous allez utiliser le Duck Typing.

Vous allez définir plusieurs classes de formes, chacune avec sa propre méthode `calculer_aire()`. Ensuite, vous écrirez une fonction qui peut prendre n'importe laquelle de ces formes et afficher son aire.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `formes.py`.

2.  **Définissez la classe `Carre`**.

    - Le constructeur `__init__` doit accepter un argument `cote`.
    - Définissez une méthode `calculer_aire()` qui retourne l'aire du carré (`cote * cote`).

3.  **Définissez la classe `Cercle`**.

    - Le constructeur `__init__` doit accepter un argument `rayon`.
    - Définissez une méthode `calculer_aire()` qui retourne l'aire du cercle (`pi * rayon²`). Vous pouvez utiliser `math.pi` pour la valeur de pi.

4.  **Définissez la classe `Rectangle`**.

    - Le constructeur `__init__` doit accepter deux arguments, `longueur` et `largeur`.
    - Définissez une méthode `calculer_aire()` qui retourne l'aire du rectangle (`longueur * largeur`).

5.  **Définissez une classe qui n'est PAS une forme**.

    - Créez une classe `Bateau` avec un constructeur qui prend un `nom`.
    - Cette classe ne doit **pas** avoir de méthode `calculer_aire()`.

6.  **Créez une fonction `afficher_aire(forme)`**.

    - Cette fonction doit prendre un objet `forme` en argument.
    - Elle doit essayer d'appeler la méthode `calculer_aire()` de l'objet.
    - Pour gérer les objets qui n'ont pas cette méthode (comme `Bateau`), utilisez un bloc `try...except AttributeError` ou la fonction `hasattr()`.
    - Si l'objet a la méthode, la fonction doit afficher un message comme `f"L'aire de la forme est : {aire}"`.
    - Si l'objet n'a pas la méthode, elle doit afficher un message comme `f"L'objet {type(forme).__name__} n'a pas de méthode pour calculer l'aire."`.

7.  **Testez votre code**.
    - Créez une liste contenant des instances de `Carre`, `Cercle`, `Rectangle`, et `Bateau`.
    - Itérez sur cette liste et appelez `afficher_aire()` pour chaque objet.

## Résultat Attendu

Votre script doit démontrer que la fonction `afficher_aire` peut interagir avec n'importe quelle forme qui "cancane" comme une forme (c'est-à-dire, qui a une méthode `calculer_aire`), et qu'elle gère gracieusement les objets qui ne le font pas.

```
L'aire de la forme est : 25
L'aire de la forme est : 78.53981633974483
L'aire de la forme est : 24
L'objet Bateau n'a pas de méthode pour calculer l'aire.
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# formes.py

import math

# --- Définition des classes de formes ---

class Carre:
    def __init__(self, cote: float):
        self.cote = cote

    def calculer_aire(self) -> float:
        """Calcule l'aire du carré."""
        return self.cote * self.cote

class Cercle:
    def __init__(self, rayon: float):
        self.rayon = rayon

    def calculer_aire(self) -> float:
        """Calcule l'aire du cercle."""
        return math.pi * (self.rayon ** 2)

class Rectangle:
    def __init__(self, longueur: float, largeur: float):
        self.longueur = longueur
        self.largeur = largeur

    def calculer_aire(self) -> float:
        """Calcule l'aire du rectangle."""
        return self.longueur * self.largeur

# --- Classe qui ne respecte pas l'interface ---

class Bateau:
    def __init__(self, nom: str):
        self.nom = nom

# --- Fonction polymorphique utilisant le Duck Typing ---

def afficher_aire(forme):
    """
    Affiche l'aire d'un objet s'il possède une méthode calculer_aire().
    """
    # On vérifie si l'objet a le comportement attendu (la méthode)
    if hasattr(forme, 'calculer_aire') and callable(forme.calculer_aire):
        aire = forme.calculer_aire()
        print(f"L'aire de la forme ({type(forme).__name__}) est : {aire}")
    else:
        print(f"L'objet {type(forme).__name__} n'a pas de méthode pour calculer l'aire.")

# --- Tests ---
if __name__ == "__main__":
    # Création d'objets de types différents.
    # Ils ne partagent aucun lien d'héritage.
    mon_carre = Carre(cote=5)
    mon_cercle = Cercle(rayon=5)
    mon_rectangle = Rectangle(longueur=6, largeur=4)
    mon_bateau = Bateau(nom="Titanic")

    # Création d'une liste d'objets hétérogènes
    objets = [mon_carre, mon_cercle, mon_rectangle, mon_bateau]

    # La fonction afficher_aire fonctionne avec tous les objets
    # qui ont la bonne "interface" (la méthode calculer_aire).
    for obj in objets:
        afficher_aire(obj)

```

</details>
