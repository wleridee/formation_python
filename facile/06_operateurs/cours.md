# Module : Les Opérateurs

## 1. Quoi : Les Opérateurs

Les opérateurs sont des symboles spéciaux qui effectuent des opérations sur des valeurs (appelées opérandes). Par exemple, dans `5 + 3`, `5` et `3` sont les opérandes et `+` est l'opérateur.

Python supporte de nombreux types d'opérateurs, que l'on peut classer en plusieurs catégories.

## 2. Pourquoi : Le cœur de la logique

Les opérateurs sont les briques de base de toute logique de programmation. Ils permettent de réaliser des calculs mathématiques, de comparer des valeurs et de combiner des conditions pour contrôler le flux d'un programme.

## 3. Comment : Les Catégories d'Opérateurs

### A. Opérateurs Arithmétiques

Ce sont les opérateurs mathématiques de base.

| Opérateur | Nom              | Exemple   | Résultat |
| :-------- | :--------------- | :-------- | :------- |
| `+`       | Addition         | `10 + 5`  | `15`     |
| `-`       | Soustraction     | `10 - 5`  | `5`      |
| `*`       | Multiplication   | `10 * 5`  | `50`     |
| `/`       | Division         | `10 / 3`  | `3.333`  |
| `//`      | Division Entière | `10 // 3` | `3`      |
| `%`       | Modulo (reste)   | `10 % 3`  | `1`      |
| `**`      | Exponentiation   | `10 ** 2` | `100`    |

```python
# La division / produit toujours un float.
result = 10 / 2  # result est 5.0

# La division entière // "arrondit" au nombre entier inférieur.
quotient = 10 // 3 # quotient est 3

# Le modulo est très utile pour savoir si un nombre est pair ou impair.
is_even = 10 % 2 == 0 # True, car le reste est 0
```

### B. Opérateurs de Comparaison

Ils comparent deux valeurs et retournent un booléen (`True` ou `False`).

| Opérateur | Nom                 | Exemple  | Résultat |
| :-------- | :------------------ | :------- | :------- |
| `==`      | Égal à              | `5 == 5` | `True`   |
| `!=`      | Différent de        | `5 != 5` | `False`  |
| `>`       | Supérieur à         | `5 > 3`  | `True`   |
| `<`       | Inférieur à         | `5 < 3`  | `False`  |
| `>=`      | Supérieur ou égal à | `5 >= 5` | `True`   |
| `<=`      | Inférieur ou égal à | `5 <= 3` | `False`  |

❌ **Erreur courante** : Confondre l'opérateur d'assignation (`=`) avec l'opérateur de comparaison (`==`).

- `age = 30` : **Assigner** la valeur 30 à la variable `age`.
- `age == 30` : **Vérifier** si la valeur de `age` est égale à 30.

### C. Opérateurs Logiques

Ils permettent de combiner des expressions booléennes.

| Opérateur | Nom         | Description                                                            |
| :-------- | :---------- | :--------------------------------------------------------------------- |
| `and`     | ET logique  | Retourne `True` si **les deux** expressions sont vraies.               |
| `or`      | OU logique  | Retourne `True` si **au moins une** des expressions est vraie.         |
| `not`     | NON logique | Inverse l'expression booléenne (`True` devient `False` et vice-versa). |

```python
age = 25
has_ticket = True

# Exemple avec 'and'
if age > 18 and has_ticket:
    print("Access granted.") # S'exécute

# Exemple avec 'or'
is_admin = False
is_editor = True
if is_admin or is_editor:
    print("Can edit content.") # S'exécute

# Exemple avec 'not'
is_banned = False
if not is_banned:
    print("Welcome!") # S'exécute
```

### D. Opérateurs d'Assignation

Ils sont des raccourcis pour modifier la valeur d'une variable.

| Opérateur | Équivalent à | Exemple (`x` vaut 10) | Résultat (`x` vaut) |
| :-------- | :----------- | :-------------------- | :------------------ |
| `+=`      | `x = x + 5`  | `x += 5`              | `15`                |
| `-=`      | `x = x - 5`  | `x -= 5`              | `5`                 |
| `*=`      | `x = x * 5`  | `x *= 5`              | `50`                |
| `/=`      | `x = x / 5`  | `x /= 5`              | `2.0`               |

✅ **Bon** : Utiliser ces opérateurs rend le code plus concis et plus lisible.

```python
score = 100
score += 10  # Le joueur gagne 10 points. score vaut maintenant 110.
```

## 4. Priorité des Opérateurs

Python suit un ordre de priorité pour évaluer les expressions, similaire à l'ordre des opérations en mathématiques.

1.  `()` : Parenthèses
2.  `**` : Exponentiation
3.  `*`, `/`, `//`, `%` : Multiplication/Division
4.  `+`, `-` : Addition/Soustraction
5.  `>`, `<`, `==`, etc. : Comparaisons
6.  `not`
7.  `and`
8.  `or`

✅ **Bonne pratique** : En cas de doute, utilisez des parenthèses `()` pour rendre l'ordre d'évaluation explicite et améliorer la lisibilité.

```python
# Difficile à lire
result = 5 + 3 * 2  # 11

# Facile à lire
result = 5 + (3 * 2) # 11
```
