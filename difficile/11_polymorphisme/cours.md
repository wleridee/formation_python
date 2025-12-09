# Module : Le Polymorphisme et le Duck Typing

## Objectif

Ce module a pour but de vous faire comprendre le concept de polymorphisme en Python, et comment il est intimement lié à la philosophie du "Duck Typing". Vous apprendrez à écrire du code plus flexible et générique qui peut fonctionner avec des objets de différents types.

## 1. Qu'est-ce que le Polymorphisme ?

Le **polymorphisme** (du grec "plusieurs formes") est la capacité d'un objet à prendre plusieurs formes. En programmation, cela signifie qu'une même interface (comme un appel de fonction ou de méthode) peut être utilisée pour des objets de types différents, et chaque objet répondra de manière appropriée à son type.

En Python, le polymorphisme est omniprésent. L'opérateur `+`, par exemple, est polymorphique :

- `2 + 3` donne `5` (addition de nombres).
- `"hello" + " world"` donne `"hello world"` (concaténation de chaînes).
- `[1, 2] + [3, 4]` donne `[1, 2, 3, 4]` (concaténation de listes).

L'opération est la même (`+`), mais le comportement dépend du type des opérandes.

## 2. Le Polymorphisme avec l'Héritage

Une manière classique d'implémenter le polymorphisme est via l'héritage. On définit une méthode dans une classe de base, et les classes filles la surchargent (`override`) avec leur propre comportement.

```python
class Animal:
    def __init__(self, nom):
        self.nom = nom

    def parler(self):
        # Comportement par défaut ou erreur
        raise NotImplementedError("La classe fille doit implémenter cette méthode.")

class Chien(Animal):
    def parler(self):
        return "Woof!"

class Chat(Animal):
    def parler(self):
        return "Meow!"

# Créons une liste d'animaux de différents types
animaux = [Chien("Rex"), Chat("Misty"), Chien("Buddy")]

# On peut itérer et appeler la même méthode .parler() sur chaque objet.
# Chaque animal répondra à sa manière.
for animal in animaux:
    print(f"{animal.nom} dit : {animal.parler()}")

# Output:
# Rex dit : Woof!
# Misty dit : Meow!
# Buddy dit : Woof!
```

Ici, la fonction qui itère sur la liste `animaux` n'a pas besoin de savoir si un objet est un `Chien` ou un `Chat`. Elle sait juste qu'il a une méthode `parler()`, et elle l'appelle. C'est du polymorphisme.

## 3. Le "Duck Typing" : la Philosophie de Python

La citation célèbre est :

> "If it walks like a duck and it quacks like a duck, then it must be a duck."
> (S'il marche comme un canard et cancane comme un canard, alors ce doit être un canard.)

En Python, cela se traduit par : **le type d'un objet est moins important que les méthodes qu'il définit**. Si votre code s'attend à un objet qui a une méthode `.parler()`, il ne se soucie pas de savoir si cet objet est un `Animal`, un `Chien` ou autre chose. Tant qu'il a une méthode `.parler()`, ça fonctionne.

Le Duck Typing permet un polymorphisme qui ne dépend pas de l'héritage.

```python
class Chien:
    def parler(self):
        return "Woof!"

class Chat:
    def parler(self):
        return "Meow!"

class Personne:
    def parler(self):
        return "Bonjour !"

# Cette fonction fonctionne avec n'importe quel objet
# qui a une méthode .parler().
def faire_parler(creature):
    print(creature.parler())

# Ces objets ne partagent AUCUN lien d'héritage.
rex = Chien()
misty = Chat()
alice = Personne()

faire_parler(rex)      # Woof!
faire_parler(misty)    # Meow!
faire_parler(alice)    # Bonjour !
```

Le code est plus flexible et découplé. La fonction `faire_parler` n'a aucune dépendance envers une classe `Animal` de base.

### Un exemple plus concret : les itérables

La boucle `for` en Python est un excellent exemple de Duck Typing. Elle fonctionne avec n'importe quel objet qui est **itérable**, c'est-à-dire qui implémente la méthode spéciale `__iter__()`.

```python
# list, tuple, str, dict, set sont tous des itérables.
ma_liste = [1, 2, 3]
mon_tuple = ('a', 'b', 'c')
ma_chaine = "hello"

for element in ma_liste:
    print(element)

for element in mon_tuple:
    print(element)

for element in ma_chaine:
    print(element)
```

La boucle `for` ne se soucie pas du type de l'objet, seulement de sa capacité à être itéré.

## 4. Avantages et Inconvénients du Duck Typing

**Avantages :**

- **Flexibilité :** Permet d'écrire du code générique qui fonctionne avec une grande variété d'objets.
- **Découplage :** Le code ne dépend pas de hiérarchies de classes spécifiques. Il est plus facile de remplacer une partie du système par une autre.
- **Simplicité :** Pas besoin de créer des hiérarchies d'héritage complexes ou des interfaces formelles juste pour le polymorphisme.

**Inconvénients :**

- **Moins de sécurité à la compilation :** Le typage dynamique signifie que les erreurs (par exemple, appeler une méthode qui n'existe pas) ne sont détectées qu'à l'exécution.
- **Lisibilité :** Il peut être moins évident de savoir quel type d'objet une fonction attend. C'est là que le **typage statique** (Type Hinting) et une bonne documentation deviennent cruciaux.

### Utiliser le Typage Statique avec le Duck Typing

Le typage statique (introduit dans les modules récents) permet de clarifier les attentes sans sacrifier la flexibilité du Duck Typing. On peut utiliser les **Protocoles** (`typing.Protocol`) pour définir une interface attendue.

```python
from typing import Protocol

# On définit un "contrat" : tout objet qui a une méthode parler()
# qui ne prend pas d'argument et retourne une chaîne est "Parlant".
class Parlant(Protocol):
    def parler(self) -> str:
        ...

def faire_parler(creature: Parlant):
    print(creature.parler())

class Chien:
    def parler(self) -> str:
        return "Woof!"

class Humain:
    def __init__(self, nom):
        self.nom = nom
    def parler(self) -> str:
        return f"Je m'appelle {self.nom}"

class Voiture:
    def demarrer(self):
        print("Vroom!")

faire_parler(Chien())   # OK
faire_parler(Humain("Alice")) # OK

# Un analyseur de type statique comme mypy lèvera une erreur ici,
# car Voiture ne respecte pas le protocole Parlant.
# faire_parler(Voiture()) # Erreur de typage
```

Les protocoles formalisent le Duck Typing, offrant le meilleur des deux mondes : la flexibilité du typage dynamique et la sécurité du typage statique.

## Conclusion

Le polymorphisme est un concept clé pour écrire du code flexible et réutilisable. En Python, il est le plus souvent exprimé à travers le Duck Typing : ce qui compte, ce sont les comportements (méthodes) d'un objet, pas son type ou son héritage. Cette philosophie, combinée à des outils modernes comme les protocoles de typage, permet de construire des systèmes robustes et adaptables.
