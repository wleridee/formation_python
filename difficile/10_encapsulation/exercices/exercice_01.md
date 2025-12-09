# Exercice 01 : Création d'une Classe `Temperature` avec Propriétés

## Objectif

Cet exercice a pour but de vous faire utiliser le décorateur `@property` pour créer une classe qui gère la température et permet de la lire en Celsius ou en Fahrenheit, tout en ne stockant la valeur que dans une seule unité (Celsius).

## Contexte

Lorsque vous manipulez des données qui peuvent être représentées dans différentes unités (comme des températures, des distances, des poids), il est courant de choisir une unité de stockage interne et de fournir des méthodes ou des propriétés pour convertir cette valeur dans d'autres unités à la volée.

Les propriétés sont parfaites pour cela, car elles donnent l'impression d'accéder à des attributs normaux (`temp.celsius`, `temp.fahrenheit`) tout en exécutant une logique de conversion en arrière-plan.

**Formules de conversion :**

- De Celsius à Fahrenheit : `F = C * 9/5 + 32`
- De Fahrenheit à Celsius : `C = (F - 32) * 5/9`

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `temperature.py`.

2.  **Définissez une classe `Temperature`**.

    - Le constructeur `__init__` doit accepter une température en **Celsius** et la stocker dans un attribut "privé" ou "protégé", par exemple `_celsius`.

3.  **Créez une propriété `celsius`**.

    - Utilisez `@property` pour définir un "getter" pour `celsius`. Cette méthode doit simplement retourner la valeur de `_celsius`.
    - Utilisez `@celsius.setter` pour définir un "setter". Cette méthode doit accepter une nouvelle valeur en Celsius, effectuer une validation simple (par exemple, s'assurer que la température n'est pas inférieure au zéro absolu, -273.15 °C), puis mettre à jour `_celsius`.

4.  **Créez une propriété `fahrenheit`**.

    - Utilisez `@property` pour définir un "getter" pour `fahrenheit`. Cette méthode ne doit **rien stocker**. Elle doit calculer la température en Fahrenheit à partir de la valeur de `_celsius` stockée et la retourner.
    - Utilisez `@fahrenheit.setter` pour définir un "setter". Cette méthode doit accepter une température en **Fahrenheit**, la convertir en Celsius, et assigner le résultat à la propriété `celsius` (ce qui déclenchera le setter de `celsius` et sa logique de validation).

5.  **Testez votre classe**.
    - Créez une instance de `Temperature` avec une valeur en Celsius (ex: 25 °C).
    - Affichez la température en Celsius et en Fahrenheit (`temp.celsius` et `temp.fahrenheit`).
    - Modifiez la température en utilisant la propriété `celsius` (`temp.celsius = 30`). Vérifiez que la valeur en Fahrenheit a été mise à jour automatiquement.
    - Modifiez la température en utilisant la propriété `fahrenheit` (`temp.fahrenheit = 50`). Vérifiez que la valeur en Celsius a été mise à jour correctement.
    - Essayez d'assigner une température invalide (ex: -300 °C) et vérifiez que votre validation fonctionne.

## Résultat Attendu

L'exécution de votre script de test doit montrer que les deux propriétés sont synchronisées, même si une seule valeur est réellement stockée.

```
Température initiale : 25.0 °C, ce qui équivaut à 77.0 °F.
---
Modification à 0 °C...
Nouvelle température : 0.0 °C, ce qui équivaut à 32.0 °F.
---
Modification à 50 °F...
Nouvelle température : 10.0 °C, ce qui équivaut à 50.0 °F.
---
Tentative de modification à -300 °C...
Erreur : La température ne peut pas être inférieure au zéro absolu (-273.15 °C).
Température finale : 10.0 °C.
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# temperature.py

class Temperature:
    """
    Classe pour gérer des températures avec conversion automatique
    entre Celsius et Fahrenheit.
    """
    ABSOLUTE_ZERO_C = -273.15

    def __init__(self, celsius: float = 0):
        # La seule donnée réellement stockée est la température en Celsius.
        # On utilise le setter dès le constructeur pour la validation.
        self.celsius = celsius

    @property
    def celsius(self) -> float:
        """Getter pour la température en Celsius."""
        return self._celsius

    @celsius.setter
    def celsius(self, value: float):
        """Setter pour la température en Celsius avec validation."""
        if value < self.ABSOLUTE_ZERO_C:
            raise ValueError(f"La température ne peut pas être inférieure au zéro absolu ({self.ABSOLUTE_ZERO_C} °C).")
        self._celsius = value

    @property
    def fahrenheit(self) -> float:
        """Getter pour la température en Fahrenheit (calculée à la volée)."""
        return self._celsius * 9/5 + 32

    @fahrenheit.setter
    def fahrenheit(self, value: float):
        """
        Setter pour la température en Fahrenheit.
        Convertit la valeur en Celsius et utilise le setter de celsius.
        """
        # On ne stocke rien ici. On délègue la modification à la propriété celsius.
        self.celsius = (value - 32) * 5/9

# --- Tests ---
if __name__ == "__main__":
    try:
        temp = Temperature(25)
        print(f"Température initiale : {temp.celsius:.1f} °C, ce qui équivaut à {temp.fahrenheit:.1f} °F.")

        print("---")
        print("Modification à 0 °C...")
        temp.celsius = 0
        print(f"Nouvelle température : {temp.celsius:.1f} °C, ce qui équivaut à {temp.fahrenheit:.1f} °F.")

        print("---")
        print("Modification à 50 °F...")
        temp.fahrenheit = 50
        print(f"Nouvelle température : {temp.celsius:.1f} °C, ce qui équivaut à {temp.fahrenheit:.1f} °F.")

        print("---")
        print("Tentative de modification à -300 °C...")
        temp.celsius = -300

    except ValueError as e:
        print(f"Erreur : {e}")
        print(f"Température finale : {temp.celsius:.1f} °C.")

```

</details>
