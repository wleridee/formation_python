# Exercice 01 : Calculateur de Pourboire

## Objectif

Cet exercice a pour but de vous faire utiliser les opérateurs arithmétiques et d'assignation pour calculer le montant d'un repas, incluant un pourboire.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `tip_calculator.py`.

2.  **Déclarez les variables suivantes** pour représenter le coût d'un repas :

    ```python
    meal_price = 44.50
    tip_percentage = 20
    ```

3.  **Calculez le montant du pourboire** :

    - Créez une variable `tip_amount`.
    - Calculez le montant du pourboire en vous basant sur le `meal_price` et le `tip_percentage`. (Rappel : `20%` équivaut à `20 / 100`).

4.  **Calculez le coût total** :

    - Créez une variable `total_cost`.
    - Calculez le coût total en additionnant le `meal_price` et le `tip_amount`.

5.  **Affichez les résultats** :
    - Utilisez des f-strings et des `print()` pour afficher un résumé clair, formaté avec deux décimales pour les montants.

## Résultat Attendu

La sortie de votre script doit être :

```
--- Bill Summary ---
Meal Price: $44.50
Tip Percentage: 20%
Tip Amount: $8.90
Total Cost: $53.40
```

## Bonus (Optionnel)

Modifiez votre script pour qu'il calcule également le coût par personne.

1.  **Ajoutez une variable** :

    ```python
    number_of_people = 4
    ```

2.  **Calculez le coût par personne** :

    - Créez une variable `cost_per_person`.
    - Divisez le `total_cost` par le `number_of_people`.

3.  **Affichez le résultat supplémentaire**.

### Résultat Attendu (avec bonus)

```
--- Bill Summary ---
Meal Price: $44.50
Tip Percentage: 20%
Tip Amount: $8.90
Total Cost: $53.40

--- Per Person ---
Number of People: 4
Cost per Person: $13.35
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# tip_calculator.py

# Variables initiales
meal_price = 44.50
tip_percentage = 20
number_of_people = 4 # Pour le bonus

# Calcul du pourboire
tip_amount = meal_price * (tip_percentage / 100)

# Calcul du coût total
total_cost = meal_price + tip_amount

# Affichage du résumé
print("--- Bill Summary ---")
print(f"Meal Price: ${meal_price:.2f}")
print(f"Tip Percentage: {tip_percentage}%")
print(f"Tip Amount: ${tip_amount:.2f}")
print(f"Total Cost: ${total_cost:.2f}")

# --- Bonus ---
print("\n--- Per Person ---")

# Calcul du coût par personne
cost_per_person = total_cost / number_of_people

# Affichage du bonus
print(f"Number of People: {number_of_people}")
print(f"Cost per Person: ${cost_per_person:.2f}")

```

</details>
