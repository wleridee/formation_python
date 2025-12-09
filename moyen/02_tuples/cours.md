# Module : Les Tuples

## 1. Quoi : Les Tuples

Un **tuple** est une collection **ordonnée** et **immuable** (non modifiable) d'éléments. Il est très similaire à une liste, à la différence près qu'une fois créé, un tuple ne peut plus être changé.

- **Ordonnée** : Les éléments conservent leur ordre.
- **Immuable** : Vous ne pouvez PAS ajouter, supprimer ou modifier des éléments après la création.

## 2. Pourquoi : L'immuabilité comme fonctionnalité

Si les listes sont si flexibles, pourquoi utiliser des tuples ?

- **Sécurité des données** : L'immuabilité garantit que les données ne seront pas modifiées accidentellement. C'est parfait pour des données qui ne doivent pas changer, comme des coordonnées (latitude, longitude) ou des couleurs RGB (rouge, vert, bleu).
- **Performance** : Les tuples sont généralement un peu plus rapides et consomment moins de mémoire que les listes, car Python peut faire des optimisations en sachant qu'ils ne changeront pas.
- **Clés de dictionnaire** : Seuls les objets immuables peuvent être utilisés comme clés dans un dictionnaire. Vous pouvez donc utiliser un tuple comme clé, mais pas une liste.

## 3. Comment : Création et Utilisation

### A. Création

On crée un tuple avec des parenthèses `()`, en séparant les éléments par des virgules.

```python
# Un tuple de nombres
point = (10, 20)

# Un tuple de chaînes de caractères
weekdays = ("Monday", "Tuesday", "Wednesday")

# Un tuple avec des types mixtes
person_data = ("Alice", 30, "Engineer")

# Les parenthèses sont optionnelles dans de nombreux cas
another_tuple = 1, 2, 3 # C'est la virgule qui fait le tuple !

# Pour créer un tuple avec un seul élément, la virgule est OBLIGATOIRE
single_element_tuple = (42,) # Sans la virgule, ce serait juste le nombre 42
empty_tuple = ()
```

### B. Accès aux éléments

L'accès aux éléments et le slicing fonctionnent exactement comme pour les listes.

```python
point = (10, 20, 30)

x = point[0]  # 10
y = point[1]  # 20
last = point[-1] # 30

sub_tuple = point[0:2] # (10, 20)
```

### C. L'immuabilité en action

Tenter de modifier un tuple lèvera une erreur.

```python
my_tuple = (1, 2, 3)

# Ces opérations sont IMPOSSIBLES et lèveront une TypeError :
# my_tuple[0] = 99
# my_tuple.append(4)
# del my_tuple[0]
```

Si vous avez besoin de "modifier" un tuple, la seule solution est d'en créer un nouveau en combinant des tuples existants.

```python
tuple1 = (1, 2)
tuple2 = (3, 4)
new_tuple = tuple1 + tuple2 # Crée un nouveau tuple : (1, 2, 3, 4)
```

### D. Le "Tuple Unpacking" (Déballage de tuple)

C'est une fonctionnalité extrêmement puissante et "pythonic". Elle permet d'assigner les éléments d'un tuple à plusieurs variables en une seule ligne.

```python
# Les données sont stockées dans un tuple
point = (100, 200)

# On "déballe" le tuple dans les variables x et y
x, y = point

print(f"x: {x}, y: {y}") # Affiche : x: 100, y: 200
```

Le déballage de tuple est très utilisé, par exemple pour retourner plusieurs valeurs d'une fonction.

```python
def get_user_info():
    # La fonction retourne un tuple
    return "John Doe", 35, "john.doe@example.com"

# On déballe directement le résultat de la fonction
name, age, email = get_user_info()
```

### E. Fonctions et Méthodes

Les tuples ont beaucoup moins de méthodes que les listes, car ils sont immuables.

- `.count(valeur)` : Compte le nombre d'occurrences d'une valeur.
- `.index(valeur)` : Retourne l'index de la première occurrence d'une valeur.

```python
numbers = (1, 2, 3, 2, 4, 2)
print(numbers.count(2))   # 3
print(numbers.index(3))   # 2
```

## 4. Liste vs Tuple : Le Résumé

| Caractéristique             | Liste (`list`)                                                | Tuple (`tuple`)                                                              |
| :-------------------------- | :------------------------------------------------------------ | :--------------------------------------------------------------------------- |
| **Syntaxe**                 | `[1, 2, 3]`                                                   | `(1, 2, 3)`                                                                  |
| **Mutabilité**              | **Muable** (modifiable)                                       | **Immuable** (non modifiable)                                                |
| **Usage typique**           | Collections d'éléments qui vont changer (ajouter, supprimer). | Collections de données qui ne doivent pas changer (coordonnées, constantes). |
| **Performance**             | Légèrement moins performante.                                 | Légèrement plus performante.                                                 |
| **En tant que clé de dict** | Non                                                           | Oui                                                                          |

✅ **Bonne pratique** : Utilisez une liste par défaut. Si vous réalisez que les données ne devraient pas changer ou si vous avez besoin d'une clé de dictionnaire, utilisez un tuple.
