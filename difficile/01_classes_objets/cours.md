# Module : Classes et Objets

## 1. Quoi : La Programmation Orientée Objet (POO)

La **Programmation Orientée Objet** est un paradigme de programmation qui repose sur le concept d'"objets". Un objet est une entité qui regroupe des **données** (appelées attributs) et des **comportements** (appelées méthodes).

- Une **Classe** est un **plan** ou un **modèle** pour créer des objets. Elle définit les attributs et les méthodes que tous les objets de cette classe auront.
- Un **Objet** (ou une **instance**) est une **création concrète** à partir d'une classe. Vous pouvez créer de multiples objets à partir d'une seule classe, chacun avec ses propres valeurs d'attributs.

Imaginez que la classe `Voiture` est le plan de construction. Les objets `ma_voiture_rouge` et `la_voiture_de_mon_voisin` sont des voitures réelles construites à partir de ce plan.

## 2. Pourquoi : Modéliser le monde réel

La POO est extrêmement puissante pour organiser des programmes complexes, car elle permet de modéliser des concepts du monde réel.

- **Encapsulation** : Regrouper les données et les comportements qui leur sont liés au sein d'un même objet. Cela rend le code plus organisé et plus facile à gérer.
- **Abstraction** : Cacher les détails complexes de l'implémentation et n'exposer qu'une interface simple pour interagir avec l'objet.
- **Héritage** : Créer de nouvelles classes basées sur des classes existantes pour réutiliser du code et créer des hiérarchies (ex: une classe `Chat` et une classe `Chien` peuvent hériter d'une classe `Animal`).
- **Polymorphisme** : Permettre à des objets de différentes classes de répondre au même message (appel de méthode) de manière spécifique à leur type.

## 3. Comment : Définir une Classe en Python

### A. Syntaxe de base

On définit une classe avec le mot-clé `class`, suivi du nom de la classe (par convention, en `CamelCase`).

```python
class Dog:
    # Le code de la classe est indenté
    pass # 'pass' est un placeholder pour indiquer un bloc vide
```

### B. Le Constructeur : `__init__()`

La méthode `__init__()` est une méthode spéciale appelée "constructeur". Elle est exécutée **automatiquement** chaque fois qu'un nouvel objet de la classe est créé. C'est l'endroit idéal pour initialiser les **attributs** de l'objet.

- Le premier paramètre de chaque méthode d'instance est toujours `self`. `self` représente l'objet lui-même. C'est à travers `self` que vous pouvez accéder aux attributs et méthodes de l'objet.

```python
class Dog:
    # Le constructeur
    def __init__(self, name, age):
        # On crée les attributs de l'instance 'self'
        self.name = name
        self.age = age
        print(f"A new dog named {self.name} has been created!")

# Création de deux objets (instances) de la classe Dog
dog1 = Dog("Buddy", 3)
dog2 = Dog("Lucy", 5)

# Chaque objet a ses propres attributs
print(f"{dog1.name} is {dog1.age} years old.") # Buddy is 3 years old.
print(f"{dog2.name} is {dog2.age} years old.") # Lucy is 5 years old.
```

### C. Les Méthodes d'Instance

Les méthodes sont des fonctions définies à l'intérieur d'une classe. Elles représentent les comportements de l'objet. Elles prennent toujours `self` comme premier argument pour pouvoir accéder aux attributs de l'instance.

```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    # Une méthode d'instance
    def bark(self):
        print(f"{self.name} says: Woof!")

    # Une autre méthode d'instance qui utilise les attributs
    def get_human_years(self):
        return self.age * 7

# Création d'un objet
my_dog = Dog("Rex", 4)

# Appel des méthodes de l'objet
my_dog.bark() # Rex says: Woof!
human_age = my_dog.get_human_years()
print(f"In human years, {my_dog.name} is {human_age} years old.")
```

### D. Attributs de Classe

Un attribut de classe est une variable qui est **partagée par toutes les instances** de la classe. On le définit directement sous la déclaration de la classe, en dehors de toute méthode.

```python
class Dog:
    # Attribut de classe, partagé par tous les chiens
    species = "Canis familiaris"

    def __init__(self, name):
        self.name = name # Attribut d'instance

dog1 = Dog("Buddy")
dog2 = Dog("Lucy")

# On peut y accéder via la classe ou l'instance
print(Dog.species)   # Canis familiaris
print(dog1.species)  # Canis familiaris
print(dog2.species)  # Canis familiaris
```

## 4. Exemple complet : un compte bancaire

```python
class BankAccount:
    # Le constructeur initialise le compte avec un nom de titulaire et un solde initial
    def __init__(self, owner_name, initial_balance=0):
        self.owner_name = owner_name
        self.balance = initial_balance
        print(f"Account for {self.owner_name} created with balance: ${self.balance}")

    # Méthode pour déposer de l'argent
    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            print(f"Deposited ${amount}. New balance: ${self.balance}")
        else:
            print("Deposit amount must be positive.")

    # Méthode pour retirer de l'argent
    def withdraw(self, amount):
        if 0 < amount <= self.balance:
            self.balance -= amount
            print(f"Withdrew ${amount}. New balance: ${self.balance}")
        else:
            print("Invalid withdrawal amount or insufficient funds.")

    # Méthode pour afficher le solde
    def get_balance(self):
        print(f"Current balance for {self.owner_name}: ${self.balance}")

# Utilisation de la classe
account1 = BankAccount("Alice", 500)
account1.deposit(100)
account1.withdraw(50)
account1.withdraw(600) # Tentative de retrait invalide
account1.get_balance()
```
