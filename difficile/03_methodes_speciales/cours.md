# Module : Méthodes Spéciales (Dunder Methods)

## 1. Quoi : Les Méthodes Spéciales

Les **méthodes spéciales**, aussi appelées "méthodes magiques" ou "dunder methods" (pour _Double Underscore_), sont des méthodes prédéfinies en Python que vous pouvez ajouter à vos classes pour qu'elles s'intègrent avec les fonctionnalités natives du langage.

Leur nom est toujours entouré de doubles underscores (ex: `__init__`, `__str__`, `__len__`). Vous en connaissez déjà une : `__init__`, le constructeur.

En implémentant ces méthodes, vous permettez à vos objets de se comporter comme des types de données intégrés, en répondant à des opérateurs (`+`, `==`), des fonctions (`len()`, `str()`), et d'autres syntaxes.

## 2. Pourquoi : Rendre les objets plus "pythonic"

L'utilisation des méthodes spéciales rend vos objets plus intuitifs et plus faciles à utiliser.

- `print(my_object)` peut afficher une description lisible au lieu de `<__main__.MyClass object at 0x...>`.
- `len(my_collection)` peut retourner le nombre d'éléments de votre objet collection personnalisé.
- `obj1 + obj2` peut effectuer une addition logique entre deux de vos objets.
- `if my_object:` peut évaluer la "vérité" de votre objet.

## 3. Comment : Les Méthodes Spéciales les plus courantes

### A. Représentation d'objet : `__str__` et `__repr__`

- `__str__(self)` : Doit retourner une chaîne de caractères **lisible par l'humain**. C'est ce qui est appelé par `print(obj)` et `str(obj)`. Le but est l'affichage.
- `__repr__(self)` : Doit retourner une représentation **non ambiguë** de l'objet, idéalement une chaîne qui pourrait être utilisée pour recréer l'objet (ex: `MyClass(arg1=value1)`). C'est ce qui est affiché dans la console interactive si vous tapez juste le nom de l'objet. C'est aussi le fallback si `__str__` n'est pas défini.

✅ **Bonne pratique** : Implémentez toujours `__repr__`. Implémentez `__str__` si vous voulez une version plus "jolie" pour l'affichage.

```python
class Book:
    def __init__(self, title, author):
        self.title = title
        self.author = author

    def __str__(self):
        return f'"{self.title}" by {self.author}'

    def __repr__(self):
        return f"Book(title='{self.title}', author='{self.author}')"

book = Book("The Hobbit", "J.R.R. Tolkien")

print(book)         # Appelle __str__ -> "The Hobbit" by J.R.R. Tolkien
print(str(book))    # Appelle __str__
print(repr(book))   # Appelle __repr__ -> Book(title='The Hobbit', author='J.R.R. Tolkien')
# Dans une console interactive, taper `book` appellerait __repr__
```

### B. Comportement de collection : `__len__` et `__getitem__`

- `__len__(self)` : Doit retourner la "longueur" de l'objet, un entier. Permet d'utiliser la fonction `len(obj)`.
- `__getitem__(self, key)` : Permet d'accéder à un élément via un index ou une clé, comme pour les listes ou les dictionnaires (`obj[key]`).

```python
class Deck:
    def __init__(self):
        ranks = [str(n) for n in range(2, 11)] + list('JQKA')
        suits = '♠♡♢♣'
        self.cards = [f"{r}{s}" for s in suits for r in ranks]

    def __len__(self):
        return len(self.cards)

    def __getitem__(self, position):
        return self.cards[position]

deck = Deck()
print(len(deck)) # Appelle __len__ -> 52
print(deck[0])   # Appelle __getitem__(0) -> 2♠
print(deck[-1])  # Appelle __getitem__(-1) -> A♣
```

### C. Opérateurs de comparaison : `__eq__`

- `__eq__(self, other)` : Définit le comportement de l'opérateur d'égalité `==`. Doit retourner `True` ou `False`.

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __eq__(self, other):
        # Deux points sont égaux si leurs coordonnées x et y sont égales
        if not isinstance(other, Point):
            return NotImplemented
        return self.x == other.x and self.y == other.y

p1 = Point(1, 2)
p2 = Point(1, 2)
p3 = Point(3, 4)

print(p1 == p2) # Appelle __eq__ -> True
print(p1 == p3) # Appelle __eq__ -> False
print(p1 == (1, 2)) # False (grâce à isinstance)
```

D'autres opérateurs existent : `__ne__` (!=), `__lt__` (<), `__gt__` (>), etc.

### D. Opérateurs arithmétiques : `__add__`

- `__add__(self, other)` : Définit le comportement de l'opérateur d'addition `+`.

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

    def __add__(self, other):
        # L'addition de deux vecteurs est l'addition de leurs composantes
        if not isinstance(other, Vector):
            return NotImplemented
        return Vector(self.x + other.x, self.y + other.y)

v1 = Vector(2, 4)
v2 = Vector(1, 3)
v3 = v1 + v2 # Appelle __add__

print(v3) # Vector(3, 7)
```

D'autres opérateurs existent : `__sub__` (-), `__mul__` (\*), etc.

### E. Valeur booléenne : `__bool__`

- `__bool__(self)` : Définit la valeur de vérité de l'objet lorsqu'il est utilisé dans un contexte booléen (`if obj:`). Doit retourner `True` ou `False`. Si non défini, Python utilise `__len__` (un objet de longueur 0 est `False`).

```python
class ShoppingCart:
    def __init__(self):
        self.items = []

    def __bool__(self):
        # Le panier est "vrai" s'il n'est pas vide
        return len(self.items) > 0

cart = ShoppingCart()
if not cart: # Appelle __bool__
    print("Your cart is empty.") # S'affiche

cart.items.append("apple")
if cart: # Appelle __bool__
    print("You have items in your cart.") # S'affiche
```
