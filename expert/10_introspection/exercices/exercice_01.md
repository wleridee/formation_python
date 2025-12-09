# Exercice 01 : Un "Pretty Printer" d'Objet avec Introspection

## Objectif

Cet exercice a pour but de vous faire utiliser les fonctions d'introspection de base (`dir`, `hasattr`, `getattr`) pour créer une fonction qui peut afficher de manière lisible les attributs et leurs valeurs pour n'importe quel objet.

## Contexte

Lorsque vous déboguez, il est souvent utile d'afficher l'état d'un objet. La fonction `print()` de base sur un objet personnalisé n'est pas très informative (elle affiche quelque chose comme `<__main__.MaClasse object at 0x...>`).

Vous allez écrire une fonction `pretty_print(obj)` qui parcourt les attributs d'un objet et les affiche joliment, en ignorant les méthodes et les attributs "magiques" (ceux qui commencent et finissent par `__`).

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `pretty_printer.py`.

2.  **Définissez une classe de test `Utilisateur`**.

    - Le constructeur `__init__` doit prendre un `nom`, un `email` et un `age`.
    - La classe doit aussi avoir un attribut de classe, par exemple `domaine = "example.com"`.
    - Définissez une méthode simple, par exemple `saluer()`, qui retourne une chaîne de caractères.

3.  **Définissez la fonction `pretty_print(obj)`**.

    - Elle prend un objet `obj` en argument.
    - Commencez par afficher le nom de la classe de l'objet. Vous pouvez l'obtenir avec `obj.__class__.__name__`.
    - Utilisez `dir(obj)` pour obtenir la liste de tous les attributs et méthodes.
    - Itérez sur chaque `nom_attribut` dans la liste retournée par `dir()`.
    - Pour chaque `nom_attribut`, vous devez filtrer pour ne garder que les attributs "publics" :
      - Ignorez les attributs qui commencent par un underscore `_`. C'est une manière simple d'ignorer les attributs "magiques" (`__...__`) et "protégés" (`_...`).
      - Récupérez la valeur de l'attribut en utilisant `getattr(obj, nom_attribut)`.
      - Vérifiez que l'attribut n'est pas une méthode. Une manière simple de le faire est de vérifier que la valeur n'est pas "appelable" (callable) avec la fonction `callable()`.
    - Si un attribut passe tous ces filtres, affichez son nom et sa valeur dans un format lisible, par exemple `  - nom : valeur`.

4.  **Testez votre fonction**.
    - Dans le bloc `if __name__ == '__main__':`, créez une instance de votre classe `Utilisateur`.
    - Appelez `pretty_print()` avec cette instance.
    - Pour montrer que votre fonction est générique, appelez-la aussi avec un autre type d'objet, par exemple une simple instance de `object` à laquelle vous avez ajouté des attributs dynamiquement.

## Résultat Attendu

L'exécution de votre script doit produire une sortie claire et formatée qui liste uniquement les attributs de données publics de l'objet, en ignorant les méthodes et les attributs spéciaux.

```
--- Affichage de l'objet Utilisateur ---
Classe: Utilisateur
  - age : 30
  - domaine : example.com
  - email : alice@email.com
  - nom : Alice

--- Affichage de l'objet générique ---
Classe: object
  - couleur : bleu
  - valeur : 123
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# pretty_printer.py

def pretty_print(obj):
    """
    Affiche les attributs de données publics d'un objet de manière lisible,
    en utilisant l'introspection.
    """
    if obj is None:
        print("Objet est None")
        return

    # Affiche le nom de la classe de l'objet
    print(f"Classe: {obj.__class__.__name__}")

    # Parcourt tous les noms d'attributs et de méthodes
    for attr_name in dir(obj):
        # Filtre 1: Ignorer les attributs qui commencent par '_'
        if not attr_name.startswith('_'):
            # Récupère la valeur de l'attribut
            try:
                attr_value = getattr(obj, attr_name)
            except AttributeError:
                continue # Peut arriver dans des cas complexes

            # Filtre 2: Ignorer les méthodes (ou tout ce qui est appelable)
            if not callable(attr_value):
                print(f"  - {attr_name} : {attr_value}")

# --- Classe de test ---
class Utilisateur:
    """Une classe simple pour les tests."""
    domaine = "example.com"

    def __init__(self, nom: str, email: str, age: int):
        self.nom = nom
        self.email = email
        self.age = age
        self._secret = "ceci est un secret" # Attribut protégé, doit être ignoré

    def saluer(self):
        """Une méthode simple, doit être ignorée."""
        return f"Bonjour, je suis {self.nom}."

# --- Tests ---
if __name__ == "__main__":
    # 1. Test avec un objet de notre classe Utilisateur
    user = Utilisateur(nom="Alice", email="alice@email.com", age=30)

    print("--- Affichage de l'objet Utilisateur ---")
    pretty_print(user)

    print("\n" + "="*20 + "\n")

    # 2. Test avec un objet générique
    generic_obj = object()
    # Ajout d'attributs dynamiques
    setattr(generic_obj, 'valeur', 123)
    setattr(generic_obj, 'couleur', 'bleu')

    print("--- Affichage de l'objet générique ---")
    pretty_print(generic_obj)

```

</details>
