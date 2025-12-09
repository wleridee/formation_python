# Exercice 01 : Hiérarchie de Personnages de Jeu Vidéo

## Objectif

Cet exercice a pour but de vous faire pratiquer l'héritage en créant une hiérarchie de classes pour des personnages de jeu vidéo. Vous créerez une classe de base `Character` et deux classes enfants `Warrior` et `Mage` qui en hériteront et la spécialiseront.

## Contexte

Dans un jeu, tous les personnages partagent des caractéristiques communes (un nom, des points de vie). Cependant, des classes spécifiques comme les guerriers et les mages ont des compétences et des attributs uniques.

- **Character (Parent)** :
  - Attributs : `name`, `health` (points de vie), `level`.
  - Méthodes : `attack()`, `show_info()`.
- **Warrior (Enfant)** :
  - Hérite de `Character`.
  - Attribut supplémentaire : `rage`.
  - Redéfinit `attack()` pour utiliser la rage.
- **Mage (Enfant)** :
  - Hérite de `Character`.
  - Attribut supplémentaire : `mana`.
  - Redéfinit `attack()` pour utiliser le mana.

## Énoncé

### Partie 1 : La Classe Parente `Character`

1.  **Créez un nouveau fichier Python** nommé `game_characters.py`.
2.  **Définissez la classe `Character`** :
    - Son constructeur `__init__` doit prendre `name` et `level` comme arguments.
    - Il doit initialiser `self.name`, `self.level`, et `self.health` (initialisez la vie à `level * 10`).
    - Créez une méthode `attack()` qui affiche : `f"{self.name} performs a basic attack."`.
    - Créez une méthode `show_info()` qui affiche les informations du personnage (nom, niveau, vie).

### Partie 2 : La Classe Enfant `Warrior`

1.  **Définissez la classe `Warrior` qui hérite de `Character`**.
2.  **Étendez le constructeur `__init__`** :
    - Il doit prendre `name` et `level` en arguments.
    - Utilisez `super().__init__()` pour appeler le constructeur du parent.
    - Ajoutez un nouvel attribut `self.rage`, initialisé à `100`.
3.  **Redéfinissez la méthode `attack()`** :
    - Elle doit vérifier si `self.rage` est supérieur ou égal à 10.
    - Si oui, elle affiche `f"{self.name} swings their mighty axe!"` et décrémente `self.rage` de 10.
    - Sinon, elle affiche `f"{self.name} is out of rage."`.

### Partie 3 : La Classe Enfant `Mage`

1.  **Définissez la classe `Mage` qui hérite de `Character`**.
2.  **Étendez le constructeur `__init__`** :
    - Il doit prendre `name` et `level` en arguments.
    - Utilisez `super().__init__()`.
    - Ajoutez un nouvel attribut `self.mana`, initialisé à `50`.
3.  **Redéfinissez la méthode `attack()`** :
    - Elle doit vérifier si `self.mana` est supérieur ou égal à 20.
    - Si oui, elle affiche `f"{self.name} casts a powerful fireball!"` et décrémente `self.mana` de 20.
    - Sinon, elle affiche `f"{self.name} is out of mana."`.

### Partie 4 : Test

1.  Créez une instance de `Warrior` et une instance de `Mage`.
2.  Affichez les informations de chaque personnage.
3.  Faites-les attaquer plusieurs fois pour voir leurs ressources (rage/mana) diminuer et le message changer lorsqu'ils n'en ont plus assez.

## Résultat Attendu

```
--- Character Info ---
Name: Conan, Level: 5, Health: 50
Name: Gandalf, Level: 6, Health: 60

--- Combat ---
Conan swings their mighty axe!
Gandalf casts a powerful fireball!
Conan swings their mighty axe!
Gandalf casts a powerful fireball!
Gandalf is out of mana.
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# game_characters.py

# 1. Classe Parente
class Character:
    def __init__(self, name, level):
        self.name = name
        self.level = level
        self.health = level * 10

    def attack(self):
        print(f"{self.name} performs a basic attack.")

    def show_info(self):
        print(f"Name: {self.name}, Level: {self.level}, Health: {self.health}")

# 2. Classe Enfant Warrior
class Warrior(Character):
    def __init__(self, name, level):
        super().__init__(name, level)
        self.rage = 100

    def attack(self):
        if self.rage >= 10:
            print(f"{self.name} swings their mighty axe!")
            self.rage -= 10
        else:
            print(f"{self.name} is out of rage.")

# 3. Classe Enfant Mage
class Mage(Character):
    def __init__(self, name, level):
        super().__init__(name, level)
        self.mana = 50

    def attack(self):
        if self.mana >= 20:
            print(f"{self.name} casts a powerful fireball!")
            self.mana -= 20
        else:
            print(f"{self.name} is out of mana.")

# 4. Test
warrior = Warrior("Conan", 5)
mage = Mage("Gandalf", 6)

print("--- Character Info ---")
warrior.show_info()
mage.show_info()

print("\n--- Combat ---")
warrior.attack()
mage.attack()
warrior.attack()
mage.attack()
mage.attack() # Le mage ne devrait plus avoir assez de mana

```

</details>
