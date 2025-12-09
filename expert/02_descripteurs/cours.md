# Module : Les Descripteurs en Python

## Objectif

Ce module a pour but de vous faire découvrir le protocole des descripteurs en Python. Vous apprendrez ce qu'est un descripteur, comment il fonctionne, et comment l'utiliser pour créer des attributs managés, valider des données et comprendre le fonctionnement interne de Python pour des fonctionnalités comme `@property` et les méthodes.

## 1. Qu'est-ce qu'un Descripteur ?

Un **descripteur** est un objet qui personnalise l'accès à un attribut dans une autre classe. Si un objet définit au moins une des méthodes `__get__`, `__set__`, ou `__delete__`, il est considéré comme un descripteur.

Le protocole descripteur est composé de ces trois méthodes :

- `__get__(self, instance, owner)` : Appelée pour récupérer la valeur de l'attribut.
  - `self`: L'instance du descripteur lui-même.
  - `instance`: L'instance de la classe qui possède l'attribut (ex: `mon_objet`).
  - `owner`: La classe qui possède l'attribut (ex: `MaClasse`).
- `__set__(self, instance, value)` : Appelée pour définir la valeur de l'attribut.
  - `value`: La valeur à assigner.
- `__delete__(self, instance)` : Appelée pour supprimer l'attribut.

Il existe deux types de descripteurs :

1.  **Descripteur de données (Data Descriptor)** : Définit à la fois `__get__` et `__set__`. Il a la priorité sur l'attribut du dictionnaire de l'instance.
2.  **Descripteur non-données (Non-Data Descriptor)** : Définit seulement `__get__`. L'attribut du dictionnaire de l'instance a la priorité.

## 2. Créer un Descripteur Simple

Créons un descripteur simple qui journalise chaque accès à un attribut.

```python
class LoggingDescriptor:
    def __get__(self, instance, owner):
        print(f"Accès (get) à l'attribut depuis l'instance {instance}")
        # Pour l'instant, on ne stocke pas de valeur
        return "valeur du descripteur"

    def __set__(self, instance, value):
        print(f"Accès (set) à l'attribut sur {instance} avec la valeur '{value}'")
        # On ne fait rien avec la valeur pour le moment

class MaClasse:
    # On assigne une instance du descripteur à un attribut de classe
    attribut_log = LoggingDescriptor()

# --- Tests ---
instance = MaClasse()

# Accès en lecture -> appelle __get__
val = instance.attribut_log
# Output: Accès (get) à l'attribut depuis l'instance <__main__.MaClasse object at ...>

# Accès en écriture -> appelle __set__
instance.attribut_log = "nouvelle valeur"
# Output: Accès (set) à l'attribut sur <__main__.MaClasse object at ...> avec la valeur 'nouvelle valeur'
```

Ce code ne stocke pas encore de valeur. L'attribut `attribut_log` est partagé par toutes les instances de `MaClasse`.

## 3. Stocker des Données par Instance

Le problème principal avec les descripteurs est de savoir où stocker la valeur pour chaque instance. Si on la stocke dans le descripteur lui-même (`self.valeur = value`), elle sera partagée par toutes les instances.

La solution la plus courante est d'utiliser le dictionnaire de l'instance (`instance.__dict__`) pour stocker la valeur. Pour éviter les conflits de noms, on utilise souvent un nom "privé".

```python
class PositiveNumber:
    def __init__(self):
        # Le nom de l'attribut dans l'instance sera '_positive_number'
        self.private_name = '_positive_number'

    def __get__(self, instance, owner):
        if instance is None:
            return self
        print(f"GET: Récupération de '{self.private_name}'")
        # getattr(instance, self.private_name) récupère la valeur de l'attribut
        return getattr(instance, self.private_name)

    def __set__(self, instance, value):
        print(f"SET: Assignation de {value} à '{self.private_name}'")
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError("La valeur doit être un nombre positif.")
        # setattr(instance, self.private_name, value) stocke la valeur
        setattr(instance, self.private_name, value)

class Produit:
    prix = PositiveNumber()
    quantite = PositiveNumber()

    def __init__(self, nom, prix, quantite):
        self.nom = nom
        self.prix = prix       # Appelle Produit.prix.__set__(self, prix)
        self.quantite = quantite # Appelle Produit.quantite.__set__(self, quantite)

# --- Tests ---
p = Produit("Pomme", 1.5, 100)
# SET: Assignation de 1.5 à '_positive_number'
# SET: Assignation de 100 à '_positive_number'

print(f"Prix de la pomme : {p.prix}") # Appelle Produit.prix.__get__(p, Produit)
# GET: Récupération de '_positive_number'
# Prix de la pomme : 1.5

try:
    p.quantite = -10 # Appelle __set__ qui lève une erreur
except ValueError as e:
    print(f"Erreur: {e}")
# SET: Assignation de -10 à '_positive_number'
# Erreur: La valeur doit être un nombre positif.
```

