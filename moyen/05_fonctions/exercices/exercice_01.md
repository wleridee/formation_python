# Exercice 01 : Validateur de Mot de Passe

## Objectif

Cet exercice a pour but de vous faire créer une fonction qui prend un mot de passe en entrée et retourne un booléen (`True`/`False`) indiquant s'il respecte un ensemble de règles de validation.

## Contexte

Vous devez implémenter une fonction de validation pour un formulaire d'inscription. Un mot de passe est considéré comme valide s'il respecte toutes les règles suivantes :

1.  Il doit faire au moins 8 caractères de long.
2.  Il doit contenir au moins une lettre majuscule.
3.  Il doit contenir au moins un chiffre.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `password_validator.py`.

2.  **Définissez une fonction** nommée `is_password_valid` qui accepte un seul paramètre, `password`.

    - N'oubliez pas d'ajouter une **docstring** pour expliquer ce que fait la fonction.

3.  **À l'intérieur de la fonction**, implémentez la logique de validation :
    a. **Vérifiez la longueur** : La première condition est de vérifier si `len(password)` est supérieur ou égal à 8. Si ce n'est pas le cas, la fonction peut immédiatement retourner `False`.

    b. **Vérifiez la présence d'une majuscule et d'un chiffre** : - Initialisez deux variables booléennes, `has_upper` et `has_digit`, à `False`. - Utilisez une boucle `for` pour itérer sur chaque `char` du `password`. - Dans la boucle, utilisez les méthodes de chaînes de caractères `.isupper()` et `.isdigit()` pour vérifier la nature du caractère. - Si vous trouvez une majuscule, passez `has_upper` à `True`. - Si vous trouvez un chiffre, passez `has_digit` à `True`.

    c. **Retournez le résultat final** : Après la boucle, la fonction doit retourner le résultat de l'expression `has_upper and has_digit`. Si les deux sont `True` (et que la condition de longueur a été passée), le mot de passe est valide.

4.  **Testez votre fonction** :
    - À l'extérieur de la fonction, créez une liste de mots de passe à tester.
    - Utilisez une boucle `for` pour appeler votre fonction `is_password_valid` sur chaque mot de passe et affichez le résultat de manière claire.

## Résultat Attendu

```
Testing 'short': False
Testing 'longpasswordwithoutdigits': False
Testing 'LongPasswordWithoutDigits': False
Testing 'longpasswordwith123': False
Testing 'ValidPass1': True
Testing 'AnotherValidPassword2': True
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# password_validator.py

def is_password_valid(password):
    """
    Checks if a password meets the required criteria.

    A valid password must:
    1. Be at least 8 characters long.
    2. Contain at least one uppercase letter.
    3. Contain at least one digit.

    Args:
        password (str): The password to validate.

    Returns:
        bool: True if the password is valid, False otherwise.
    """
    # 1. Vérification de la longueur
    if len(password) < 8:
        return False

    # 2. Initialisation des "drapeaux"
    has_upper = False
    has_digit = False

    # 3. Itération pour trouver une majuscule et un chiffre
    for char in password:
        if char.isupper():
            has_upper = True
        elif char.isdigit():
            has_digit = True

    # Optimisation : si on a déjà trouvé les deux, on peut arrêter la boucle
    # if has_upper and has_digit:
    #     break

    # 4. Retourner le résultat final
    return has_upper and has_digit

# --- Zone de test ---
passwords_to_test = [
    "short",
    "longpasswordwithoutdigits",
    "LongPasswordWithoutDigits",
    "longpasswordwith123",
    "ValidPass1",
    "AnotherValidPassword2"
]

for pwd in passwords_to_test:
    is_valid = is_password_valid(pwd)
    print(f"Testing '{pwd}': {is_valid}")

```

</details>
