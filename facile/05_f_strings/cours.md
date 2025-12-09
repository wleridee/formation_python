# Module : Formatage avec les f-strings

## 1. Quoi : Les f-strings

Introduites en Python 3.6, les **f-strings** (ou _formatted string literals_) sont une manière moderne, lisible et performante de formater des chaînes de caractères. Elles permettent d'intégrer directement des expressions Python à l'intérieur d'une chaîne.

La syntaxe est simple : il suffit de préfixer la chaîne par la lettre `f` (ou `F`) et de placer les variables ou expressions entre accolades `{}`.

```python
name = "Alice"
age = 30

# La f-string remplace {name} et {age} par leurs valeurs
greeting = f"Hello, my name is {name} and I am {age} years old."
print(greeting)
# Affiche : "Hello, my name is Alice and I am 30 years old."
```

## 2. Pourquoi : Les avantages des f-strings

Avant les f-strings, les développeurs Python utilisaient principalement la méthode `.format()` ou l'opérateur `%`.

- **Ancienne méthode (`.format()`)** :
  ```python
  greeting = "Hello, my name is {} and I am {} years old.".format(name, age)
  ```
- **Très ancienne méthode (`%`)** :
  ```python
  greeting = "Hello, my name is %s and I am %d years old." % (name, age)
  ```

Les f-strings sont aujourd'hui la méthode recommandée pour plusieurs raisons :

- **Lisibilité** : Les variables sont directement insérées là où elles apparaissent dans la chaîne, ce qui rend le code beaucoup plus facile à lire.
- **Performance** : Elles sont généralement plus rapides que les autres méthodes de formatage.
- **Puissance** : On peut mettre presque n'importe quelle expression Python valide à l'intérieur des accolades.

## 3. Comment : Utilisation avancée des f-strings

### A. Exécution d'expressions

Vous pouvez faire plus que simplement insérer des variables.

```python
price = 19.99
tax_rate = 0.20

# Calcul directement dans la f-string
total_price_message = f"Total price with tax: {price * (1 + tax_rate)}"
print(total_price_message) # Affiche : "Total price with tax: 23.988"

# Appel de fonctions ou de méthodes
name = "  Bob  "
message = f"Welcome, {name.strip().title()}!"
print(message) # Affiche : "Welcome, Bob!"
```

### B. Options de formatage

Les f-strings permettent de contrôler précisément l'affichage des valeurs, notamment pour les nombres. On ajoute un deux-points `:` suivi d'un spécificateur de format.

- **Formatage des nombres à virgule flottante (`float`)** :

  - Spécifiez le nombre de décimales avec `.nf`, où `n` est le nombre de chiffres après la virgule.

  ```python
  pi = 3.14159265
  message = f"The value of pi is approximately {pi:.2f}"
  print(message) # Affiche : "The value of pi is approximately 3.14"
  ```

- **Remplissage et alignement** :

  - Alignez le texte à l'intérieur d'un espace donné.

  ```python
  text = "hello"
  # Aligne à gauche sur 10 caractères
  left_aligned = f"'{text:<10}'"  # Résultat : "'hello     '"
  # Aligne à droite sur 10 caractères
  right_aligned = f"'{text:>10}'" # Résultat : "'     hello'"
  # Centre sur 10 caractères
  centered = f"'{text:^10}'"      # Résultat : "'  hello   '"
  # Centre et remplit avec un caractère
  filled_centered = f"'{text:*^10}'" # Résultat : "'**hello***'"
  ```

### C. Débogage rapide (Python 3.8+)

C'est une fonctionnalité extrêmement pratique pour le débogage. En ajoutant un signe égal `=` à la fin de l'expression, la f-string affichera à la fois l'expression et sa valeur.

```python
user = "admin"
is_logged_in = True

# La syntaxe {variable=} est un raccourci pour f"variable={variable}"
print(f"{user=} {is_logged_in=}")
# Affiche : user='admin' is_logged_in=True
```

## 4. Bonnes Pratiques

- ✅ **Bon** : Utilisez les f-strings comme méthode de formatage par défaut dans tout nouveau code Python (version 3.6+).
- ✅ **Bon** : Profitez de la possibilité d'appeler des méthodes (comme `.strip()`) directement dans les accolades pour garder le code concis.
- ❌ **Mauvais** : Évitez de mettre une logique métier trop complexe à l'intérieur d'une f-string. Si le calcul est long, faites-le d'abord dans une variable, puis insérez la variable dans la chaîne pour une meilleure lisibilité.

---

**Ressources Externes :**

- [PEP 498 -- Literal String Interpolation](https://peps.python.org/pep-0498/)
- [Guide complet sur le formatage de chaînes en Python](https://realpython.com/python-string-formatting/)
