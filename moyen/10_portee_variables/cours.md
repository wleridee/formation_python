# Module : La Portée des Variables (Scope)

## Objectif

Ce module a pour but de vous faire comprendre le concept de portée des variables (scope) en Python. Vous apprendrez comment Python détermine où chercher une variable et la règle LEGB (Local, Enclosing, Global, Built-in) qui régit ce comportement.

## 1. Qu'est-ce que la Portée (Scope) ?

La **portée** d'une variable est la partie du code où cette variable est accessible. Python ne permet pas d'accéder à n'importe quelle variable depuis n'importe où.

```python
def ma_fonction():
    # 'x' est une variable locale à ma_fonction
    x = 10
    print(x)

ma_fonction() # Affiche 10

# Essayer d'accéder à 'x' ici lèvera une NameError
# car 'x' n'existe que dans le scope de ma_fonction.
# print(x) # NameError: name 'x' is not defined
```

## 2. La Règle LEGB

Python utilise la règle **LEGB** pour résoudre les noms de variables. Quand vous utilisez une variable, Python la cherche dans l'ordre suivant :

1.  **L - Local** : La portée la plus interne, qui contient les noms définis à l'intérieur de la fonction actuelle (et qui ne sont pas déclarés `global` ou `nonlocal`).
2.  **E - Enclosing (Englobante)** : La portée des fonctions englobantes (closures). Si une fonction est définie à l'intérieur d'une autre fonction, la fonction interne peut accéder aux variables de la fonction externe.
3.  **G - Global** : La portée au niveau du module. Ce sont les noms définis au plus haut niveau d'un script ou d'un module.
4.  **B - Built-in** : La portée la plus externe, qui contient les noms prédéfinis en Python (`print`, `len`, `str`, etc.).

Python s'arrête à la première occurrence qu'il trouve.

### Exemple illustrant la règle LEGB

```python
# B - Built-in (print est une fonction built-in)
# G - Global
x_global = "Je suis globale"

def fonction_externe():
    # E - Enclosing
    x_enclosing = "Je suis dans la portée englobante"

    def fonction_interne():
        # L - Local
        x_local = "Je suis locale"

        # Python cherche 'x_local' : il le trouve dans la portée locale (L).
        print(f"Interne voit local: {x_local}")

        # Python cherche 'x_enclosing' : il ne le trouve pas en (L),
        # il cherche donc en (E) et le trouve.
        print(f"Interne voit enclosing: {x_enclosing}")

        # Python cherche 'x_global' : il ne le trouve ni en (L) ni en (E),
        # il cherche donc en (G) et le trouve.
        print(f"Interne voit global: {x_global}")

    fonction_interne()

fonction_externe()
```

## 3. Modifier des Variables de Portées Supérieures

Par défaut, vous pouvez lire des variables de portées supérieures, mais pas les modifier. Si vous assignez une valeur à une variable dans une fonction, Python suppose que c'est une nouvelle variable locale.

### Le mot-clé `global`

Pour modifier une variable de la portée globale depuis une fonction, vous devez utiliser le mot-clé `global`.

```python
compteur = 0 # Variable globale

def incrementer():
    # Indique à Python que 'compteur' est la variable de la portée globale
    global compteur
    compteur += 1
    print(f"Dans la fonction : {compteur}")

incrementer()
incrementer()
print(f"Hors de la fonction : {compteur}")

# Output:
# Dans la fonction : 1
# Dans la fonction : 2
# Hors de la fonction : 2
```

**Attention :** L'usage excessif de `global` est souvent considéré comme une mauvaise pratique car il rend le code plus difficile à suivre et à déboguer.

### Le mot-clé `nonlocal`

Le mot-clé `nonlocal` est similaire à `global`, mais il est utilisé dans les fonctions imbriquées pour indiquer qu'une variable fait référence à une variable de la portée englobante (Enclosing), et non globale.

```python
def fonction_externe():
    compteur_englobant = 0

    def fonction_interne():
        # Indique que 'compteur_englobant' est celui de la portée (E)
        nonlocal compteur_englobant
        compteur_englobant += 1
        return compteur_englobant

    return fonction_interne # Retourne la fonction interne (closure)

# 'compteur' est maintenant une closure qui "se souvient" de son compteur_englobant
compteur = fonction_externe()

print(compteur()) # 1
print(compteur()) # 2
print(compteur()) # 3
```

`nonlocal` est essentiel pour créer des fonctions avec état, comme des compteurs ou des accumulateurs, sans utiliser de classes.

## 4. Portée des Blocs `if`, `for`, `while`

Contrairement à de nombreux autres langages (comme C++ ou Java), les blocs `if`, `for`, et `while` en Python **ne créent pas de nouvelle portée locale**.

```python
for i in range(5):
    x = i * 10

# 'i' et 'x' sont toujours accessibles ici, après la fin de la boucle.
print(f"Après la boucle, i = {i}") # i = 4
print(f"Après la boucle, x = {x}") # x = 40

if True:
    y = "visible"

print(y) # "visible"
```

Seules les **fonctions (`def`, `lambda`), les classes (`class`) et les modules** créent de nouvelles portées en Python.

Les compréhensions de liste (depuis Python 3) ont leur propre portée pour la variable d'itération, ce qui évite les fuites.

```python
z = 100
ma_liste = [z for z in range(5)]

print(ma_liste) # [0, 1, 2, 3, 4]
# La variable 'z' de la compréhension n'a pas écrasé la variable 'z' externe.
print(z) # 100
```

## Conclusion

Comprendre la portée des variables est fondamental pour écrire du code Python correct et éviter des bugs subtils. La règle LEGB est le modèle mental à garder en tête pour savoir où Python va chercher une variable. Les mots-clés `global` et `nonlocal` sont les outils qui vous permettent de briser la règle de lecture seule et de modifier des variables dans des portées supérieures, mais ils doivent être utilisés avec discernement. Enfin, rappelez-vous que seuls `def`, `class` et les modules créent de vraies nouvelles portées.
