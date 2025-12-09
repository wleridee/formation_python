# Exercice 01 : Filtrage et Transformation de Données

## Objectif

Cet exercice a pour but de vous faire utiliser les compréhensions de liste pour filtrer et transformer des données de manière concise, en appliquant une condition `if` et une expression.

## Contexte

Vous avez une liste de dictionnaires, chaque dictionnaire représentant un produit avec son nom et son prix. Vous devez effectuer deux tâches :

1.  Extraire uniquement les noms des produits qui coûtent moins de 50.
2.  Créer une nouvelle liste de produits où une réduction de 10% est appliquée à chaque produit, en ajoutant une clé `discounted_price`.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `product_filter.py`.

2.  **Initialisez la liste de produits suivante** :
    ```python
    products = [
        {"name": "Laptop", "price": 1200},
        {"name": "Mouse", "price": 25},
        {"name": "Keyboard", "price": 75},
        {"name": "Monitor", "price": 300},
        {"name": "Webcam", "price": 45},
    ]
    ```

### Tâche 1 : Filtrer les produits abordables

1.  En utilisant une **compréhension de liste avec une condition `if`**, créez une nouvelle liste nommée `affordable_product_names`.
2.  Cette liste doit contenir uniquement les **noms** (`name`) des produits dont le `price` est strictement inférieur à 50.
3.  Affichez le résultat.

### Tâche 2 : Appliquer une réduction

1.  En utilisant une **compréhension de dictionnaire** (ou une compréhension de liste qui crée des dictionnaires), créez une nouvelle liste nommée `discounted_products`.
2.  Chaque élément de cette nouvelle liste doit être un dictionnaire contenant le `name` du produit et un nouveau champ `discounted_price` qui correspond au prix original moins 10%.
3.  Affichez le résultat. Pour une meilleure lisibilité, vous pouvez afficher un produit par ligne.

## Résultat Attendu

```
Affordable Products (< $50):
['Mouse', 'Webcam']

Products with 10% discount:
{'name': 'Laptop', 'discounted_price': 1080.0}
{'name': 'Mouse', 'discounted_price': 22.5}
{'name': 'Keyboard', 'discounted_price': 67.5}
{'name': 'Monitor', 'discounted_price': 270.0}
{'name': 'Webcam', 'discounted_price': 40.5}
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# product_filter.py

products = [
    {"name": "Laptop", "price": 1200},
    {"name": "Mouse", "price": 25},
    {"name": "Keyboard", "price": 75},
    {"name": "Monitor", "price": 300},
    {"name": "Webcam", "price": 45},
]

# --- Tâche 1: Filtrer ---

# [ <expression> for <element> in <iterable> if <condition> ]
# L'expression est p['name']
# L'élément est p
# L'itérable est products
# La condition est p['price'] < 50
affordable_product_names = [p["name"] for p in products if p["price"] < 50]

print("Affordable Products (< $50):")
print(affordable_product_names)


# --- Tâche 2: Transformer ---
print("\nProducts with 10% discount:")

# [ <expression> for <element> in <iterable> ]
# L'expression est un nouveau dictionnaire
# L'élément est p
# L'itérable est products
discounted_products = [
    {"name": p["name"], "discounted_price": p["price"] * 0.9} for p in products
]

# Affichage plus lisible
for product in discounted_products:
    print(product)

```

</details>
