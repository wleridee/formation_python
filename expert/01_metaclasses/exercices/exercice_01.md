# Exercice 01 : Métaclasse pour la Conversion Automatique des Noms de Méthodes

## Objectif

Cet exercice a pour but de vous faire créer une métaclasse qui inspecte les méthodes d'une classe et les convertit automatiquement en minuscules (`lowercase`). Cela illustre comment une métaclasse peut manipuler la structure d'une classe avant sa création finale.

## Contexte

Dans certaines conventions de codage ou pour des raisons de compatibilité, il peut être nécessaire de s'assurer que tous les noms de méthodes d'une API suivent un format spécifique, comme être entièrement en minuscules. Une métaclasse est l'outil parfait pour automatiser cette transformation et garantir la cohérence.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `lowercase_meta.py`.

2.  **Définissez une métaclasse `LowercaseMethodMeta`**.

    - Elle doit hériter de `type`.
    - Surchargez la méthode `__new__(cls, name, bases, attrs)`.

3.  **Dans la méthode `__new__`** :

    - Créez un nouveau dictionnaire, par exemple `new_attrs`, pour stocker les attributs modifiés.
    - Itérez sur les paires clé-valeur (`key`, `value`) du dictionnaire `attrs` original.
    - Pour chaque attribut :
      - Si la clé (`key`) commence par `__` (ce sont des méthodes spéciales comme `__init__`), ajoutez-la au `new_attrs` sans modification.
      - Si la valeur (`value`) est un objet appelable (une fonction/méthode), cela signifie que c'est une méthode que nous voulons transformer. Ajoutez-la au `new_attrs` mais avec son nom de clé converti en minuscules (`key.lower()`).
      - Pour tous les autres attributs (attributs de classe simples), ajoutez-les au `new_attrs` sans modification.
    - Une fois la boucle terminée, appelez la méthode `super().__new__` en lui passant `new_attrs` à la place de `attrs` pour construire la classe avec les noms de méthodes modifiés.
    - Retournez la nouvelle classe créée.

4.  **Créez une classe de test `MyService`** qui utilise cette métaclasse.

    - Définissez plusieurs méthodes avec des noms en casse mixte (CamelCase, PascalCase), par exemple : `ConnectToAPI`, `FetchData`, `process_RESULTS`.
    - Chaque méthode doit simplement retourner une chaîne de caractères indiquant son nom original pour le test.

5.  **Testez le comportement**.
    - Créez une instance de `MyService`.
    - Essayez d'appeler les méthodes en utilisant leurs noms en minuscules (par exemple, `service.connecttoapi()`).
    - Vérifiez que les appels fonctionnent et retournent les bonnes chaînes.
    - Utilisez `hasattr` pour vérifier que les noms de méthodes originaux (en casse mixte) n'existent plus sur l'instance.

## Résultat Attendu

Le script doit s'exécuter sans erreur. Les appels aux méthodes en minuscules doivent réussir, tandis que la vérification de l'existence des méthodes en casse mixte doit renvoyer `False`.

```
# Sortie attendue des print
connecttoapi a été appelée
fetchdata a été appelée
process_results a été appelée

L'attribut 'ConnectToAPI' existe-t-il ? False
L'attribut 'connecttoapi' existe-t-il ? True
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# lowercase_meta.py

import inspect

class LowercaseMethodMeta(type):
    """
    Métaclasse qui convertit tous les noms de méthodes (sauf les spéciales)
    d'une classe en minuscules.
    """
    def __new__(cls, name, bases, attrs):
        print(f"--- Métaclasse : Traitement de la classe '{name}' ---")

        new_attrs = {}
        for key, value in attrs.items():
            if key.startswith("__"):
                # Garder les méthodes spéciales intactes
                new_attrs[key] = value
            elif inspect.isfunction(value):
                # Si c'est une fonction, convertir son nom en minuscules
                print(f"Conversion de la méthode : '{key}' -> '{key.lower()}'")
                new_attrs[key.lower()] = value
            else:
                # Garder les autres attributs intacts
                new_attrs[key] = value

        print("--- Métaclasse : Fin du traitement ---")
        # Créer la classe avec les attributs modifiés
        return super().__new__(cls, name, bases, new_attrs)

# Classe utilisant la métaclasse
class MyService(metaclass=LowercaseMethodMeta):

    def ConnectToAPI(self):
        return "connecttoapi a été appelée"

    def FetchData(self):
        return "fetchdata a été appelée"

    def process_RESULTS(self):
        return "process_results a été appelée"

# --- Tests ---
print("\nCréation de l'instance de service...")
service = MyService()

# Appeler les méthodes avec leurs nouveaux noms en minuscules
print(service.connecttoapi())
print(service.fetchdata())
print(service.process_results())

# Vérifier que les anciens noms n'existent plus
print(f"\nL'attribut 'ConnectToAPI' existe-t-il ? {hasattr(service, 'ConnectToAPI')}")
print(f"L'attribut 'connecttoapi' existe-t-il ? {hasattr(service, 'connecttoapi')}")

```

</details>
