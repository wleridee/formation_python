# Module : L'Introspection en Python

## Objectif

Ce module a pour but de vous faire découvrir l'introspection, qui est la capacité d'un programme à examiner son propre état et sa propre structure (objets, modules, fonctions) pendant son exécution. Vous apprendrez à utiliser les fonctions et attributs intégrés de Python pour explorer dynamiquement votre code.

## 1. Qu'est-ce que l'Introspection ?

L'introspection consiste à poser des questions à des objets pendant l'exécution. Par exemple :

- Quel est le type de cet objet ?
- Quels sont les attributs et méthodes de cet objet ?
- Cette fonction attend-elle des arguments ?
- Cette classe hérite-t-elle d'une autre classe ?

Python, étant un langage très dynamique, offre des capacités d'introspection très puissantes. C'est ce qui permet aux débogueurs, aux IDE et à de nombreux frameworks de fonctionner.

## 2. Fonctions d'Introspection de Base

### a. `type()`

Renvoie le type d'un objet. C'est la fonction d'introspection la plus fondamentale.

```python
nom = "Alice"
age = 30
nombres = [1, 2, 3]

print(type(nom))   # <class 'str'>
print(type(age))   # <class 'int'>
print(type(nombres)) # <class 'list'>
```

### b. `dir()`

Sans argument, `dir()` liste les noms dans la portée locale.
Avec un objet en argument, `dir()` retourne la liste des attributs et méthodes valides pour cet objet.

```python
ma_liste = [1, 2]

# Affiche toutes les méthodes et attributs de l'objet liste
print(dir(ma_liste))
# ['__add__', '__class__', ..., 'append', 'clear', 'copy', 'count', 'extend', ...]
```

`dir()` est extrêmement utile pour explorer un objet inconnu dans une console interactive.

### c. `id()`

Retourne l'identifiant unique d'un objet en mémoire. C'est utile pour vérifier si deux variables pointent vers le même objet.

```python
a = [1, 2]
b = a
c = [1, 2]

print(id(a) == id(b)) # True (a et b sont deux noms pour le même objet)
print(id(a) == id(c)) # False (c est un objet différent, même si son contenu est identique)
```

### d. `hasattr()`, `getattr()`, `setattr()`, `delattr()`

Ces fonctions permettent de manipuler les attributs d'un objet en utilisant des chaînes de caractères pour leur nom.

- `hasattr(objet, 'nom_attribut')`: Vérifie si un objet a un attribut donné.
- `getattr(objet, 'nom_attribut'[, defaut])`: Récupère la valeur d'un attribut. Peut retourner une valeur par défaut si l'attribut n'existe pas.
- `setattr(objet, 'nom_attribut', valeur)`: Assigne une valeur à un attribut.
- `delattr(objet, 'nom_attribut')`: Supprime un attribut.

```python
class Personne:
    def __init__(self, nom):
        self.nom = nom

p = Personne("Alice")

# Vérifier
if hasattr(p, 'nom'):
    print(f"La personne a un nom : {getattr(p, 'nom')}")

# Ajouter un attribut dynamiquement
setattr(p, 'age', 30)
print(f"Nouvel attribut 'age' : {p.age}")

# Supprimer un attribut
delattr(p, 'nom')
# print(p.nom) # AttributeError
```

Ces fonctions sont le cœur de la programmation dynamique et sont massivement utilisées dans les frameworks pour configurer des objets à partir de fichiers de configuration, par exemple.

## 3. Introspection sur les Types et Classes

### a. `isinstance()` et `issubclass()`

- `isinstance(objet, type_ou_tuple_de_types)`: Vérifie si un objet est une instance d'une classe ou d'une de ses sous-classes. C'est la manière correcte de vérifier le type d'un objet.
- `issubclass(classe, classe_de_base)`: Vérifie si une classe hérite d'une autre.

```python
class Animal: pass
class Chien(Animal): pass
class Chat: pass

rex = Chien()

print(isinstance(rex, Chien))  # True
print(isinstance(rex, Animal)) # True (car Chien hérite de Animal)
print(isinstance(rex, Chat))   # False

print(issubclass(Chien, Animal)) # True
print(issubclass(Chat, Animal))  # False
```

**Pourquoi `isinstance()` est mieux que `type()` ?**
`type(rex) == Animal` serait `False`. `isinstance()` respecte l'héritage, ce qui est essentiel pour le polymorphisme.

### b. Attributs Spéciaux des Objets

- `__dict__`: Un dictionnaire qui contient les attributs (modifiables) d'un objet.
- `__class__`: Retourne la classe à laquelle une instance appartient.
- `__name__`: Le nom d'une classe, fonction ou module.
- `__doc__`: La docstring d'un objet.

```python
class MaClasse:
    """Ceci est une docstring."""
    pass

obj = MaClasse()
obj.x = 100

print(obj.__dict__)   # {'x': 100}
print(obj.__class__)  # <class '__main__.MaClasse'>
print(MaClasse.__name__) # MaClasse
print(MaClasse.__doc__)  # Ceci est une docstring.
```

## 4. Le Module `inspect`

Pour une introspection encore plus poussée, notamment sur les fonctions et les modules, le module `inspect` est l'outil de choix.

### a. Examiner des Fonctions

```python
import inspect

def ma_fonction(a, b=10, *args, **kwargs):
    """Une fonction d'exemple."""
    pass

# Obtenir la signature de la fonction
sig = inspect.signature(ma_fonction)
print(f"Signature : {sig}") # (a, b=10, *args, **kwargs)

# Itérer sur les paramètres
for name, param in sig.parameters.items():
    print(f"  - {name}: type={param.kind}, default={param.default}")

# Obtenir le code source
print("\nCode source :")
print(inspect.getsource(ma_fonction))
```

### b. Examiner des Objets

```python
import inspect

class MaClasse:
    def methode(self):
        pass

# Lister les membres d'une classe
# (nom, type_de_membre)
membres = inspect.getmembers(MaClasse, predicate=inspect.isfunction)
print(membres) # [('methode', <function MaClasse.methode at ...>)]
```

Le module `inspect` est très puissant et permet d'analyser la pile d'appels (`stack`), les modules, les classes, etc.

## 5. Cas d'Usage de l'Introspection

- **Frameworks et ORM** : Des frameworks comme Django ou SQLAlchemy utilisent l'introspection pour découvrir les modèles que vous avez définis, leurs champs, et générer automatiquement des tables de base de données ou des formulaires web.
- **Tests et Mocking** : Les bibliothèques de test comme `pytest` ou `unittest.mock` utilisent l'introspection pour remplacer des parties de votre code par des "mocks" (bouchons) pendant les tests.
- **Débogage** : Le débogueur `pdb` utilise l'introspection pour vous permettre d'inspecter l'état des variables à n'importe quel point de l'exécution.
- **Sérialisation** : Convertir des objets Python en formats comme JSON ou XML. L'introspection permet de parcourir les attributs d'un objet pour les écrire dans le format de sortie.

## Conclusion

L'introspection est une caractéristique qui rend Python extrêmement flexible et puissant. Elle permet au code de s'examiner et de se modifier lui-même, ouvrant la porte à des techniques de méta-programmation avancées. Bien que vous ne l'utilisiez peut-être pas tous les jours dans votre code applicatif, comprendre ses principes est essentiel pour savoir comment fonctionnent les outils et frameworks que vous utilisez au quotidien. Les fonctions `type()`, `dir()`, `isinstance()` et `hasattr()` sont des outils de base que tout développeur Python devrait connaître.
