# Exercice 01 : Création d'une Classe `Vector`

## Objectif

Cet exercice a pour but de vous faire implémenter plusieurs méthodes spéciales pour créer une classe `Vector` (vecteur 2D) qui se comporte de manière intuitive avec les opérateurs Python natifs.

## Contexte

Vous allez créer une classe `Vector` qui représente un vecteur dans un plan 2D, avec des composantes `x` et `y`. Vous la rendrez plus "pythonic" en implémentant des méthodes spéciales pour :

- L'afficher de manière lisible (`__str__` et `__repr__`).
- L'additionner avec un autre vecteur (`__add__`).
- Tester son égalité avec un autre vecteur (`__eq__`).

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `vector_class.py`.

2.  **Définissez la classe `Vector`**.

    - Son constructeur `__init__` doit accepter `x` et `y` et les stocker comme attributs.

3.  **Implémentez `__repr__(self)`** :

    - Cette méthode doit retourner une chaîne de caractères qui pourrait être utilisée pour recréer l'objet.
    - _Exemple de retour :_ `Vector(x=3, y=4)`

4.  **Implémentez `__str__(self)`** :

    - Cette méthode doit retourner une représentation plus simple et lisible par un humain.
    - _Exemple de retour :_ `(3, 4)`

5.  **Implémentez `__eq__(self, other)`** :

    - Cette méthode définit le comportement de l'opérateur `==`.
    - Elle doit retourner `True` si `other` est aussi une instance de `Vector` ET que leurs composantes `x` et `y` sont égales. Sinon, elle retourne `False`.

6.  **Implémentez `__add__(self, other)`** :

    - Cette méthode définit le comportement de l'opérateur `+`.
    - L'addition de deux vecteurs `v1(x1, y1)` et `v2(x2, y2)` est un nouveau vecteur `v3(x1+x2, y1+y2)`.
    - La méthode doit vérifier si `other` est bien une instance de `Vector`. Si ce n'est pas le cas, elle doit retourner `NotImplemented` (une constante spéciale qui indique à Python que l'opération n'est pas supportée entre ces types).
    - Si `other` est un `Vector`, la méthode doit retourner une **nouvelle instance** de `Vector` représentant la somme.

7.  **Testez votre classe** :
    - Créez deux vecteurs `v1 = Vector(2, 3)` et `v2 = Vector(5, 1)`.
    - Testez `print()`, `str()`, et `repr()`.
    - Testez l'addition `v3 = v1 + v2` et affichez `v3`.
    - Testez l'égalité `v1 == v2` et `v1 == Vector(2, 3)`.

## Résultat Attendu

```
v1 string representation: (2, 3)
v1 official representation: Vector(x=2, y=3)

v3 = v1 + v2
v3 string representation: (7, 4)

v1 == v2: False
v1 == Vector(2, 3): True
v1 == (2, 3): False
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# vector_class.py

class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __repr__(self):
        """Official representation, good for debugging."""
        return f"Vector(x={self.x}, y={self.y})"

    def __str__(self):
        """User-friendly string representation."""
        return f"({self.x}, {self.y})"

    def __eq__(self, other):
        """Equality comparison (==)."""
        if not isinstance(other, Vector):
            return False
        return self.x == other.x and self.y == other.y

    def __add__(self, other):
        """Addition (+)."""
        if not isinstance(other, Vector):
            return NotImplemented
        # Return a new Vector instance
        return Vector(self.x + other.x, self.y + other.y)

# --- Testing ---
v1 = Vector(2, 3)
v2 = Vector(5, 1)

# Test __str__ and __repr__
print(f"v1 string representation: {str(v1)}")
print(f"v1 official representation: {repr(v1)}")
print("-" * 20)

# Test __add__
print("v3 = v1 + v2")
v3 = v1 + v2
print(f"v3 string representation: {v3}")
print("-" * 20)

# Test __eq__
print(f"v1 == v2: {v1 == v2}")
print(f"v1 == Vector(2, 3): {v1 == Vector(2, 3)}")
print(f"v1 == (2, 3): {v1 == (2, 3)}") # Should be False thanks to isinstance check

```

</details>
