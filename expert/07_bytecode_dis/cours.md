# Module : Le Bytecode Python et le module `dis`

## Objectif

Ce module a pour but de vous faire plonger dans les rouages internes de l'interpréteur Python. Vous apprendrez ce qu'est le bytecode, comment Python compile votre code source en bytecode, et comment utiliser le module `dis` (disassembler) pour inspecter ces instructions de bas niveau.

## 1. De la Source au Bytecode

Lorsque vous exécutez un script Python, l'interpréteur ne travaille pas directement avec votre code source. Il y a une étape de compilation intermédiaire.

Le processus est le suivant :

1.  **Code Source (`.py`)** : Le code que vous écrivez.
2.  **Compilation** : L'interpréteur CPython compile le code source en un ensemble d'instructions de plus bas niveau appelé **bytecode**.
3.  **Fichiers `.pyc`** : Pour éviter de recompiler les modules à chaque fois, Python met en cache le bytecode dans des fichiers `.pyc` situés dans un dossier `__pycache__`. Si le fichier source n'a pas changé, Python chargera directement le `.pyc`.
4.  **Machine Virtuelle Python (PVM)** : Le bytecode est ensuite exécuté par la PVM, qui est le véritable moteur d'exécution de Python. La PVM est une boucle qui lit chaque instruction de bytecode et l'exécute.

Le bytecode est une représentation intermédiaire, portable entre différentes plateformes (Windows, macOS, Linux), tant que la version de Python est la même.

## 2. Le Module `dis` : Désassembler le Bytecode

Le module `dis` est l'outil de la bibliothèque standard qui permet de "désassembler" un objet Python (fonction, méthode, classe, module) pour voir le bytecode qu'il a généré.

La fonction la plus simple est `dis.dis()`.

```python
import dis

def addition(a, b):
    resultat = a + b
    return resultat

# Désassembler la fonction 'addition'
dis.dis(addition)
```

La sortie ressemblera à ceci (les numéros de ligne et les indices peuvent varier légèrement) :

```
  4           0 LOAD_FAST                0 (a)
              2 LOAD_FAST                1 (b)
              4 BINARY_ADD
              6 STORE_FAST               2 (resultat)

  5           8 LOAD_FAST                2 (resultat)
             10 RETURN_VALUE
```

### Comprendre la Sortie de `dis`

Chaque ligne représente une instruction de bytecode :

