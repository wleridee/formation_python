# Exercice 01 : Annotation d'une Fonction de Traitement de Données

## Objectif

Cet exercice a pour but de vous faire pratiquer l'ajout d'annotations de type à une fonction existante et d'utiliser `mypy` pour vérifier la correction de votre code.

## Contexte

Vous disposez d'une fonction qui traite une liste de dictionnaires représentant des utilisateurs. Chaque dictionnaire contient un nom (chaîne), un âge (entier) et une liste d'emails (qui peut être vide). La fonction doit filtrer les utilisateurs pour ne garder que ceux qui sont majeurs (18 ans ou plus) et qui ont au moins un email.

Votre tâche est d'ajouter des annotations de type précises à cette fonction et de vérifier votre travail avec `mypy`.

## Énoncé

### Étape 1 : Créer le fichier et la fonction

Créez un fichier nommé `process_users.py` et copiez-y le code suivant :

```python
# process_users.py

def process_users(users):
    """
    Filtre une liste d'utilisateurs pour ne garder que les adultes avec au moins un email.
    """
    processed_list = []
    for user in users:
        if user['age'] >= 18 and user['emails']:
            processed_list.append(user)
    return processed_list

# --- Données de test ---
sample_users = [
    {
        "name": "Alice",
        "age": 30,
        "emails": ["alice@example.com"]
    },
    {
        "name": "Bob",
        "age": 17,
        "emails": ["bob@example.com"]
    },
    {
        "name": "Charlie",
        "age": 40,
        "emails": [] # Pas d'email
    },
    {
        "name": "David",
        "age": 25,
        "emails": ["david@example.com", "d.d@work.com"]
    }
]

if __name__ == "__main__":
    adult_users = process_users(sample_users)
    print("Utilisateurs adultes avec email :")
    for user in adult_users:
        print(f"- {user['name']} (Age: {user['age']})")
```

### Étape 2 : Ajouter les annotations de type

1.  **Importez les types nécessaires** depuis le module `typing`. Vous aurez besoin de `List` et `Dict`. Pour représenter un dictionnaire avec des clés de type `str` et des valeurs de n'importe quel type, vous pouvez utiliser `Dict[str, any]`. Cependant, pour être plus précis, on peut définir la structure de l'utilisateur.
    _Pour cet exercice, nous allons utiliser une approche simple avec `Dict[str, any]` d'abord, puis une plus avancée._

2.  **Annotez la fonction `process_users`**.

    - L'argument `users` est une liste de dictionnaires. Annotez-le en conséquence.
    - La fonction retourne également une liste de dictionnaires. Annotez la valeur de retour.

3.  **(Optionnel, plus avancé) Créez un alias de type pour l'utilisateur**.
    Pour améliorer la lisibilité, vous pouvez définir un alias de type pour la structure d'un dictionnaire utilisateur en utilisant `TypeAlias` (Python 3.10+) ou une simple assignation.

    ```python
    from typing import TypeAlias, List, Dict, Union

    # Pour être très précis
    UserDict: TypeAlias = Dict[str, Union[str, int, List[str]]]
    ```

    Utilisez ensuite `UserDict` dans vos annotations.

### Étape 3 : Vérifier avec `mypy`

1.  **Installez `mypy`** si ce n'est pas déjà fait :

    ```bash
    pip install mypy
    ```

2.  **Exécutez `mypy`** sur votre fichier :

    ```bash
    mypy process_users.py
    ```

    Si vos annotations sont correctes, `mypy` devrait afficher : `Success: no issues found in 1 source file`.

3.  **Introduisez une erreur de type** pour voir `mypy` en action.
    - Par exemple, essayez d'appeler `process_users` avec une liste de chaînes de caractères au lieu d'une liste de dictionnaires.
    ```python
    # Ajoutez cette ligne à la fin de votre bloc if __name__ == "__main__":
    process_users(["user1", "user2"])
    ```
    - Relancez `mypy`. Il devrait maintenant signaler une erreur.

## Résultat Attendu

Votre fichier `process_users.py` doit être entièrement annoté. `mypy` ne doit trouver aucune erreur sur la version correcte du code, mais doit en trouver une lorsque vous introduisez un appel avec un type incorrect.

**Sortie de `mypy` après avoir introduit l'erreur :**

```
process_users.py:45: error: Argument 1 to "process_users" has incompatible type "List[str]"; expected "List[Dict[str, object]]"  [arg-type]
Found 1 error in 1 file (checked 1 source file)
```

_(Note : Le type attendu peut varier légèrement selon la précision de votre annotation, par exemple `List[Dict[str, Any]]` ou `List[UserDict]`)._

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# process_users.py

from typing import List, Dict, Any, Union

# On peut définir un alias pour rendre le code plus lisible.
# Union[...] permet de spécifier les différents types possibles pour les valeurs.
User = Dict[str, Union[str, int, List[str]]]

def process_users(users: List[User]) -> List[User]:
    """
    Filtre une liste d'utilisateurs pour ne garder que les adultes avec au moins un email.
    """
    processed_list: List[User] = []
    for user in users:
        # mypy peut inférer que user['age'] est un int et user['emails'] une liste,
        # mais seulement si l'annotation de User est précise.
        # Si on avait utilisé Dict[str, Any], mypy ne pourrait pas être sûr.
        if user['age'] >= 18 and user['emails']:
            processed_list.append(user)
    return processed_list

# --- Données de test ---
sample_users: List[User] = [
    {
        "name": "Alice",
        "age": 30,
        "emails": ["alice@example.com"]
    },
    {
        "name": "Bob",
        "age": 17,
        "emails": ["bob@example.com"]
    },
    {
        "name": "Charlie",
        "age": 40,
        "emails": [] # Pas d'email
    },
    {
        "name": "David",
        "age": 25,
        "emails": ["david@example.com", "d.d@work.com"]
    }
]

if __name__ == "__main__":
    adult_users = process_users(sample_users)
    print("Utilisateurs adultes avec email :")
    for user in adult_users:
        print(f"- {user['name']} (Age: {user['age']})")

    # --- Ligne pour introduire une erreur de type ---
    # Décommentez la ligne suivante pour voir mypy lever une erreur
    # process_users(["user1", "user2"])
```

**Annotation alternative plus simple (mais moins précise) :**
Si vous ne voulez pas définir un type `User` complexe, vous pouvez utiliser `Any` ou `object`.

```python
from typing import List, Dict, Any

def process_users(users: List[Dict[str, Any]]) -> List[Dict[str, Any]]:
    # ...
```

Cette version est aussi valide pour `mypy`, mais elle offre moins de sécurité à l'intérieur de la fonction, car `mypy` ne connaîtra pas le type des valeurs comme `user['age']`.

</details>
