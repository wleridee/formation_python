# Exercice 01 : Modélisation d'une Personne avec un Âge Contrôlé

## Objectif

Cet exercice a pour but de vous faire utiliser le décorateur `@property` pour créer un attribut calculé en lecture seule (`age`) et un attribut contrôlé (`email`) avec un getter et un setter.

## Contexte

Vous allez créer une classe `Person` qui stocke le nom et la date de naissance d'une personne.

- L'**âge** ne sera pas stocké directement. Il sera calculé à la volée à partir de la date de naissance. Ce doit être une propriété en **lecture seule**.
- L'**email** sera un attribut contrôlé. Le "setter" devra valider que la valeur fournie contient bien un caractère "@".

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `person_model.py`.
2.  **Importez le module `datetime`** en haut du fichier.

3.  **Définissez la classe `Person`**.

    - Son constructeur `__init__` doit accepter `name` et `birth_date`. `birth_date` sera un objet `datetime.date`.
    - Stockez `name` et `birth_date` comme attributs.
    - Initialisez un attribut "privé" `_email` à `None`.

4.  **Créez la propriété `age` en lecture seule**.

    - Définissez une méthode `age` et décorez-la avec `@property`.
    - Cette méthode doit calculer l'âge de la personne en se basant sur la date d'aujourd'hui (`datetime.date.today()`) et `self.birth_date`.
    - _Astuce :_ La différence entre deux dates est un objet `timedelta`. Vous pouvez obtenir le nombre de jours avec `.days` et diviser par 365.25 pour avoir une approximation de l'âge en années.
    - La méthode doit retourner l'âge sous forme d'entier.
    - Ne créez pas de setter pour `age`.

5.  **Créez la propriété `email` contrôlée**.

    - **Getter** : Définissez une méthode `email` décorée avec `@property` qui retourne la valeur de `self._email`.
    - **Setter** : Définissez une méthode `email` décorée avec `@email.setter`.
      - Cette méthode prend `value` en argument.
      - Elle doit vérifier si la chaîne `value` contient le caractère `"@"`.
      - Si ce n'est pas le cas, elle doit lever une `ValueError` avec un message clair.
      - Si la validation réussit, elle assigne `value` à `self._email`.

6.  **Testez votre classe** :
    - Créez une instance de `Person` avec une date de naissance.
    - Affichez le nom et l'âge de la personne. L'âge doit être calculé correctement.
    - Essayez d'assigner une valeur à l'âge (`person.age = 30`). Vous devriez obtenir une `AttributeError`.
    - Assignez un email valide à la personne et affichez-le.
    - Essayez d'assigner un email invalide et assurez-vous qu'une `ValueError` est bien levée (vous pouvez utiliser un bloc `try...except` pour la démonstration).

## Résultat Attendu

```
Name: Alice, Age: 30
---
Attempting to set age directly...
Error: can't set attribute 'age'
---
Setting a valid email...
Email: alice@example.com
---
Attempting to set an invalid email...
Error: Invalid email address provided.
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# person_model.py
import datetime

class Person:
    def __init__(self, name, birth_date):
        self.name = name
        self.birth_date = birth_date
        self._email = None

    @property
    def age(self):
        """Calculates the person's age in years. This is a read-only property."""
        today = datetime.date.today()
        # Calculate the difference in days and approximate the years
        age_in_days = (today - self.birth_date).days
        return int(age_in_days / 365.25)

    @property
    def email(self):
        """Getter for the email address."""
        return self._email

    @email.setter
    def email(self, value):
        """Setter for the email address with validation."""
        if "@" not in value:
            raise ValueError("Invalid email address provided.")
        self._email = value

# --- Testing ---

# Create a person instance (assuming today is around late 2025)
p = Person("Alice", datetime.date(1995, 5, 10))

# Test name and read-only age
print(f"Name: {p.name}, Age: {p.age}")
print("---")

# Test setting age (should fail)
print("Attempting to set age directly...")
try:
    p.age = 30
except AttributeError as e:
    print(f"Error: {e}")
print("---")

# Test setting a valid email
print("Setting a valid email...")
p.email = "alice@example.com"
print(f"Email: {p.email}")
print("---")

# Test setting an invalid email (should fail)
print("Attempting to set an invalid email...")
try:
    p.email = "invalid-email.com"
except ValueError as e:
    print(f"Error: {e}")

```

</details>
