# Exercice 01 : Modélisation d'une Voiture

## Objectif

Cet exercice a pour but de vous faire créer votre première classe, `Car`, pour modéliser un objet simple avec ses attributs (données) et ses méthodes (comportements).

## Contexte

Vous allez créer une classe `Car` qui représente une voiture. Une voiture a une marque, un modèle, une année, et un kilométrage. Elle peut également rouler (ce qui augmente son kilométrage) et afficher ses informations.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `car_model.py`.

2.  **Définissez la classe `Car`**.

    - Par convention, les noms de classe utilisent le `CamelCase`.

3.  **Écrivez le constructeur `__init__`** :

    - Il doit accepter trois arguments (en plus de `self`) : `make` (la marque), `model` (le modèle), et `year` (l'année).
    - À l'intérieur du constructeur, initialisez les attributs d'instance suivants :
      - `self.make`
      - `self.model`
      - `self.year`
      - `self.odometer_reading` : initialisez cet attribut à `0`. Il représentera le kilométrage de la voiture.

4.  **Créez une méthode `get_descriptive_name`** :

    - Cette méthode ne prend pas d'autres arguments que `self`.
    - Elle doit retourner une chaîne de caractères formatée décrivant la voiture, par exemple : `"2023 Ford Mustang"`.

5.  **Créez une méthode `read_odometer`** :

    - Cette méthode doit afficher le kilométrage de la voiture de manière claire, par exemple : `"This car has 0 miles on it."`.

6.  **Créez une méthode `drive`** :

    - Cette méthode doit accepter un argument `miles` (le nombre de kilomètres parcourus).
    - Elle doit vérifier que `miles` est un nombre positif.
    - Si c'est le cas, elle ajoute ce nombre au `self.odometer_reading`.
    - Sinon, elle affiche un message d'erreur : `"You can't drive negative miles!"`.

7.  **Testez votre classe** :
    - Créez une instance de votre classe `Car`.
    - Appelez `get_descriptive_name` et affichez le résultat.
    - Appelez `read_odometer`.
    - Appelez la méthode `drive` avec une valeur positive (ex: 100) et appelez à nouveau `read_odometer` pour voir le changement.
    - Appelez la méthode `drive` avec une valeur négative pour tester la gestion de l'erreur.

## Résultat Attendu

```
2023 Ford Mustang
This car has 0 miles on it.
This car has 100 miles on it.
You can't drive negative miles!
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# car_model.py

class Car:
    """A simple attempt to represent a car."""

    def __init__(self, make, model, year):
        """Initialize attributes to describe a car."""
        self.make = make
        self.model = model
        self.year = year
        self.odometer_reading = 0

    def get_descriptive_name(self):
        """Return a neatly formatted descriptive name."""
        long_name = f"{self.year} {self.make} {self.model}"
        return long_name.title()

    def read_odometer(self):
        """Print a statement showing the car's mileage."""
        print(f"This car has {self.odometer_reading} miles on it.")

    def drive(self, miles):
        """Increase the odometer reading by the given amount."""
        if miles >= 0:
            self.odometer_reading += miles
        else:
            print("You can't drive negative miles!")

# --- Testing the class ---

# Create an instance of the Car class
my_new_car = Car('ford', 'mustang', 2023)

# Print the descriptive name
print(my_new_car.get_descriptive_name())

# Read the initial odometer reading
my_new_car.read_odometer()

# Drive the car and check the odometer again
my_new_car.drive(100)
my_new_car.read_odometer()

# Try to drive a negative distance
my_new_car.drive(-50)

```

</details>
