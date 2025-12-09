# Module : Méthodes de Classe et Méthodes Statiques

## 1. Quoi : Au-delà des méthodes d'instance

En plus des méthodes d'instance (celles qui prennent `self` comme premier argument), les classes Python peuvent avoir deux autres types de méthodes : les **méthodes de classe** et les **méthodes statiques**.

- **Méthode d'instance** : `def my_method(self, ...)`

  - Opère sur une instance spécifique (`self`).
  - Peut accéder et modifier les attributs de l'instance (`self.name`).
  - C'est le type de méthode le plus courant.

- **Méthode de classe** : ` @classmethod def my_method(cls, ...)`

  - Opère sur la classe elle-même (`cls`), pas sur une instance.
  - Peut accéder et modifier les attributs de la classe (`cls.species`).
  - Ne peut pas accéder aux attributs d'une instance spécifique (`self`).

- **Méthode statique** : `@staticmethod def my_method(...)`
  - N'opère ni sur l'instance (`self`) ni sur la classe (`cls`).
  - Est essentiellement une fonction normale, mais "rangée" à l'intérieur de la classe pour des raisons d'organisation.
  - Ne peut accéder ni aux attributs de l'instance, ni aux attributs de la classe.

## 2. Pourquoi : Différents niveaux d'opération

Le choix entre ces types de méthodes dépend de ce sur quoi la méthode doit opérer.

- Utilisez une **méthode d'instance** pour tout ce qui concerne un objet spécifique (ex: `my_car.drive()`).
- Utilisez une **méthode de classe** lorsque la méthode est liée à la classe dans son ensemble, mais pas à une instance particulière. Un cas d'usage très courant est la création de **constructeurs alternatifs**.
- Utilisez une **méthode statique** pour une fonction utilitaire qui est logiquement liée à la classe, mais qui n'a pas besoin d'accéder à l'état de la classe ou de l'instance.

## 3. Comment : Les Décorateurs `@classmethod` et `@staticmethod`

### A. Méthodes de Classe (`@classmethod`)

On utilise le décorateur `@classmethod`. Le premier argument de la méthode est, par convention, `cls` (qui représente la classe).

**Exemple : Un constructeur alternatif**

Imaginons que nous avons une classe `Person` et que nous voulons pouvoir la créer à partir d'une année de naissance, en plus du constructeur standard qui prend l'âge.

```python
import datetime

class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod
    def from_birth_year(cls, name, birth_year):
        """
        Constructeur alternatif pour créer une personne à partir de son année de naissance.
        'cls' ici est la classe Person elle-même.
        """
        current_year = datetime.date.today().year
        age = current_year - birth_year
        # On retourne une nouvelle instance de la classe 'cls'
        return cls(name, age)

    def display_info(self):
        print(f"{self.name} is {self.age} years old.")

# Utilisation du constructeur standard
person1 = Person("Alice", 30)

# Utilisation du constructeur alternatif (méthode de classe)
person2 = Person.from_birth_year("Bob", 1995)

person1.display_info() # Alice is 30 years old.
person2.display_info() # Bob is 30 years old. (en 2025)
```

L'avantage d'utiliser `cls` au lieu de `Person` directement dans la méthode est que si une autre classe hérite de `Person`, cette méthode de classe créera une instance de la classe enfant, pas de `Person`.

### B. Méthodes Statiques (`@staticmethod`)

On utilise le décorateur `@staticmethod`. La méthode ne prend ni `self` ni `cls` comme premier argument implicite.

**Exemple : Une fonction utilitaire**

Imaginons une classe `MathUtils` qui regroupe des fonctions mathématiques. Ces fonctions n'ont pas besoin de l'état d'une instance ou de la classe.

```python
class MathUtils:
    @staticmethod
    def add(a, b):
        """Une fonction simple qui n'a pas besoin de self ou cls."""
        return a + b

    @staticmethod
    def is_even(number):
        """Vérifie si un nombre est pair."""
        return number % 2 == 0

# On peut appeler la méthode statique directement depuis la classe
result = MathUtils.add(5, 3)
print(result) # 8

print(MathUtils.is_even(10)) # True
print(MathUtils.is_even(7))  # False
```

**Question :** Pourquoi ne pas simplement définir `is_even` comme une fonction normale en dehors de la classe ?
**Réponse :** On le pourrait. Mais la placer à l'intérieur de la classe `MathUtils` a du sens d'un point de vue de l'**organisation** et de l'**espace de noms** (`namespace`). Cela indique clairement que `is_even` est une fonctionnalité liée au concept de `MathUtils`.

## 4. Résumé

| Type de Méthode        | Décorateur      | Premier Argument | Accède à `self` (instance) ? | Accède à `cls` (classe) ? | Objectif Principal                                         |
| ---------------------- | --------------- | ---------------- | ---------------------------- | ------------------------- | ---------------------------------------------------------- |
| **Méthode d'Instance** | (aucun)         | `self`           | ✅ Oui                       | ✅ Oui                    | Opérer sur l'état d'une instance spécifique.               |
| **Méthode de Classe**  | `@classmethod`  | `cls`            | ❌ Non                       | ✅ Oui                    | Opérer sur l'état de la classe, constructeurs alternatifs. |
| **Méthode Statique**   | `@staticmethod` | (aucun)          | ❌ Non                       | ❌ Non                    | Fonction utilitaire logiquement liée à la classe.          |
