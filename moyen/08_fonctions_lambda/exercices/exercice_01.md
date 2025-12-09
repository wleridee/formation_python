# Exercice 01 : Tri de Données Complexes

## Objectif

Cet exercice a pour but de vous faire utiliser des fonctions `lambda` comme clé de tri (`key`) avec la fonction `sorted()` pour trier une liste de dictionnaires selon différents critères.

## Contexte

Vous avez une liste de dictionnaires représentant des produits. Chaque produit a un nom, un prix et une note de popularité. Vous devez trier cette liste de différentes manières pour l'afficher sur un site e-commerce.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `product_sorter.py`.

2.  **Initialisez la liste de produits suivante** :

    ```python
    products = [
        {"name": "Laptop", "price": 1200, "popularity": 8},
        {"name": "Mouse", "price": 25, "popularity": 10},
        {"name": "Keyboard", "price": 75, "popularity": 9},
        {"name": "Monitor", "price": 300, "popularity": 7},
    ]
    ```

3.  **Triez les produits par prix (du moins cher au plus cher)** :

    - Utilisez la fonction `sorted()` avec une fonction `lambda` comme `key`. La lambda doit extraire le `price` de chaque dictionnaire.
    - Stockez le résultat dans une variable `sorted_by_price`.
    - Affichez le résultat de manière lisible.

4.  **Triez les produits par popularité (du plus populaire au moins populaire)** :

    - Utilisez à nouveau `sorted()`, mais cette fois, la `lambda` doit extraire la `popularity`.
    - Pour trier en ordre décroissant, utilisez l'argument `reverse=True` de la fonction `sorted()`.
    - Stockez le résultat dans `sorted_by_popularity`.
    - Affichez le résultat.

5.  **Triez les produits par la longueur de leur nom (du plus court au plus long)** :
    - Utilisez `sorted()` et une `lambda` qui calcule la longueur (`len()`) du `name` de chaque produit.
    - Stockez le résultat dans `sorted_by_name_length`.
    - Affichez le résultat.

## Résultat Attendu

```
--- Sorted by Price (Ascending) ---
[{'name': 'Mouse', 'price': 25, 'popularity': 10}, {'name': 'Keyboard', 'price': 75, 'popularity': 9}, {'name': 'Monitor', 'price': 300, 'popularity': 7}, {'name': 'Laptop', 'price': 1200, 'popularity': 8}]

--- Sorted by Popularity (Descending) ---
[{'name': 'Mouse', 'price': 25, 'popularity': 10}, {'name': 'Keyboard', 'price': 75, 'popularity': 9}, {'name': 'Laptop', 'price': 1200, 'popularity': 8}, {'name': 'Monitor', 'price': 300, 'popularity': 7}]

--- Sorted by Name Length (Ascending) ---
[{'name': 'Mouse', 'price': 25, 'popularity': 10}, {'name': 'Laptop', 'price': 1200, 'popularity': 8}, {'name': 'Monitor', 'price': 300, 'popularity': 7}, {'name': 'Keyboard', 'price': 75, 'popularity': 9}]
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# product_sorter.py

products = [
    {"name": "Laptop", "price": 1200, "popularity": 8},
    {"name": "Mouse", "price": 25, "popularity": 10},
    {"name": "Keyboard", "price": 75, "popularity": 9},
    {"name": "Monitor", "price": 300, "popularity": 7},
]

# 1. Tri par prix (croissant)
# La lambda prend un produit 'p' et retourne son prix p['price'].
sorted_by_price = sorted(products, key=lambda p: p["price"])
print("--- Sorted by Price (Ascending) ---")
print(sorted_by_price)

# 2. Tri par popularité (décroissant)
# La lambda prend un produit 'p' et retourne sa popularité p['popularity'].
# L'argument reverse=True inverse l'ordre du tri.
sorted_by_popularity = sorted(products, key=lambda p: p["popularity"], reverse=True)
print("\n--- Sorted by Popularity (Descending) ---")
print(sorted_by_popularity)

# 3. Tri par longueur du nom (croissant)
# La lambda prend un produit 'p' et retourne la longueur de son nom len(p['name']).
sorted_by_name_length = sorted(products, key=lambda p: len(p["name"]))
print("\n--- Sorted by Name Length (Ascending) ---")
print(sorted_by_name_length)

```

</details>
