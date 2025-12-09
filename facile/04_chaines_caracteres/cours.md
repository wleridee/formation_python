# Module : Les Chaînes de Caractères (`str`)

## 1. Quoi : Le type `str`

Une chaîne de caractères (ou _string_ en anglais) est une **séquence de caractères** utilisée pour représenter du texte. En Python, les chaînes sont **immuables** (_immutable_), ce qui signifie qu'une fois créées, elles ne peuvent pas être modifiées directement. Toute opération qui semble modifier une chaîne en crée en réalité une nouvelle.

## 2. Pourquoi : L'omniprésence du texte

Le texte est partout en programmation : noms d'utilisateurs, messages d'erreur, contenu de fichiers, données d'API, etc. Maîtriser la manipulation des chaînes de caractères est donc une compétence fondamentale.

## 3. Comment : Création et Manipulation

### A. Création

On peut créer des chaînes avec des guillemets simples (`'`), doubles (`"`) ou triples (`'''` ou `"""`) pour les chaînes multi-lignes.

```python
# Guillemets simples ou doubles, c'est pareil
simple_quote_string = 'Hello, World!'
double_quote_string = "Hello, Python!"

# ✅ Bon : Utiliser des guillemets différents permet d'inclure l'autre dans la chaîne
quote_inside = "Il a dit 'Bonjour !'"

# Chaînes multi-lignes avec des triples guillemets
multi_line_string = """Ceci est une chaîne
qui s'étend sur
plusieurs lignes."""
```

### B. Concaténation et Répétition

- **Concaténation** (coller des chaînes ensemble) avec l'opérateur `+`.
- **Répétition** avec l'opérateur `*`.

```python
first_name = "John"
last_name = "Doe"

# Concaténation
full_name = first_name + " " + last_name  # Résultat : "John Doe"

# Répétition
separator = "-" * 10  # Résultat : "----------"
```

> **Note** : La concaténation avec `+` est simple mais peut être inefficace si vous collez un grand nombre de chaînes. Les f-strings (vues au prochain module) sont généralement préférables.

### C. Accès aux caractères et Slicing

Les chaînes se comportent comme des séquences. On peut accéder à leurs éléments avec des index.

- L'indexation commence à **0**.
- On peut utiliser des index négatifs pour partir de la fin.

```python
greeting = "Hello"

# Accès par index
first_char = greeting[0]   # 'H'
last_char = greeting[4]    # 'o'
last_char_alt = greeting[-1] # 'o' (plus "pythonic")

# Slicing (découpage)
# Syntaxe : [start:stop:step] (stop est exclu)
slice1 = greeting[1:4]   # 'ell' (de l'index 1 à 3)
slice2 = greeting[:3]    # 'Hel' (du début à l'index 2)
slice3 = greeting[2:]    # 'llo' (de l'index 2 à la fin)
```

### D. Méthodes de chaînes courantes

Python fournit une multitude de méthodes utiles pour travailler avec les chaînes. Voici quelques-unes des plus courantes.

```python
text = "  Hello, World!  "

# Changer la casse
upper_text = text.upper()  # "  HELLO, WORLD!  "
lower_text = text.lower()  # "  hello, world!  "
title_text = text.title()  # "  Hello, World!  " (Met en majuscule chaque mot)

# Supprimer les espaces au début et à la fin
stripped_text = text.strip()  # "Hello, World!"

# Remplacer une sous-chaîne
replaced_text = stripped_text.replace("World", "Python") # "Hello, Python!"

# Vérifier le début ou la fin
starts_with_hello = stripped_text.startswith("Hello") # True
ends_with_python = replaced_text.endswith("Python")   # True

# Trouver la position d'une sous-chaîne
# .find() retourne -1 si non trouvé
position = stripped_text.find(",") # 5
# .index() lève une erreur si non trouvé
```

### E. Fonctions utiles

- `len()` : Pour obtenir la longueur d'une chaîne.

```python
message = "Python"
length = len(message)  # 6
```

## 4. Immuabilité : Un concept clé

Comme les chaînes sont immuables, vous ne pouvez pas faire ceci :

```python
my_string = "Hello"
my_string[0] = "J"  # ❌ Erreur ! TypeError: 'str' object does not support item assignment
```

Pour "changer" une chaîne, vous devez en créer une nouvelle, par exemple avec `replace()` ou en la reconstruisant.

```python
# ✅ Bon : Créer une nouvelle chaîne
my_string = "Hello"
my_string = "J" + my_string[1:]  # Crée "Jello"
```

---

**Checklist :**

- [ ] Les chaînes sont des séquences de caractères immuables.
- [ ] Utiliser `+` pour la concaténation et `*` pour la répétition.
- [ ] L'indexation commence à 0. Le slicing permet d'extraire des sous-chaînes.
- [ ] Les méthodes comme `.upper()`, `.strip()`, `.replace()` sont vos meilleures amies.
- [ ] `len()` donne la longueur de la chaîne.
