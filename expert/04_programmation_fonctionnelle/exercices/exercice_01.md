# Exercice 01 : Pipeline de Traitement de Données Fonctionnel

## Objectif

Cet exercice a pour but de vous faire utiliser les outils de programmation fonctionnelle (`map`, `filter`, `reduce`) et la composition de fonctions pour créer un pipeline de traitement de données simple et lisible.

## Contexte

Vous disposez d'une liste de dictionnaires représentant des transactions commerciales. Chaque dictionnaire contient l'ID de la transaction, le montant et le pays. Votre objectif est de calculer le revenu total des transactions provenant d'un pays spécifique (par exemple, "USA") et dont le montant est supérieur à un certain seuil.

Vous allez résoudre ce problème en créant une série de petites fonctions pures et en les combinant.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `functional_pipeline.py`.

2.  **Définissez les données de départ**.
    Créez une liste de dictionnaires `transactions` comme celle-ci :

    ```python
    transactions = [
        {"id": 1, "country": "USA", "amount": 150.0},
        {"id": 2, "country": "France", "amount": 200.0},
        {"id": 3, "country": "USA", "amount": 75.0},
        {"id": 4, "country": "UK", "amount": 300.0},
        {"id": 5, "country": "USA", "amount": 225.0},
        {"id": 6, "country": "France", "amount": 50.0},
    ]
    ```

3.  **Créez des fonctions de filtrage**.

    - Créez une fonction `is_from_country(transaction, country)` qui retourne `True` si la transaction vient du pays spécifié.
    - Créez une fonction `is_above_threshold(transaction, threshold)` qui retourne `True` si le montant de la transaction est supérieur au seuil.

4.  **Créez une fonction d'extraction**.

    - Créez une fonction `get_amount(transaction)` qui retourne le montant de la transaction.

5.  **Utilisez `functools.partial` pour spécialiser vos fonctions**.

    - Créez une fonction `is_from_usa` en utilisant `partial` sur `is_from_country`.
    - Créez une fonction `is_above_100` en utilisant `partial` sur `is_above_threshold`.

6.  **Construisez le pipeline**.

    - Utilisez `filter` et `is_from_usa` pour ne garder que les transactions des USA.
    - Enchaînez avec un autre `filter` et `is_above_100` pour filtrer par montant.
    - Utilisez `map` et `get_amount` pour extraire les montants des transactions restantes.
    - Utilisez `functools.reduce` pour sommer les montants et obtenir le total. Vous pouvez aussi utiliser `sum()` qui est souvent plus simple. Pour cet exercice, essayez avec `reduce`.

7.  **Affichez le résultat final**.

## Résultat Attendu

Le script doit calculer et afficher le revenu total des transactions des USA dont le montant est supérieur à 100.

```
Transactions initiales:
[{'id': 1, 'country': 'USA', 'amount': 150.0}, {'id': 2, 'country': 'France', 'amount': 200.0}, ...]

Transactions des USA:
<filter object at ...>

Transactions des USA > 100:
<filter object at ...>

Montants extraits:
<map object at ...>

Revenu total des transactions éligibles : 375.0
```

(Les transactions des USA > 100 sont celles avec les ID 1 (150.0) et 5 (225.0). La somme est 150.0 + 225.0 = 375.0)

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# functional_pipeline.py

from functools import partial, reduce

# 1. Données de départ
transactions = [
    {"id": 1, "country": "USA", "amount": 150.0},
    {"id": 2, "country": "France", "amount": 200.0},
    {"id": 3, "country": "USA", "amount": 75.0},
    {"id": 4, "country": "UK", "amount": 300.0},
    {"id": 5, "country": "USA", "amount": 225.0},
    {"id": 6, "country": "France", "amount": 50.0},
]

# 2. Fonctions pures et génériques
def is_from_country(transaction, country):
    return transaction["country"] == country

def is_above_threshold(transaction, threshold):
    return transaction["amount"] > threshold

def get_amount(transaction):
    return transaction["amount"]

def add(x, y):
    return x + y

# 3. Spécialisation des fonctions avec partial
is_from_usa = partial(is_from_country, country="USA")
is_above_100 = partial(is_above_threshold, threshold=100)

# 4. Construction du pipeline
print(f"Transactions initiales:\n{transactions}\n")

# Étape 1: Filtrer par pays
usa_transactions = filter(is_from_usa, transactions)

# Étape 2: Filtrer par montant
eligible_transactions = filter(is_above_100, usa_transactions)

# Étape 3: Extraire les montants
amounts = map(get_amount, eligible_transactions)

# Étape 4: Réduire (sommer) les montants
# Note: sum(amounts) serait plus simple et plus idiomatique en Python.
# Nous utilisons reduce ici pour l'exercice.
total_revenue = reduce(add, amounts, 0)

print(f"Revenu total des transactions éligibles : {total_revenue}")

# --- Version alternative en une seule ligne (moins lisible) ---
total_revenue_oneline = reduce(add, map(get_amount, filter(is_above_100, filter(is_from_usa, transactions))))
# print(f"Total en une ligne : {total_revenue_oneline}")

```

</details>
