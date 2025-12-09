# Module : Propriétés (`@property`)

## 1. Quoi : Les Propriétés

En Python, une **propriété** (`property`) permet de transformer une méthode de classe en un "attribut managé". Elle vous permet d'exécuter du code (logique de validation, calculs, etc.) chaque fois qu'un attribut est lu, modifié ou supprimé, tout en conservant une syntaxe d'accès simple et directe comme pour un attribut normal.

Une propriété est créée à l'aide du décorateur `@property` et, optionnellement, des décorateurs `@<nom_propriete>.setter` et `@<nom_propriete>.deleter`.

## 2. Pourquoi : Contrôler l'accès aux attributs

L'accès direct aux attributs (`mon_objet.attribut = valeur`) est simple, mais il n'offre aucun contrôle. Que se passe-t-il si vous voulez :

- Valider une nouvelle valeur avant de l'assigner (ex: un email doit contenir un "@") ?
- Calculer une valeur à la volée au lieu de la stocker (ex: l'âge d'une personne calculé à partir de sa date de naissance) ?
- Déclencher une action lorsqu'un attribut est modifié ?

Les propriétés permettent de faire tout cela sans changer la manière dont l'utilisateur interagit avec votre objet. C'est une façon très "pythonic" de gérer l'encapsulation.

## 3. Comment : Getter, Setter, Deleter

### A. Le "Getter" (`@property`)

Le décorateur `@property` est utilisé pour définir la méthode "getter". Cette méthode est appelée chaque fois que l'attribut est lu.

**Exemple : Un attribut calculé**

Imaginons une classe `Rectangle` avec une largeur et une hauteur. Nous voulons pouvoir accéder à son aire comme si c'était un attribut, mais elle doit être calculée à chaque fois.

```python
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    @property
    def area(self):
        """Calculates and returns the area of the rectangle."""
        print("(Calculating area...)")
        return self.width * self.height

rect = Rectangle(10, 5)

# On accède à 'area' comme un attribut, mais c'est la méthode 'area' qui est appelée.
print(f"The area is: {rect.area}") # (Calculating area...) The area is: 50

# Si on change les dimensions, l'aire est recalculée à la prochaine lecture.
rect.width = 12
print(f"The new area is: {rect.area}") # (Calculating area...) The new area is: 60
```

Notez qu'on appelle `rect.area` et non `rect.area()`.

### B. Le "Setter" (`@<nom_propriete>.setter`)

Le décorateur "setter" est utilisé pour contrôler ce qui se passe lorsqu'on essaie d'assigner une valeur à la propriété.

**Exemple : Validation de données**

Imaginons une classe `Product` avec un prix. Nous voulons nous assurer que le prix ne peut jamais être négatif.

Pour cela, on utilise un attribut "privé" (par convention, préfixé d'un underscore `_`) pour stocker la valeur réelle, et une propriété publique pour y accéder.

```python
class Product:
    def __init__(self, name, price):
        self.name = name
        # On appelle le setter dès l'initialisation
        self.price = price

    @property
    def price(self):
        """Getter for the price."""
        return self._price

    @price.setter
    def price(self, value):
        """Setter for the price, with validation."""
        if value < 0:
            raise ValueError("Price cannot be negative.")
        # La valeur est stockée dans l'attribut "privé"
        self._price = value

# Création d'un produit
try:
    p = Product("Laptop", 1200)
    print(f"{p.name} costs ${p.price}")

    # Modification du prix (appelle le setter)
    p.price = 1300
    print(f"New price: ${p.price}")

    # Tentative de modification invalide (appelle le setter, qui lève une erreur)
    p.price = -50
except ValueError as e:
    print(f"Error: {e}")
```

### C. Le "Deleter" (`@<nom_propriete>.deleter`)

Le "deleter" est moins courant. Il est appelé lorsqu'on utilise l'instruction `del` sur la propriété.

```python
class MyClass:
    def __init__(self):
        self._my_attribute = "some value"

    @property
    def my_attribute(self):
        return self._my_attribute

    @my_attribute.setter
    def my_attribute(self, value):
        self._my_attribute = value

    @my_attribute.deleter
    def my_attribute(self):
        print("Deleting attribute...")
        self._my_attribute = None

obj = MyClass()
print(obj.my_attribute) # "some value"

del obj.my_attribute # Appelle le deleter
print(obj.my_attribute) # None
```

## 4. Propriétés en lecture seule

Si vous définissez un "getter" (`@property`) mais pas de "setter", la propriété devient en **lecture seule**. Toute tentative d'assignation lèvera une `AttributeError`.

```python
class Circle:
    def __init__(self, radius):
        self.radius = radius

    @property
    def diameter(self):
        """Diameter is a read-only calculated property."""
        return self.radius * 2

c = Circle(10)
print(f"Radius: {c.radius}")
print(f"Diameter: {c.diameter}") # OK

# c.diameter = 30 # ❌ Lèvera une AttributeError: can't set attribute
```

C'est une excellente manière de protéger des attributs qui ne devraient pas être modifiés de l'extérieur.
