# Exercice 01 : Génération d'un Reçu Formaté

## Objectif

Cet exercice a pour but de vous faire utiliser les f-strings pour créer une chaîne de caractères formatée qui ressemble à un reçu de caisse, en utilisant des calculs et des options de formatage.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `receipt_generator.py`.

2.  **Déclarez les variables suivantes** au début de votre script :

    ```python
    product_name = "Laptop"
    quantity = 2
    unit_price = 1200.50
    ```

3.  **Effectuez le calcul suivant** et stockez le résultat dans une variable `total_price` :

    - Calculez le prix total en multipliant la quantité par le prix unitaire.

4.  **Générez le reçu** :

    - En utilisant **une seule f-string multi-lignes** (`f"""..."""`), créez une chaîne de caractères qui ressemble au reçu ci-dessous.
    - **Contraintes de formatage** :
      - Le prix unitaire et le prix total doivent être affichés avec **deux décimales**.
      - Les noms des champs (`Product`, `Quantity`, etc.) doivent être alignés à gauche sur 10 caractères.
      - Les valeurs correspondantes doivent être alignées à droite sur 15 caractères.

5.  **Affichez le reçu** en utilisant `print()`.

## Résultat Attendu

La sortie de votre script doit être exactement la suivante :

```
***************************
*      STORE RECEIPT      *
***************************
Product   :          Laptop
Quantity  :               2
Unit Price:        1200.50
---------------------------
Total     :        2401.00
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# receipt_generator.py

# 1. Variables
product_name = "Laptop"
quantity = 2
unit_price = 1200.50

# 2. Calcul
total_price = quantity * unit_price

# 3. Génération du reçu avec une f-string multi-lignes
receipt = f"""
***************************
*      STORE RECEIPT      *
***************************
{'Product':<10}: {product_name:>15}
{'Quantity':<10}: {quantity:>15}
{'Unit Price':<10}: {unit_price:>15.2f}
---------------------------
{'Total':<10}: {total_price:>15.2f}
"""

# 4. Affichage
print(receipt)

```

**Explication des options de formatage :**

- `{'Product':<10}` : Aligne le mot "Product" à gauche (`<`) sur un total de 10 caractères.
- `{product_name:>15}` : Aligne la valeur de `product_name` à droite (`>`) sur un total de 15 caractères.
- `{unit_price:>15.2f}` : Combine les options :
  - `>` : Aligne à droite.
  - `15` : Sur 15 caractères au total.
  - `.2f` : Formate le nombre flottant (`f`) pour n'avoir que 2 chiffres après la virgule.

</details>
