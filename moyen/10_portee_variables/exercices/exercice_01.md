# Exercice 01 : Créateur de Compteur avec `nonlocal`

## Objectif

Cet exercice a pour but de vous faire utiliser le mot-clé `nonlocal` pour créer une "closure" qui agit comme un compteur. Cela illustre comment une fonction interne peut modifier l'état de sa fonction parente.

## Contexte

Une "closure" (fermeture) est une fonction qui se souvient de l'environnement dans lequel elle a été créée. En d'autres termes, elle a accès aux variables de la fonction qui l'a définie, même après que cette fonction parente a terminé son exécution.

En utilisant `nonlocal`, on peut créer des fonctions qui maintiennent un état interne, ce qui est une alternative légère à l'utilisation d'une classe pour des cas simples.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `counter_factory.py`.

2.  **Définissez une fonction "factory" `creer_compteur`**.

    - Cette fonction ne prend pas d'arguments.
    - À l'intérieur de `creer_compteur`, initialisez une variable `compte` à `0`. C'est la variable de la portée englobante (Enclosing).

3.  **Définissez une fonction interne `incrementer`**.

    - Cette fonction doit être définie _à l'intérieur_ de `creer_compteur`.
    - Elle ne prend pas d'arguments.
    - Utilisez le mot-clé `nonlocal` pour déclarer que vous voulez modifier la variable `compte` de la fonction `creer_compteur`.
    - Incrémentez `compte` de 1.
    - La fonction `incrementer` doit retourner la nouvelle valeur de `compte`.

4.  **La fonction `creer_compteur` doit retourner la fonction `incrementer`**.

5.  **Testez votre créateur de compteurs**.
    - Dans le bloc principal (`if __name__ == '__main__':`), créez un premier compteur en appelant `creer_compteur()`.
    - Appelez ce premier compteur plusieurs fois et affichez le résultat pour vérifier qu'il s'incrémente correctement.
    - Créez un _deuxième_ compteur en appelant à nouveau `creer_compteur()`.
    - Appelez ce deuxième compteur et vérifiez que son état est indépendant du premier.

## Résultat Attendu

L'exécution de votre script de test doit montrer que chaque compteur maintient son propre état, indépendant des autres.

```
--- Compteur 1 ---
1
2
3

--- Compteur 2 ---
1

--- Compteur 1 (suite) ---
4
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# counter_factory.py

def creer_compteur():
    """
    Une fonction factory qui crée et retourne une fonction compteur.
    Chaque compteur a son propre état indépendant.
    """
    # E - Portée Englobante (Enclosing Scope)
    compte = 0

    def incrementer():
        """
        Fonction interne qui incrémente et retourne la variable 'compte'
        de sa portée englobante.
        """
        # L - Portée Locale (Local Scope)

        # On déclare que 'compte' n'est pas une nouvelle variable locale,
        # mais bien celle de la portée englobante.
        nonlocal compte

        compte += 1
        return compte

    # La factory retourne la fonction interne.
    return incrementer

# --- Tests ---
if __name__ == "__main__":
    # Crée une première instance de compteur.
    compteur1 = creer_compteur()

    print("--- Compteur 1 ---")
    print(compteur1())
    print(compteur1())
    print(compteur1())

    # Crée une deuxième instance. Elle aura son propre 'compte'.
    compteur2 = creer_compteur()

    print("\n--- Compteur 2 ---")
    print(compteur2())

    print("\n--- Compteur 1 (suite) ---")
    # On vérifie que l'état de compteur1 n'a pas été affecté.
    print(compteur1())

```

</details>
