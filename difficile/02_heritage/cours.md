# Module : L'Héritage

## 1. Quoi : L'Héritage

L'**héritage** est l'un des piliers de la programmation orientée objet. C'est un mécanisme qui permet de créer une nouvelle classe (appelée **classe enfant**, **sous-classe** ou **classe dérivée**) à partir d'une classe existante (appelée **classe parente**, **super-classe** ou **classe de base**).

La classe enfant **hérite** de tous les attributs et de toutes les méthodes de la classe parente. Elle peut ensuite :

- Utiliser les fonctionnalités héritées telles quelles.
- Ajouter de nouveaux attributs et de nouvelles méthodes.
- **Redéfinir** (ou "surcharger", _override_) des méthodes de la classe parente pour qu'elles aient un comportement spécifique.

## 2. Pourquoi : Réutilisation et Spécialisation

- **Réutilisation du code (Principe DRY)** : Au lieu de copier-coller du code entre des classes similaires, vous placez le code commun dans une classe parente et vous en faites hériter les classes enfants.
- **Création de hiérarchies logiques** : L'héritage permet de modéliser des relations "est un" (a _is-a_ relationship). Par exemple, un `Chien` **est un** `Animal`. Une `VoitureElectrique` **est une** `Voiture`.
- **Spécialisation** : Vous partez d'une classe générale et vous créez des versions plus spécifiques qui ajoutent ou modifient des fonctionnalités.

## 3. Comment : La Syntaxe en Python

### A. Créer une classe enfant

Pour faire hériter une classe, on indique le nom de la classe parente entre parenthèses lors de la définition de la classe enfant.

```python
# Classe Parente
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        # Comportement générique
        print(f"{self.name} makes a sound.")

# Classe Enfant qui hérite de Animal
class Dog(Animal):
    # Pour l'instant, Dog ne fait rien de plus que Animal
    pass

# Utilisation
generic_animal = Animal("Creature")
generic_animal.speak() # Creature makes a sound.

my_dog = Dog("Buddy") # Dog hérite du __init__ de Animal
my_dog.speak()        # Dog hérite de la méthode speak() -> Buddy makes a sound.
```

### B. Redéfinir une méthode de la classe parente

La classe enfant peut fournir sa propre implémentation d'une méthode qui existe déjà dans la classe parente.

```python
class Cat(Animal):
    # On redéfinit la méthode speak() pour la classe Cat
    def speak(self):
        print(f"{self.name} says: Meow!")

my_cat = Cat("Whiskers")
my_cat.speak() # Affiche "Whiskers says: Meow!" et non le message générique.
```

### C. Ajouter de nouvelles méthodes

La classe enfant peut avoir ses propres méthodes, qui n'existent pas dans la classe parente.

```python
class Dog(Animal):
    # Redéfinition
    def speak(self):
        print(f"{self.name} says: Woof!")

    # Nouvelle méthode, spécifique à Dog
    def fetch(self, item):
        print(f"{self.name} fetches the {item}.")

my_dog = Dog("Rex")
my_dog.speak()      # Rex says: Woof!
my_dog.fetch("ball") # Rex fetches the ball.
```

### D. Étendre le constructeur `__init__`

Souvent, la classe enfant a besoin d'ajouter ses propres attributs en plus de ceux de la classe parente. Pour cela, il faut :

1.  Redéfinir la méthode `__init__` dans la classe enfant.
2.  **Appeler explicitement le constructeur de la classe parente** pour s'assurer que ses attributs sont bien initialisés. On utilise pour cela `super().__init__()`.

```python
class ElectricCar(Car): # On suppose qu'on a une classe Car
    """Represents aspects of a car, specific to electric vehicles."""

    def __init__(self, make, model, year, battery_size):
        """
        Initialize attributes of the parent class.
        Then initialize attributes specific to an electric car.
        """
        # 1. Appelle le __init__ de la classe parente (Car)
        super().__init__(make, model, year)

        # 2. Ajoute le nouvel attribut spécifique à ElectricCar
        self.battery_size = battery_size # en kWh

    # Nouvelle méthode
    def describe_battery(self):
        """Print a statement describing the battery size."""
        print(f"This car has a {self.battery_size}-kWh battery.")

    # Redéfinition d'une méthode existante (si Car avait une méthode fill_gas_tank)
    def fill_gas_tank(self):
        """Electric cars don't have gas tanks."""
        print("This car doesn't need a gas tank!")

# Utilisation
my_tesla = ElectricCar('tesla', 'model s', 2023, 100)
print(my_tesla.get_descriptive_name()) # Méthode héritée de Car
my_tesla.describe_battery()            # Nouvelle méthode
```

- `super()` est une fonction spéciale qui vous permet d'appeler des méthodes de la classe parente, évitant ainsi d'avoir à la nommer explicitement. C'est la manière moderne et recommandée de le faire.

## 4. Héritage Multiple

Python autorise une classe à hériter de **plusieurs classes parentes**.

```python
class A:
    def method_a(self):
        print("Method from A")

class B:
    def method_b(self):
        print("Method from B")

class C(A, B): # Hérite de A et de B
    pass

instance_c = C()
instance_c.method_a() # Method from A
instance_c.method_b() # Method from B
```

L'héritage multiple peut être puissant, mais il peut aussi introduire de la complexité, notamment le "problème du diamant" (diamond problem), qui concerne l'ordre dans lequel les méthodes sont résolues (MRO - Method Resolution Order). Il doit être utilisé avec prudence.
