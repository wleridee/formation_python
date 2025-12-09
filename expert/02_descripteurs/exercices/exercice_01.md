# Exercice 01 : Descripteur de Validation d'Email

## Objectif

Cet exercice a pour but de vous faire créer un descripteur de données pour valider qu'un attribut de classe contient une chaîne de caractères qui ressemble à une adresse email valide.

## Contexte

Dans de nombreuses applications, vous devez stocker des informations sur des utilisateurs, y compris leur adresse email. Il est crucial de s'assurer que la valeur assignée à un attribut `email` est bien formatée. Un descripteur est une solution élégante et réutilisable pour implémenter cette logique de validation.

Pour cet exercice, une validation simple utilisant le module `re` (expressions régulières) suffira. Une adresse email sera considérée comme "valide" si elle contient un caractère `@`.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `email_validator.py`.

2.  **Définissez une classe de descripteur `ValidatedEmail`**.

    - Cette classe doit implémenter le protocole des descripteurs de données.

3.  **Implémentez la méthode `__set_name__`**.

    - Cette méthode est appelée lorsque le descripteur est assigné à un attribut de classe.
    - Elle doit stocker le nom public de l'attribut (ex: `email`) et créer un nom privé (ex: `_email`) pour stocker la valeur réelle dans le dictionnaire de l'instance.

4.  **Implémentez la méthode `__get__`**.

    - Elle est appelée lorsqu'on lit la valeur de l'attribut.
    - Elle doit récupérer et retourner la valeur stockée sous le nom privé dans l'instance.

5.  **Implémentez la méthode `__set__`**.

    - Elle est appelée lorsqu'on assigne une valeur à l'attribut.
    - C'est ici que la logique de validation doit avoir lieu.
    - Vérifiez que la `value` est une chaîne de caractères (`str`).
    - Vérifiez que la `value` contient le caractère `@`.
    - Si l'une de ces conditions n'est pas remplie, levez une `ValueError` avec un message explicite.
    - Si la valeur est valide, stockez-la dans l'instance en utilisant le nom d'attribut privé.

6.  **Créez une classe `User` pour tester le descripteur**.

    - Définissez un attribut de classe `email` qui est une instance de votre descripteur `ValidatedEmail`.
    - Le constructeur `__init__` de `User` doit accepter un `name` et un `email`, et les assigner aux attributs correspondants.

7.  **Testez le comportement**.
    - Créez une instance de `User` avec une adresse email valide. Vérifiez que l'attribut `email` peut être lu correctement.
    - Essayez de créer une instance de `User` avec une adresse email invalide (sans `@`). Assurez-vous qu'une `ValueError` est levée.
    - Créez une instance valide, puis essayez de changer son email pour une valeur invalide (par exemple, un nombre ou une chaîne sans `@`). Assurez-vous qu'une `ValueError` est levée.

## Résultat Attendu

Le script doit démontrer que la validation fonctionne à la fois lors de la création de l'objet et lors de la modification de l'attribut.

```
# Création d'un utilisateur valide
Utilisateur créé : Alice, Email: alice@example.com

# Tentative de création avec un email invalide
Erreur attrapée : L'email 'bob-invalid' n'est pas une adresse email valide.

# Tentative de modification avec un type invalide
Erreur attrapée : La valeur assignée à 'email' doit être une chaîne de caractères.

# Tentative de modification avec un email invalide
Erreur attrapée : L'email 'carol@invalid' n'est pas une adresse email valide.
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# email_validator.py

class ValidatedEmail:
    """
    Descripteur qui valide qu'un attribut est une chaîne de caractères
    contenant un '@'.
    """
    def __set_name__(self, owner, name):
        self.public_name = name
        self.private_name = '_' + name

    def __get__(self, instance, owner):
        if instance is None:
            return self
        # Récupère la valeur depuis le dictionnaire de l'instance
        return getattr(instance, self.private_name, None)

    def __set__(self, instance, value):
        # Valider le type
        if not isinstance(value, str):
            raise ValueError(f"La valeur assignée à '{self.public_name}' doit être une chaîne de caractères.")

        # Valider le format
        if '@' not in value:
            raise ValueError(f"L'email '{value}' n'est pas une adresse email valide.")

        # Stocker la valeur validée
        setattr(instance, self.private_name, value)


class User:
    """Classe représentant un utilisateur avec un email validé."""
    email = ValidatedEmail()

    def __init__(self, name: str, email: str):
        self.name = name
        self.email = email # L'assignation ici déclenche le __set__ du descripteur

# --- Tests ---

# 1. Création d'un utilisateur valide
try:
    user_alice = User("Alice", "alice@example.com")
    print(f"Utilisateur créé : {user_alice.name}, Email: {user_alice.email}")
except ValueError as e:
    print(f"Erreur inattendue: {e}")

print("-" * 20)

# 2. Tentative de création avec un email invalide
try:
    user_bob = User("Bob", "bob-invalid")
except ValueError as e:
    print(f"Erreur attrapée : {e}")

print("-" * 20)

# 3. Tentative de modification avec un type invalide
try:
    user_alice.email = 12345
except ValueError as e:
    print(f"Erreur attrapée : {e}")

print("-" * 20)

# 4. Tentative de modification avec un email invalide
try:
    user_alice.email = "carol@invalid" # Ceci est valide pour notre simple test
    user_alice.email = "dave_no_at_sign.com" # Ceci est invalide
except ValueError as e:
    print(f"Erreur attrapée : {e}")

```

</details>