- **Colonne 1 (numéro de ligne)** : Le numéro de la ligne dans le code source (`.py`) qui a généré cette instruction.
- **Colonne 2 (offset)** : L'indice (en octets) de l'instruction dans la séquence de bytecode.
- **Colonne 3 (Nom de l'instruction)** : Le nom de l'opération (l'opcode). C'est la partie la plus importante.
- **Colonne 4 (Argument de l'instruction)** : La plupart des instructions prennent un argument (un indice, une constante, un nom de variable).
- **Colonne 5 (Interprétation de l'argument)** : Une aide lisible pour comprendre l'argument.

### Analyse de l'exemple `addition`

Python exécute les fonctions en utilisant une pile (stack).

- `LOAD_FAST 0 (a)`: Charge la variable locale `a` (stockée à l'index 0) sur le dessus de la pile.
- `LOAD_FAST 1 (b)`: Charge la variable locale `b` (stockée à l'index 1) sur le dessus de la pile. La pile contient maintenant `[a, b]`.
- `BINARY_ADD`: Prend les deux éléments du dessus de la pile, les additionne, et pousse le résultat sur la pile. La pile contient maintenant `[a+b]`.
- `STORE_FAST 2 (resultat)`: Prend la valeur du dessus de la pile et la stocke dans la variable locale `resultat` (à l'index 2). La pile est maintenant vide.
- `LOAD_FAST 2 (resultat)`: Recharge la valeur de `resultat` sur la pile.
- `RETURN_VALUE`: Prend la valeur du dessus de la pile et la retourne comme résultat de la fonction.

## 3. Pourquoi le Bytecode est-il Intéressant ?

Inspecter le bytecode peut vous aider à comprendre pourquoi certaines constructions de code sont plus rapides que d'autres. Cela révèle le "coût" réel de vos instructions.

### Exemple 1 : Accès aux variables locales vs. globales

```python
import dis

x = 100 # Variable globale

def acces_local():
    y = 10
    return y

def acces_global():
    return x

print("--- Accès Local ---")
dis.dis(acces_local)
# LOAD_CONST 1 (10)
# STORE_FAST 0 (y)
# LOAD_FAST 0 (y)  <- Rapide

print("\n--- Accès Global ---")
dis.dis(acces_global)
# LOAD_GLOBAL 0 (x) <- Plus lent
```

- `LOAD_FAST` est très rapide car les variables locales sont stockées dans un tableau à taille fixe, et l'interpréteur y accède par un simple indice.
- `LOAD_GLOBAL` est plus lent car il implique une recherche dans le dictionnaire des variables globales.

**Leçon :** L'accès aux variables locales est plus rapide que l'accès aux variables globales. Dans une boucle très serrée, copier une variable globale dans une variable locale peut apporter un léger gain.

### Exemple 2 : Compréhension de liste vs. boucle `for`

```python
import dis

def creer_liste_for():
    ma_liste = []
    for i in range(10):
        ma_liste.append(i)
    return ma_liste

def creer_liste_comprehension():
    return [i for i in range(10)]

print("--- Boucle for ---")
dis.dis(creer_liste_for)
# ...
# FOR_ITER
# STORE_FAST
# LOAD_FAST
# LOAD_METHOD (append)
# LOAD_FAST
# CALL_METHOD
# JUMP_ABSOLUTE
# ...

print("\n--- Compréhension de liste ---")
dis.dis(creer_liste_comprehension)
# ...
# LOAD_CONST
# MAKE_FUNCTION
# LOAD_NAME (range)
# ...
# GET_ITER
# CALL_FUNCTION
# RETURN_VALUE
```

En inspectant le bytecode de `creer_liste_comprehension`, on verrait une instruction `LIST_APPEND` (dans les versions plus anciennes) ou un mécanisme interne plus optimisé (`MAKE_FUNCTION` pour créer un objet de compréhension). L'interpréteur est hautement optimisé pour les compréhensions. Il n'a pas besoin de résoudre la méthode `.append` à chaque itération.

**Leçon :** Les compréhensions de liste sont généralement plus rapides que la construction manuelle d'une liste avec une boucle `for` et `.append()`, car elles utilisent des opcodes plus spécialisés et plus rapides.

### Exemple 3 : L'instruction `+=`

L'opérateur `+=` n'est pas toujours le même. Son comportement dépend du type de l'objet.

```python
import dis

def ajout_immuable(t):
    t += (4,) # Crée un nouveau tuple
    return t

def ajout_muable(l):
    l += [4] # Modifie la liste en place
    return l

print("--- Tuple (immuable) ---")
dis.dis(ajout_immuable)
# LOAD_FAST 0 (t)
# LOAD_CONST 1 ((4,))
# INPLACE_ADD  <- Devient BINARY_ADD car tuple immuable
# STORE_FAST 0 (t)

print("\n--- Liste (muable) ---")
dis.dis(ajout_muable)
# LOAD_FAST 0 (l)
# LOAD_CONST 1 (4)
# BUILD_LIST 1
# INPLACE_ADD  <- Modifie la liste en place
# STORE_FAST 0 (l)
```

- Pour le tuple, `INPLACE_ADD` est équivalent à `BINARY_ADD`. Il crée un nouveau tuple et le réassigne à `t`.
- Pour la liste, `INPLACE_ADD` modifie l'objet liste existant directement, ce qui est beaucoup plus efficace que de créer une nouvelle liste.

**Leçon :** Le bytecode révèle que les opérations "en place" (`+=`, `*=`, etc.) sont optimisées pour les types muables, ce qui les rend très performantes.

## 4. Le Compilateur Optimiseur de Python (peephole optimizer)

Python effectue quelques optimisations simples au niveau du bytecode. Le "peephole optimizer" examine de courtes séquences d'instructions et les remplace par des séquences plus efficaces.

Un exemple classique est le pré-calcul des constantes.

```python
import dis

def calcul_constant():
    return 2 * 60 * 60 # 2 heures en secondes

dis.dis(calcul_constant)
```

Sortie :

```
  4           0 LOAD_CONST               1 (7200)
              2 RETURN_VALUE
```

Au lieu de générer des instructions pour faire deux multiplications, le compilateur a calculé le résultat `7200` et l'a stocké comme une constante.

**Leçon :** Le compilateur Python est capable d'effectuer des optimisations de base, comme le pliage de constantes (`constant folding`).

## Conclusion

Le bytecode est le langage de la Machine Virtuelle Python. Bien que vous n'ayez pas besoin de le lire au quotidien, le module `dis` est un outil de diagnostic puissant. Il vous permet de :

- Comprendre pourquoi un idiome Python est plus rapide qu'un autre.
- Voir comment Python gère les différents types de données et d'opérations.
- Apprécier les optimisations que l'interpréteur effectue pour vous.
- Acquérir une compréhension plus profonde du fonctionnement interne de Python, ce qui fait de vous un meilleur développeur.
