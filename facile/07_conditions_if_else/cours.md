# Module : Les Conditions (`if`, `elif`, `else`)

## 1. Quoi : Le Contrôle de Flux

Le contrôle de flux (ou _control flow_) est la capacité d'un programme à exécuter différentes actions en fonction de différentes conditions. C'est ce qui permet à un programme de prendre des "décisions".

En Python, la structure de contrôle de flux la plus fondamentale est le bloc `if`/`elif`/`else`.

## 2. Pourquoi : Rendre les programmes intelligents

Sans conditions, un programme ne serait qu'une séquence d'instructions exécutées toujours dans le même ordre. Les conditions permettent de :

- Réagir aux entrées de l'utilisateur.
- Gérer différents états ou cas de figure.
- Valider des données.
- Créer des logiques complexes.

## 3. Comment : La Syntaxe

### A. La condition `if`

Le bloc `if` exécute un morceau de code **uniquement si** une condition est `True`.

**La règle d'or de Python : l'indentation !** Le code à exécuter à l'intérieur de la condition doit être **indenté** (généralement avec 4 espaces). C'est ainsi que Python sait que ce code appartient au bloc `if`.

```python
age = 20

if age >= 18:
    # Ce bloc est indenté, il ne s'exécute que si la condition est vraie.
    print("You are an adult.")
    print("You can vote.")

# Ce bloc n'est pas indenté, il s'exécute toujours.
print("End of program.")
```

### B. La condition `else`

Le bloc `else` est optionnel et s'exécute **uniquement si** la condition du `if` est `False`.

```python
age = 16

if age >= 18:
    print("You are an adult.")
else:
    # Ce bloc s'exécute car la condition (16 >= 18) est fausse.
    print("You are a minor.")
```

### C. La condition `elif` (else if)

Le bloc `elif` permet de tester **plusieurs conditions en séquence**. Python les évalue dans l'ordre. Dès qu'une condition `if` ou `elif` est `True`, son bloc de code est exécuté, et le reste de la structure est ignoré.

```python
score = 85

if score >= 90:
    print("Grade: A")
elif score >= 80:
    # Le score (85) n'est pas >= 90, donc on teste cette condition.
    # 85 >= 80 est vrai, donc ce bloc s'exécute.
    print("Grade: B")
elif score >= 70:
    # Cette condition ne sera même pas testée.
    print("Grade: C")
else:
    print("Grade: F")
```

## 4. "Truthy" et "Falsy"

En Python, d'autres valeurs que `True` et `False` peuvent être évaluées dans un contexte booléen.

- **Valeurs "Falsy"** (considérées comme fausses) :

  - `False`
  - `None`
  - Le nombre `0` (entier ou flottant)
  - Les séquences vides : chaîne `""`, liste `[]`, tuple `()`, dictionnaire `{}`.

- **Valeurs "Truthy"** (considérées comme vraies) :
  - Toutes les autres valeurs ! (ex: un nombre non nul, une chaîne non vide, une liste non vide).

Ceci permet d'écrire des conditions plus concises.

```python
# Au lieu d'écrire : if len(my_list) > 0:
my_list = [1, 2, 3]
if my_list: # ✅ Bon : Plus "pythonic"
    print("The list is not empty.")

# Au lieu d'écrire : if name != "":
name = "Alice"
if name: # ✅ Bon
    print(f"Hello, {name}")

# Un cas d'usage courant pour vérifier si une variable a une valeur
user = None
# ... plus tard dans le code
if user:
    print("User is logged in.")
else:
    print("User is not logged in.")
```

## 5. L'Opérateur Ternaire

C'est une manière compacte d'écrire une instruction `if`/`else` sur une seule ligne, principalement pour assigner une valeur à une variable.

**Syntaxe** : `valeur_si_vrai if condition else valeur_si_faux`

```python
age = 22

# Version longue
if age >= 18:
    status = "Adult"
else:
    status = "Minor"

# Version avec opérateur ternaire
# ✅ Bon : Plus concis pour les assignations simples
status = "Adult" if age >= 18 else "Minor"

print(status) # Affiche "Adult"
```
