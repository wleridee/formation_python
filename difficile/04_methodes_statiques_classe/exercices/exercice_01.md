# Exercice 01 : Validateur de Date

## Objectif

Cet exercice a pour but de vous faire utiliser des méthodes de classe et des méthodes statiques dans un contexte pratique : la création et la validation d'une classe `Date`.

## Contexte

Vous allez créer une classe `Date` qui stocke un jour, un mois et une année.

- Le constructeur principal (`__init__`) prendra le jour, le mois et l'année comme entiers.
- Vous créerez un **constructeur alternatif** (une méthode de classe) pour créer une instance de `Date` à partir d'une chaîne de caractères au format "DD-MM-YYYY".
- Vous ajouterez une **méthode statique** comme fonction utilitaire pour vérifier si une année est bissextile, car cette logique est liée aux dates mais n'a pas besoin d'une instance ou de la classe `Date` elle-même pour fonctionner.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `date_validator.py`.

2.  **Définissez la classe `Date`**.

    - Son constructeur `__init__` doit accepter `day`, `month`, et `year` et les stocker.
    - Implémentez la méthode `__str__` pour qu'elle retourne la date au format "DD/MM/YYYY".

3.  **Créez une méthode statique `is_leap_year`**.

    - Décorez-la avec `@staticmethod`.
    - Elle prend un seul argument, `year`.
    - Elle doit retourner `True` si l'année est bissextile, `False` sinon.
    - _Rappel de la logique d'une année bissextile :_ une année est bissextile si elle est divisible par 4, sauf si elle est divisible par 100, à moins qu'elle ne soit également divisible par 400.

4.  **Créez une méthode de classe `from_string`**.

    - Décorez-la avec `@classmethod`.
    - Elle prend deux arguments : `cls` et `date_string`.
    - Elle doit "parser" la `date_string` (qui est au format "DD-MM-YYYY") pour en extraire le jour, le mois et l'année.
      - _Astuce :_ Utilisez la méthode `.split('-')` sur la chaîne.
    - Elle doit convertir ces parties en entiers.
    - Elle doit ensuite retourner une nouvelle instance de la classe en utilisant `cls(day, month, year)`.

5.  **Testez votre classe** :
    - Créez une instance de `Date` en utilisant le constructeur normal.
    - Créez une autre instance en utilisant la méthode de classe `from_string`.
    - Affichez les deux dates pour vérifier que la méthode `__str__` fonctionne.
    - Testez la méthode statique `is_leap_year` avec quelques années (ex: 2020, 2021, 1900, 2000).

## Résultat Attendu

```
Date 1: 25/12/2023
Date 2: 30/10/2024
Is 2020 a leap year? True
Is 2021 a leap year? False
Is 1900 a leap year? False
Is 2000 a leap year? True
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# date_validator.py

class Date:
    def __init__(self, day, month, year):
        self.day = day
        self.month = month
        self.year = year

    def __str__(self):
        return f"{self.day:02d}/{self.month:02d}/{self.year}"

    @staticmethod
    def is_leap_year(year):
        """Checks if a year is a leap year."""
        return (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0)

    @classmethod
    def from_string(cls, date_string):
        """Creates a Date instance from a 'DD-MM-YYYY' string."""
        day, month, year = map(int, date_string.split('-'))
        return cls(day, month, year)

# --- Testing ---

# 1. Using the standard constructor
date1 = Date(25, 12, 2023)
print(f"Date 1: {date1}")

# 2. Using the class method as an alternative constructor
date_str = "30-10-2024"
date2 = Date.from_string(date_str)
print(f"Date 2: {date2}")

# 3. Using the static method
print(f"Is 2020 a leap year? {Date.is_leap_year(2020)}")
print(f"Is 2021 a leap year? {Date.is_leap_year(2021)}")
print(f"Is 1900 a leap year? {Date.is_leap_year(1900)}")
print(f"Is 2000 a leap year? {Date.is_le_year(2000)}")

```

</details>
