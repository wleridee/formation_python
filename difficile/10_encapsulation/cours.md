# Module : L'Encapsulation en Programmation Orientée Objet

## Objectif

Ce module a pour but de vous introduire au principe d'encapsulation en Python. Vous apprendrez comment protéger les données d'un objet contre les modifications accidentelles ou non autorisées en utilisant des conventions pour les attributs "privés" et "protégés".

## 1. Qu'est-ce que l'Encapsulation ?

L'encapsulation est l'un des piliers de la programmation orientée objet (POO). Elle consiste à **regrouper les données (attributs) et les méthodes qui les manipulent au sein d'un même objet**.

Un aspect clé de l'encapsulation est le **contrôle d'accès** : cacher les détails internes de l'implémentation d'un objet et n'exposer qu'une interface publique et contrôlée.

**Analogies :**

- **Une voiture :** Vous interagissez avec une interface publique (volant, pédales, levier de vitesse). Vous n'avez pas besoin de connaître le fonctionnement interne du moteur pour la conduire. Le moteur est "encapsulé".
- **Une API :** Vous utilisez les points d'accès (endpoints) documentés sans avoir à connaître le code du serveur.

**Avantages :**

- **Sécurité :** Empêche les modifications invalides de l'état interne d'un objet.
- **Maintenance :** Permet de modifier l'implémentation interne d'une classe sans casser le code qui l'utilise, tant que l'interface publique ne change pas.
- **Simplicité :** L'utilisateur de la classe n'a qu'à se soucier de l'interface publique.

## 2. Conventions de Nommage en Python

Contrairement à des langages comme Java ou C++, Python n'a pas de mots-clés `private` ou `public` pour forcer le contrôle d'accès. Tout est basé sur des **conventions de nommage**.

### a. Attributs Publics

Par défaut, tous les attributs et méthodes sont publics.

```python
class CompteBancaire:
    def __init__(self, solde_initial):
        self.solde = solde_initial # Attribut public

compte = CompteBancaire(1000)
print(compte.solde) # Accès direct autorisé

# Le problème : on peut assigner une valeur invalide
compte.solde = -500 # L'état de l'objet est maintenant incohérent
print(compte.solde)
```

### b. Attributs "Protégés" (un seul underscore `_`)

Un attribut préfixé par un seul underscore (`_`) est, par convention, destiné à un **usage interne**. C'est un signal pour les autres développeurs : "Ne touchez pas à cet attribut directement depuis l'extérieur de la classe, sauf si vous savez ce que vous faites."

Python **ne bloque pas** l'accès à ces attributs. C'est une simple convention.

```python
class CompteBancaire:
    def __init__(self, solde_initial):
        self._solde = solde_initial # Attribut "protégé"

    def deposer(self, montant):
        if montant > 0:
            self._solde += montant
            print(f"Dépôt de {montant}€ effectué. Nouveau solde : {self._solde}€")

    def get_solde(self):
        # On fournit une méthode publique pour lire le solde
        return self._solde

compte = CompteBancaire(1000)

# On utilise la méthode publique
compte.deposer(200)

# On peut toujours y accéder directement, mais on ne devrait pas
print(compte._solde) # 1200
```

Cette convention est surtout utilisée dans le contexte de l'héritage, pour indiquer qu'un attribut peut être utilisé par les classes filles.

### c. Attributs "Privés" (deux underscores `__`)

Un attribut préfixé par deux underscores (`__`) est traité différemment par Python. Ce mécanisme s'appelle le **"Name Mangling"** (décoration de nom).

Python renomme automatiquement l'attribut pour le rendre plus difficile d'accès depuis l'extérieur. Il le transforme en `_NomDeLaClasse__nom_attribut`.

