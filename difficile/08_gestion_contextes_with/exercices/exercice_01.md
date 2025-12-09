# Exercice 01 : Gestionnaire de Contexte pour les Connexions à une Base de Données

## Objectif

Cet exercice a pour but de vous faire créer un gestionnaire de contexte simple en utilisant une classe pour simuler la gestion d'une connexion à une base de données. Cela illustre comment `with` peut garantir que les ressources sont correctement ouvertes et fermées.

## Contexte

Lorsque vous travaillez avec des bases de données, il est crucial de s'assurer que chaque connexion ouverte est correctement fermée à la fin, même si des erreurs se produisent. Un gestionnaire de contexte est la solution parfaite pour ce problème.

Vous allez simuler ce comportement en créant une classe `DatabaseConnection` qui pourra être utilisée avec l'instruction `with`.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `db_context_manager.py`.

2.  **Définissez la classe `DatabaseConnection`**.

    - Son constructeur `__init__` doit accepter un paramètre `db_name` (le nom de la base de données).
    - Dans le constructeur, stockez `db_name` et initialisez un attribut `self.is_connected` à `False`.

3.  **Implémentez la méthode `__enter__`**.

    - Cette méthode simule l'ouverture de la connexion.
    - Elle doit afficher un message comme `f"Connecting to database '{self.db_name}'..."`.
    - Elle doit passer `self.is_connected` à `True`.
    - Elle doit se retourner elle-même (`return self`) pour que l'objet puisse être utilisé dans le bloc `with` (avec la syntaxe `as`).

4.  **Implémentez la méthode `__exit__`**.

    - Cette méthode simule la fermeture de la connexion.
    - Elle doit afficher un message comme `f"Closing connection to database '{self.db_name}'..."`.
    - Elle doit passer `self.is_connected` à `False`.
    - Elle ne doit pas supprimer les exceptions (elle doit donc retourner `None` ou `False` implicitement).

5.  **Ajoutez une méthode `query`** à votre classe.

    - Elle prend un argument `sql_query`.
    - Elle doit vérifier si `self.is_connected` est `True`.
    - Si oui, elle affiche `f"Executing query: '{sql_query}'"`.
    - Si non, elle lève une `ConnectionError` avec un message clair.

6.  **Testez votre gestionnaire de contexte** :
    a. Utilisez une instruction `with` pour créer une connexion à une base de données "test_db".
    b. À l'intérieur du bloc `with`, appelez la méthode `query` avec une requête SQL de votre choix.
    c. Après le bloc `with`, essayez d'appeler à nouveau la méthode `query` sur l'objet connexion pour vérifier que la connexion est bien fermée et qu'une erreur est levée. Utilisez un bloc `try...except` pour attraper cette `ConnectionError`.

## Résultat Attendu

```
Connecting to database 'test_db'...
Executing query: 'SELECT * FROM users'
Closing connection to database 'test_db'...
---
Trying to query after closing connection...
Error: Not connected to the database.
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# db_context_manager.py

class DatabaseConnection:
    def __init__(self, db_name):
        self.db_name = db_name
        self.is_connected = False

    def __enter__(self):
        """Opens the database connection."""
        print(f"Connecting to database '{self.db_name}'...")
        self.is_connected = True
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        """Closes the database connection."""
        print(f"Closing connection to database '{self.db_name}'...")
        self.is_connected = False
        # We don't handle exceptions, so we let them propagate by returning None/False.

    def query(self, sql_query):
        """Executes a SQL query."""
        if not self.is_connected:
            raise ConnectionError("Not connected to the database.")
        print(f"Executing query: '{sql_query}'")

# --- Testing ---

# 1. Normal usage with the 'with' statement
with DatabaseConnection("test_db") as db:
    db.query("SELECT * FROM users")

print("---")

# 2. Trying to use the connection outside the 'with' block
print("Trying to query after closing connection...")
# We need to re-create the object to test this, as 'db' from the previous
# block still exists but is in a "closed" state.
db_instance = DatabaseConnection("another_db")
# Let's manually enter and exit to simulate the state after a 'with' block
db_instance.__enter__()
db_instance.__exit__(None, None, None) # Manually close it

try:
    db_instance.query("SELECT * FROM products")
except ConnectionError as e:
    print(f"Error: {e}")

```

</details>