**Problème :** Dans cet exemple, `prix` et `quantite` utilisent le même nom `_positive_number` pour stocker leur valeur dans l'instance, ce qui va créer un conflit. La valeur de `quantite` écrasera celle de `prix`.

## 4. Une Meilleure Gestion des Noms d'Attributs

Pour résoudre le conflit de noms, le descripteur doit connaître le nom de l'attribut auquel il est assigné dans la classe propriétaire. Python 3.6+ a introduit la méthode `__set_name__` pour cela.

`__set_name__(self, owner, name)`

- `owner`: La classe où le descripteur est défini (`Produit`).
- `name`: Le nom de l'attribut (`prix` ou `quantite`).

```python
class PositiveNumber:
    def __set_name__(self, owner, name):
        # Stocke le nom de l'attribut pour un usage futur
        self.public_name = name
        self.private_name = '_' + name

    def __get__(self, instance, owner):
        if instance is None:
            return self
        # Récupère la valeur de l'attribut privé, ex: instance._prix
        return getattr(instance, self.private_name)

    def __set__(self, instance, value):
        if not isinstance(value, (int, float)) or value <= 0:
            raise ValueError(f"'{self.public_name}' doit être un nombre positif.")
        # Stocke la valeur dans l'attribut privé, ex: instance._prix = value
        setattr(instance, self.private_name, value)

class Produit:
    prix = PositiveNumber()
    quantite = PositiveNumber()

    def __init__(self, nom, prix, quantite):
        self.nom = nom
        self.prix = prix
        self.quantite = quantite

# --- Tests ---
p = Produit("Livre", 25, 50)
print(f"Prix: {p.prix}, Quantité: {p.quantite}") # Prix: 25, Quantité: 50

# Inspectons le dictionnaire de l'instance
print(p.__dict__)
# {'nom': 'Livre', '_prix': 25, '_quantite': 50}
# Les valeurs sont bien stockées séparément !
```

## 5. Les Descripteurs dans le Cœur de Python

Les descripteurs sont un mécanisme fondamental en Python, utilisé pour implémenter de nombreuses fonctionnalités.

### a. Les Propriétés (`@property`)

L'utilisation de `@property` est en réalité du sucre syntaxique pour créer un descripteur.

```python
class MaClasse:
    def __init__(self):
        self._x = None

    @property
    def x(self):
        """Je suis la méthode 'get'."""
        return self._x

    @x.setter
    def x(self, value):
        """Je suis la méthode 'set'."""
        self._x = value

# Est (presque) équivalent à :

class PropertyDescriptor:
    def __init__(self, fget=None, fset=None):
        self.fget = fget
        self.fset = fset

    def __get__(self, instance, owner):
        return self.fget(instance)

    def __set__(self, instance, value):
        self.fset(instance, value)

class MaClasseEquivalente:
    def __init__(self):
        self._x = None

    def get_x(self):
        return self._x

    def set_x(self, value):
        self._x = value

    x = PropertyDescriptor(fget=get_x, fset=set_x)
```

### b. Les Méthodes

Même les méthodes de classe sont implémentées via des descripteurs (non-données). Quand vous faites `instance.methode`, vous n'obtenez pas directement la fonction. Vous obtenez une "méthode liée" (`bound method`).

Le `__get__` de la fonction est appelé. Il reçoit l'instance et retourne un objet "méthode liée" qui encapsule la fonction et l'instance (`self`). C'est ainsi que `self` est automatiquement passé lors de l'appel `instance.methode()`.

```python
class MaClasse:
    def ma_methode(self, arg):
        print(f"self: {self}, arg: {arg}")

instance = MaClasse()

# instance.ma_methode est une méthode liée
bound_method = instance.ma_methode
print(bound_method)
# <bound method MaClasse.ma_methode of <__main__.MaClasse object at ...>>

# L'appel passe 'self' automatiquement
bound_method(10)
# self: <__main__.MaClasse object at ...>, arg: 10
```

## Conclusion

Les descripteurs sont un patron de conception puissant qui permet de créer du code réutilisable pour gérer l'accès aux attributs. Ils offrent un contrôle fin sur la manière dont les attributs sont lus, écrits et supprimés. En comprenant les descripteurs, vous comprenez non seulement comment créer des validateurs de données élégants et des attributs "magiques", mais aussi comment fonctionnent des parties essentielles du langage Python lui-même, comme les propriétés et les méthodes.