```python
class MaClasse:
    def __init__(self):
        self.__variable_privee = 42

    def get_variable(self):
        return self.__variable_privee

obj = MaClasse()

# Essayer d'y accéder directement lève une AttributeError
# print(obj.__variable_privee) # AttributeError: 'MaClasse' object has no attribute '__variable_privee'

# On peut toujours y accéder si on connaît le nom "décoré"
print(obj._MaClasse__variable_privee) # 42

# L'accès normal se fait via une méthode publique
print(obj.get_variable()) # 42
```

**Pourquoi ce mécanisme ?**
Le but principal du "name mangling" n'est pas tant la sécurité que d'**éviter les conflits de noms dans le contexte de l'héritage**. Si une classe fille définit un attribut avec le même nom `__variable_privee`, il sera lui aussi renommé (`_ClasseFille__variable_privee`) et n'écrasera pas l'attribut de la classe mère.

## 3. Getters, Setters et Propriétés

Pour contrôler l'accès aux attributs, on utilise des méthodes publiques :

- **Getter** : Une méthode pour lire la valeur d'un attribut (ex: `get_solde()`).
- **Setter** : Une méthode pour modifier la valeur d'un attribut, en y ajoutant une logique de validation (ex: `set_solde()`).

```python
class Personne:
    def __init__(self, nom, age):
        self.nom = nom
        self._age = age # On le met en "protégé"

    # Getter pour l'âge
    def get_age(self):
        return self._age

    # Setter pour l'âge
    def set_age(self, nouvel_age):
        if 0 < nouvel_age < 130:
            self._age = nouvel_age
        else:
            print("Erreur : L'âge est invalide.")

p = Personne("Alice", 30)
print(p.get_age()) # 30

p.set_age(31)
print(p.get_age()) # 31

p.set_age(-5) # Erreur : L'âge est invalide.
print(p.get_age()) # 31 (l'âge n'a pas changé)
```

### La manière "Pythonic" : les Propriétés (`@property`)

L'utilisation de getters et setters explicites (`get_...`, `set_...`) est un peu lourde et n'est pas très "Pythonic". Python offre une syntaxe beaucoup plus élégante pour cela : le décorateur `@property`.

Une propriété permet de définir des méthodes getter, setter et deleter pour un attribut, tout en conservant une syntaxe d'accès simple et directe.

```python
class Personne:
    def __init__(self, nom, age):
        self.nom = nom
        self._age = age # L'attribut réel qui stocke la donnée

    @property
    def age(self):
        """Ceci est le 'getter'. Il est appelé quand on fait obj.age"""
        print("(Accès en lecture à l'âge)")
        return self._age

    @age.setter
    def age(self, nouvel_age):
        """Ceci est le 'setter'. Il est appelé quand on fait obj.age = valeur"""
        print(f"(Tentative de modification de l'âge à {nouvel_age})")
        if 0 < nouvel_age < 130:
            self._age = nouvel_age
        else:
            print("Erreur : L'âge est invalide.")

p = Personne("Bob", 40)

# Accès en lecture (appelle le getter)
current_age = p.age # (Accès en lecture à l'âge)
print(f"L'âge de Bob est {current_age}")

# Accès en écriture (appelle le setter)
p.age = 41 # (Tentative de modification de l'âge à 41)

p.age = -10 # (Tentative de modification de l'âge à -10) -> Erreur : L'âge est invalide.
print(f"L'âge final de Bob est {p.age}")
```

Avec `@property`, on combine le meilleur des deux mondes :

- On a une syntaxe d'accès simple et naturelle (`p.age`).
- On bénéficie du contrôle et de la validation offerts par les méthodes.

## Conclusion

L'encapsulation en Python repose sur des conventions (`_`) et un mécanisme pour éviter les conflits de noms (`__`). Bien que Python ne l'impose pas de manière stricte, c'est un principe de conception essentiel pour écrire du code POO propre et maintenable. En cachant les détails d'implémentation et en exposant une interface contrôlée, idéalement via des propriétés (`@property`), vous rendez vos classes plus robustes, plus sûres et plus faciles à faire évoluer.
