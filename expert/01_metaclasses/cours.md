# Module : Les Métaclasses en Python

## Objectif

Ce module a pour but de vous introduire au concept avancé et puissant des métaclasses en Python. Vous apprendrez ce qu'est une métaclasse, comment elle fonctionne, et comment en créer pour personnaliser la création de classes.

> "Les métaclasses sont de la magie noire que 99% des utilisateurs ne devraient jamais toucher. Si vous vous demandez si vous en avez besoin, ce n'est pas le cas." - Tim Peters

Malgré cet avertissement célèbre, comprendre les métaclasses est essentiel pour une maîtrise profonde de la programmation orientée objet en Python.

## 1. Tout est Objet en Python

La première chose à comprendre est qu'en Python, absolument tout est un objet. Cela inclut les nombres, les chaînes de caractères, les fonctions, et même les classes elles-mêmes.

```python
def ma_fonction():
    pass

class MaClasse:
    pass

print(type(5))         # <class 'int'>
print(type("hello"))   # <class 'str'>
print(type(ma_fonction)) # <class 'function'>
print(type(MaClasse))    # <class 'type'>
```

Si `MaClasse` est un objet, cela signifie qu'il a été créé à partir de quelque chose. Ce "quelque chose" est sa **métaclasse**. Par défaut, la métaclasse de toutes les classes en Python est `type`.

`type` est donc à la fois une fonction (pour connaître le type d'un objet) et une classe qui crée d'autres classes.

## 2. `type` : La Métaclasse par Défaut

La fonction `type()` peut être utilisée de deux manières :

1.  `type(objet)` : Renvoie le type de l'objet.
2.  `type(nom, bases, attrs)` : Crée une nouvelle classe dynamiquement.

C'est cette deuxième utilisation qui est la clé pour comprendre les métaclasses.

```python
# Définition de classe traditionnelle
class MaClasse:
    x = 10
    def methode(self):
        return "hello"

# Création de la même classe dynamiquement avec type()
def methode_dynamique(self):
    return "hello"

MaClasseDynamique = type(
    'MaClasseDynamique',  # Nom de la classe
    (object,),            # Tuple des classes de base
    {                     # Dictionnaire des attributs et méthodes
        'x': 10,
        'methode': methode_dynamique
    }
)

instance = MaClasseDynamique()
print(instance.x)       # 10
print(instance.methode()) # hello
print(type(MaClasseDynamique)) # <class 'type'>
```

Lorsque vous écrivez `class MaClasse: ...`, Python fait en réalité appel à `type('MaClasse', bases, attrs)` en coulisses pour construire l'objet classe.

## 3. Créer une Métaclasse Personnalisée

Une métaclasse est une classe dont les instances sont des classes. Pour créer votre propre métaclasse, vous devez hériter de `type`.

La méthode la plus importante à surcharger dans une métaclasse est `__new__`. C'est cette méthode qui est appelée avant le `__init__` pour construire et retourner le nouvel objet (ici, l'objet classe).

```python
# 1. Définition de la métaclasse
class MaMetaclasse(type):
    def __new__(cls, name, bases, attrs):
        print(f"--- Création de la classe '{name}' avec MaMetaclasse ---")
        print(f"Nom de la classe: {name}")
        print(f"Classes de base: {bases}")
        print(f"Attributs: {attrs}")

        # On peut modifier les attributs avant de créer la classe
        attrs['auteur'] = 'Copilot'

        # On appelle le __new__ de la classe parente (type) pour créer la classe
        nouvelle_classe = super().__new__(cls, name, bases, attrs)

        print(f"--- Classe '{name}' créée. ---")
        return nouvelle_classe

# 2. Utilisation de la métaclasse
class MaClasse(metaclass=MaMetaclasse):
    x = 10

    def __init__(self):
        print("Instance de MaClasse créée.")

# L'affichage de la métaclasse se produit à la DÉFINITION de la classe,
# pas à l'instanciation.

print("\nMaintenant, créons une instance...")
instance = MaClasse()
print(f"Attribut ajouté par la métaclasse : {instance.auteur}")
```

