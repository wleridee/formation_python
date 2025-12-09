# Module : Variables et Types Simples

## 1. Quoi : Les Variables et les Types de Données

- **Une variable** est une "boîte" nommée dans laquelle vous pouvez stocker une valeur. Le nom de la variable vous permet de récupérer ou de modifier cette valeur plus tard.
- **Un type de donnée** définit la nature de la valeur stockée (est-ce un nombre, du texte, un booléen ?).

En Python, la création d'une variable se fait par simple assignation :

```python
# La variable 'age' est créée et la valeur 30 lui est assignée.
# Python déduit que le type de 'age' est un nombre entier (integer).
age = 30

# La variable 'name' est créée et la valeur "Alice" lui est assignée.
# Python déduit que le type de 'name' est une chaîne de caractères (string).
name = "Alice"
```

## 2. Pourquoi : L'utilité des variables

Les variables sont fondamentales en programmation. Elles permettent de :

- **Stocker de l'information** : Garder en mémoire des données qui peuvent changer au cours de l'exécution du programme (ex: le score d'un joueur, le nom de l'utilisateur).
- **Rendre le code lisible** : Utiliser un nom de variable explicite (`user_age` au lieu de `ua`) rend le code beaucoup plus facile à comprendre.
- **Éviter la répétition** : Si vous utilisez une valeur à plusieurs endroits, stockez-la dans une variable. Si la valeur doit changer, vous n'aurez à la modifier qu'à un seul endroit.

## 3. Comment : Les Types de Données Simples

Python dispose de plusieurs types de données intégrés ("built-in"). Voici les plus simples :

### A. Les Nombres

- **`int` (Integer)** : Les nombres entiers, positifs ou négatifs.
  ```python
  user_count = 100
  temperature = -5
  ```
- **`float` (Floating-point)** : Les nombres à virgule.
  ```python
  price = 19.99
  pi_approximation = 3.14159
  ```
  ✅ **Bon à savoir** : En Python, même si un nombre est entier, vous pouvez le déclarer en `float` en ajoutant `.0` (ex: `version = 2.0`).

### B. Les Chaînes de Caractères (`str`)

Nous les verrons en détail dans le prochain module, mais pour faire simple, elles représentent du texte. On peut les déclarer avec des guillemets simples (`'`) ou doubles (`"`).

```python
message = "Hello, World!"
user_name = 'Bob'
```

### C. Les Booléens (`bool`)

Un booléen ne peut avoir que deux valeurs : `True` ou `False`. Ils sont essentiels pour la logique et les conditions.

```python
is_active = True
has_permission = False
```

❌ **Mauvais** : N'utilisez pas de chaînes de caractères `"True"` ou `"False"`. Utilisez les mots-clés `True` et `False` qui ont une signification spéciale.

### D. Le Type `None`

`None` est un type spécial qui représente l'absence de valeur. C'est l'équivalent de `null` dans d'autres langages.

```python
# La variable 'winner' existe, mais elle n'a pas encore de valeur.
winner = None
```

## 4. Conventions et Bonnes Pratiques

### Nommage des Variables

La convention en Python (définie dans la [PEP 8](https://peps.python.org/pep-0008/)) est d'utiliser le **`snake_case`** pour les noms de variables : des mots en minuscules séparés par des underscores (`_`).

- ✅ **Bon** : `first_name`, `user_age`, `is_logged_in`
- ❌ **Mauvais** : `firstName` (camelCase), `userage`, `isloggedin`

Les noms de variables doivent être **descriptifs**.

- ✅ **Bon** : `remaining_attempts = 3`
- ❌ **Mauvais** : `ra = 3`

### La fonction `type()`

Pour vérifier le type d'une variable, vous pouvez utiliser la fonction intégrée `type()`.

```python
age = 25
price = 9.95
is_admin = True

print(type(age))      # Affiche : <class 'int'>
print(type(price))    # Affiche : <class 'float'>
print(type(is_admin)) # Affiche : <class 'bool'>
```

---

**Checklist :**

- [ ] Les variables stockent des valeurs.
- [ ] Python a des types simples : `int`, `float`, `bool`, `None`.
- [ ] Utiliser le `snake_case` pour nommer les variables.
- [ ] Un nom de variable doit être explicite.
