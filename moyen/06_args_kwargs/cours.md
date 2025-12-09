# Module : Arguments Flexibles (`*args` et `**kwargs`)

## 1. Quoi : `*args` et `**kwargs`

`*args` et `**kwargs` sont des conventions de syntaxe en Python qui permettent à une fonction d'accepter un **nombre variable d'arguments**.

- `*args` (Arguments) : Permet de passer un nombre variable d'arguments **positionnels**. Python les regroupe dans un **tuple**.
- `**kwargs` (Keyword Arguments) : Permet de passer un nombre variable d'arguments **nommés**. Python les regroupe dans un **dictionnaire**.

Les noms `args` et `kwargs` sont une convention. C'est l'astérisque (`*`) et le double astérisque (`**`) qui sont importants.

## 2. Pourquoi : Créer des fonctions flexibles

Ces syntaxes sont utiles dans de nombreux scénarios :

- Créer des fonctions qui peuvent opérer sur un nombre indéterminé d'entrées (ex: une fonction `sum()` qui additionne tous les nombres passés en argument).
- Créer des "wrappers" ou des "décorateurs" qui doivent transmettre des arguments à une autre fonction sans connaître ces arguments à l'avance.
- Étendre les fonctionnalités d'une fonction existante sans casser la compatibilité avec l'ancienne version.

## 3. Comment : Utilisation de `*args`

`*args` collecte tous les arguments positionnels supplémentaires dans un tuple.

```python
# 'a' et 'b' sont des paramètres positionnels normaux.
# *args collecte tout le reste.
def my_function(a, b, *args):
    print(f"a: {a}")
    print(f"b: {b}")
    print(f"args: {args}") # args est un tuple
    print("-" * 10)

my_function(1, 2)
# a: 1
# b: 2
# args: ()  (un tuple vide)

my_function(1, 2, 3, 4, 5)
# a: 1
# b: 2
# args: (3, 4, 5)
```

**Exemple pratique : une fonction `sum_all`**

```python
def sum_all(*numbers):
    """Calculates the sum of all numbers passed as arguments."""
    total = 0
    for number in numbers: # 'numbers' est un tuple
        total += number
    return total

print(sum_all(1, 2, 3))       # 6
print(sum_all(10, 20, 30, 40)) # 100
print(sum_all())              # 0
```

## 4. Comment : Utilisation de `**kwargs`

`**kwargs` collecte tous les arguments nommés supplémentaires dans un dictionnaire.

```python
def display_info(**kwargs):
    print(f"kwargs: {kwargs}") # kwargs est un dictionnaire
    for key, value in kwargs.items():
        print(f" - {key}: {value}")
    print("-" * 10)

display_info(name="Alice", age=30)
# kwargs: {'name': 'Alice', 'age': 30}
#  - name: Alice
#  - age: 30

display_info(user_id=123, status="active", theme="dark")
# kwargs: {'user_id': 123, 'status': 'active', 'theme': 'dark'}
#  - user_id: 123
#  - status: active
#  - theme: dark
```

## 5. Combinaison de tous les types d'arguments

L'ordre des paramètres dans la définition d'une fonction doit être respecté :

1.  Arguments positionnels standards.
2.  `*args`.
3.  Arguments nommés standards (après `*args`).
4.  `**kwargs`.

```python
def kitchen_sink(a, b, *args, setting1="default", **kwargs):
    print(f"a: {a}")
    print(f"b: {b}")
    print(f"args: {args}")
    print(f"setting1: {setting1}")
    print(f"kwargs: {kwargs}")
    print("-" * 20)

kitchen_sink(1, 2, 3, 4, setting1="custom", user="Alice", score=99)
# a: 1
# b: 2
# args: (3, 4)
# setting1: custom
# kwargs: {'user': 'Alice', 'score': 99}
```

## 6. Déballage d'arguments (`*` et `**` à l'appel)

On peut aussi utiliser `*` et `**` lors de l'**appel** d'une fonction pour "déballer" une liste/tuple ou un dictionnaire en arguments.

### Déballage avec `*`

Si vous avez une liste ou un tuple, vous pouvez le passer comme arguments positionnels à une fonction.

```python
def add(a, b, c):
    return a + b + c

my_list = [10, 20, 30]

# Au lieu de faire add(my_list[0], my_list[1], my_list[2])
# On peut "déballer" la liste :
result = add(*my_list)
print(result) # 60
```

### Déballage avec `**`

Si vous avez un dictionnaire, vous pouvez le passer comme arguments nommés à une fonction. Les clés du dictionnaire doivent correspondre aux noms des paramètres de la fonction.

```python
def create_user(name, age, city):
    print(f"User: {name}, Age: {age}, City: {city}")

user_data = {
    "name": "Bob",
    "age": 42,
    "city": "New York"
}

# Le dictionnaire est déballé en arguments nommés :
# name="Bob", age=42, city="New York"
create_user(**user_data)
```

Cette technique est extrêmement utile pour appeler des fonctions de manière dynamique à partir de données de configuration ou de résultats d'API.