**Analyse de l'exécution :**

1.  Python lit la définition de `MaClasse`.
2.  Il voit `metaclass=MaMetaclasse`.
3.  Il appelle `MaMetaclasse.__new__` avec les informations de `MaClasse` (`name`, `bases`, `attrs`).
4.  Notre métaclasse affiche les informations, ajoute l'attribut `auteur`, puis appelle `type.__new__` pour créer réellement l'objet classe.
5.  L'objet classe est retourné et assigné à la variable `MaClasse`.
6.  Seulement ensuite, le reste du script s'exécute. L'appel `MaClasse()` crée une _instance_ de la classe, ce qui déclenche son `__init__`.

## 4. Cas d'Usage des Métaclasses

Les métaclasses sont utiles pour des patrons de conception qui nécessitent d'intervenir sur la création des classes elles-mêmes.

### a. Enregistrer des Classes (Pattern Registry)

Imaginez que vous voulez garder une trace de toutes les classes de "plugins" définies dans votre code.

```python
plugins = {}

class PluginMeta(type):
    def __new__(cls, name, bases, attrs):
        new_class = super().__new__(cls, name, bases, attrs)
        # On n'enregistre pas la classe de base du plugin
        if name != 'BasePlugin':
            print(f"Enregistrement du plugin: {name}")
            plugins[name.lower()] = new_class
        return new_class

class BasePlugin(metaclass=PluginMeta):
    pass

# Dès que ces classes sont définies, elles sont enregistrées
class PluginA(BasePlugin):
    pass

class PluginB(BasePlugin):
    pass

print("\nPlugins enregistrés :", plugins)
# Output:
# Enregistrement du plugin: PluginA
# Enregistrement du plugin: PluginB
#
# Plugins enregistrés : {'plugina': <class '__main__.PluginA'>, 'pluginb': <class '__main__.PluginB'>}
```

### b. Valider des Attributs de Classe

Vous pouvez forcer les classes à respecter certaines règles, par exemple, s'assurer qu'elles définissent certains attributs.

```python
class APIValidatorMeta(type):
    def __new__(cls, name, bases, attrs):
        if 'api_endpoint' not in attrs:
            raise TypeError(f"La classe '{name}' doit définir un attribut 'api_endpoint'.")
        if not attrs['api_endpoint'].startswith('/'):
            raise ValueError(f"L'attribut 'api_endpoint' de '{name}' doit commencer par '/'.")

        return super().__new__(cls, name, bases, attrs)

class BaseAPI(metaclass=APIValidatorMeta):
    pass

# Cette classe est valide
class UserAPI(BaseAPI):
    api_endpoint = '/users'

# Cette classe lèvera une TypeError
# class ProductAPI(BaseAPI):
#     pass

# Cette classe lèvera une ValueError
# class OrderAPI(BaseAPI):
#     api_endpoint = 'orders'
```

### c. Singleton

Bien qu'il y ait des manières plus simples de créer des Singletons en Python, une métaclasse est une approche possible.

```python
class SingletonMeta(type):
    _instances = {}
    def __call__(cls, *args, **kwargs):
        # __call__ est appelée quand on fait Classe()
        if cls not in cls._instances:
            instance = super().__call__(*args, **kwargs)
            cls._instances[cls] = instance
        return cls._instances[cls]

class Database(metaclass=SingletonMeta):
    def __init__(self):
        print("Connexion à la base de données...")

db1 = Database()
db2 = Database()

print(db1 is db2) # True
# Output:
# Connexion à la base de données...
# True
```

## Conclusion

Les métaclasses sont un concept qui opère au niveau de la création des classes, pas des instances. Elles permettent d'automatiser, de valider et de modifier des classes au moment de leur définition. Bien que rares, elles sont le mécanisme sous-jacent de nombreux frameworks et bibliothèques Python (comme les ORM Django ou SQLAlchemy) pour fournir une API déclarative et puissante. Les comprendre, c'est comprendre le fonctionnement interne le plus profond du modèle objet de Python.
