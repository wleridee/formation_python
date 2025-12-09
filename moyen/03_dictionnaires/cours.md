# Module : Les Dictionnaires

## 1. Quoi : Les Dictionnaires

Un **dictionnaire** (`dict`) est une collection **non ordonnée** (avant Python 3.7) et **muable** d'éléments stockés sous forme de paires **clé-valeur**.

- **Paire clé-valeur** : Chaque élément d'un dictionnaire est composé d'une clé unique et de la valeur qui lui est associée.
- **Clé unique** : Les clés doivent être uniques et de type immuable (ex: `str`, `int`, `tuple`). Elles servent à accéder rapidement à leur valeur correspondante.
- **Muable** : Vous pouvez ajouter, modifier et supprimer des paires clé-valeur.
- **Ordonné (depuis Python 3.7)** : Les dictionnaires conservent l'ordre d'insertion des éléments.

## 2. Pourquoi : Stocker des données structurées

Les dictionnaires sont parfaits pour représenter des objets du monde réel ou des données structurées où chaque information a une étiquette.

- Un utilisateur : clé "nom", valeur "Alice"; clé "age", valeur 30.
- Un produit : clé "id", valeur 123; clé "price", valeur 19.99.
- Des données de configuration.

Ils permettent un accès quasi instantané aux valeurs via leurs clés, ce qui est beaucoup plus efficace que de chercher dans une liste.

## 3. Comment : Création et Manipulation

### A. Création

On crée un dictionnaire avec des accolades `{}`, en séparant les paires clé-valeur par des virgules. La clé et la valeur sont séparées par deux-points `:`.

```python
# Un dictionnaire représentant un utilisateur
user = {
    "name": "Alice",
    "age": 30,
    "is_admin": False,
    "city": "Paris"
}

# Un dictionnaire vide
empty_dict = {}
```

### B. Accès aux valeurs

- **Avec des crochets `[]`** : Simple et direct. Lève une erreur `KeyError` si la clé n'existe pas.
- **Avec la méthode `.get(key, default)`** : Plus sûr. Retourne `None` (ou une valeur par défaut spécifiée) si la clé n'existe pas, sans lever d'erreur.

```python
user = {"name": "Alice", "age": 30}

# Accès direct
print(user["name"])  # "Alice"
# print(user["country"]) # ❌ Lèvera une KeyError

# Accès sécurisé avec .get()
print(user.get("age")) # 30
print(user.get("country")) # None (pas d'erreur)
print(user.get("country", "Unknown")) # "Unknown" (valeur par défaut)
```

✅ **Bonne pratique** : Utilisez `.get()` lorsque vous n'êtes pas sûr si une clé existe.

### C. Ajout et Modification

L'assignation via une clé permet à la fois d'ajouter une nouvelle paire et de modifier une paire existante.

```python
user = {"name": "Alice"}

# Modifier une valeur existante
user["name"] = "Alicia"

# Ajouter une nouvelle paire clé-valeur
user["age"] = 31
user["city"] = "Lyon"

print(user) # {'name': 'Alicia', 'age': 31, 'city': 'Lyon'}
```

### D. Suppression d'éléments

- `del dict[key]` : Supprime la paire clé-valeur. Lève une `KeyError` si la clé n'existe pas.
- `.pop(key, default)` : Supprime la paire et **retourne la valeur**. Plus sûr, car on peut fournir une valeur par défaut si la clé n'existe pas.

```python
user = {"name": "Bob", "age": 40, "city": "New York"}

# Supprimer avec del
del user["city"]

# Supprimer et récupérer la valeur avec .pop()
removed_age = user.pop("age")
print(f"Removed age: {removed_age}") # Removed age: 40

# Essayer de supprimer une clé qui n'existe pas (sans erreur)
removed_country = user.pop("country", "Not found")
print(f"Removed country: {removed_country}") # Removed country: Not found

print(user) # {'name': 'Bob'}
```

### E. Itération sur les dictionnaires

Il y a trois manières principales d'itérer.

```python
user = {"name": "Charles", "age": 25}

# 1. Itérer sur les clés (comportement par défaut)
print("--- Keys ---")
for key in user:
    print(key)

# 2. Itérer sur les valeurs
print("--- Values ---")
for value in user.values():
    print(value)

# 3. Itérer sur les paires clé-valeur (le plus courant et le plus utile)
print("--- Items ---")
for key, value in user.items():
    print(f"{key} -> {value}")
```

### F. Fonctions et Opérateurs Utiles

- `len(dict)` : Retourne le nombre de paires clé-valeur.
- `key in dict` : Vérifie si une clé existe dans le dictionnaire (très rapide).

```python
user = {"name": "David", "age": 28}

print(len(user)) # 2
print("age" in user) # True
print("email" in user) # False
```

✅ **Bonne pratique** : `key in dict` est la manière "pythonic" de vérifier l'existence d'une clé.
