# Module : Générateurs Avancés et Coroutines Simples

## Objectif

Ce module a pour but d'explorer les fonctionnalités avancées des générateurs en Python, notamment la méthode `send()`, qui transforme un simple générateur en une **coroutine**. Cela jette les bases pour comprendre les concepts plus modernes de `async/await`.

## 1. Rappel sur les Générateurs

Un générateur est une fonction qui utilise le mot-clé `yield` pour produire une série de valeurs de manière paresseuse (une à la fois), sans stocker toute la séquence en mémoire.

```python
def simple_generator():
    yield 1
    yield 2
    yield 3

gen = simple_generator()
print(next(gen)) # 1
print(next(gen)) # 2
print(next(gen)) # 3
```

Chaque appel à `next(gen)` exécute le code jusqu'au prochain `yield`, retourne la valeur, et met l'état du générateur en pause.

## 2. Plus qu'un simple `yield` : `(yield)`

L'instruction `yield` peut aussi être utilisée comme une **expression**. Lorsqu'elle est utilisée de cette manière, elle peut _recevoir_ des valeurs de l'extérieur.

La syntaxe est `variable = (yield)`. Les parenthèses sont importantes.

```python
def simple_coroutine():
    print("Coroutine démarrée.")
    valeur_recue = (yield) # Le générateur se met en pause ici, en attente d'une valeur.
    print(f"Coroutine a reçu : {valeur_recue}")
    (yield) # Se met en pause une dernière fois avant de terminer.

# --- Interaction ---

# 1. Créer la coroutine (générateur)
coro = simple_coroutine()

# 2. Démarrer la coroutine
# On doit appeler next() une première fois pour avancer jusqu'au premier 'yield'.
# C'est ce qu'on appelle "amorcer" (priming) la coroutine.
next(coro)
# Output: Coroutine démarrée.

# 3. Envoyer une valeur à la coroutine
# La méthode send() envoie une valeur qui devient le résultat de l'expression (yield).
# Le code de la coroutine reprend alors jusqu'au prochain 'yield'.
coro.send("Hello World")
# Output: Coroutine a reçu : Hello World
```

**Flux d'exécution détaillé :**

1.  `coro = simple_coroutine()`: Crée l'objet générateur, mais n'exécute aucun code.
2.  `next(coro)`: Exécute le code de `simple_coroutine` jusqu'à la ligne `valeur_recue = (yield)`. Le générateur se met en pause _avant_ l'assignation.
3.  `coro.send("Hello World")`: La valeur `"Hello World"` est envoyée au point de pause. Elle est assignée à `valeur_recue`. Le code reprend, la ligne `print(f"Coroutine a reçu : {valeur_recue}")` s'exécute, puis le générateur atteint le `(yield)` final et se remet en pause.

## 3. La Méthode `.send()`

La méthode `send(valeur)` fait deux choses :

1.  Elle envoie une valeur au générateur, qui devient la valeur de l'expression `(yield)`.
2.  Elle continue l'exécution du générateur jusqu'au prochain `yield` et retourne la valeur produite par ce `yield`.

`next(generateur)` est équivalent à `generateur.send(None)`.

Voyons un exemple plus interactif : un "running average".

```python
def running_average():
    """Coroutine qui calcule une moyenne glissante."""
    total = 0.0
    count = 0
    average = None
    while True:
        # Le générateur produit la moyenne actuelle et attend une nouvelle valeur.
        nouvelle_valeur = (yield average)
        total += nouvelle_valeur
        count += 1
        average = total / count

# --- Utilisation ---
avg_coro = running_average()

# 1. Amorcer la coroutine. Elle s'exécute jusqu'au premier (yield average).
# 'average' est None à ce stade, donc next() retourne None.
print(f"Amorçage : {next(avg_coro)}") # ou avg_coro.send(None)

# 2. Envoyer des valeurs et recevoir la moyenne en retour.
print(f"Envoi de 10, Moyenne : {avg_coro.send(10):.2f}") # Moyenne : 10.00
print(f"Envoi de 20, Moyenne : {avg_coro.send(20):.2f}") # Moyenne : 15.00
print(f"Envoi de 3, Moyenne : {avg_coro.send(3):.2f}")  # Moyenne : 11.00
```

Cette structure permet de créer des "pipelines" de traitement de données où les générateurs agissent comme des unités de traitement qui reçoivent, transforment et envoient des données.

## 4. Fermer et Lancer des Exceptions dans les Générateurs

### a. `.close()`

La méthode `close()` permet de terminer un générateur proprement. Quand elle est appelée, une exception `GeneratorExit` est levée à l'intérieur du générateur, au point où il était en pause.

```python
def simple_coroutine():
    print("Coroutine démarrée.")
    try:
        while True:
            valeur = (yield)
            print(f"Reçu : {valeur}")
    except GeneratorExit:
        print("Coroutine se ferme.")

coro = simple_coroutine()
next(coro)
coro.send(10)
coro.close()

# Output:
# Coroutine démarrée.
# Reçu : 10
# Coroutine se ferme.
```

C'est utile pour s'assurer que les ressources (comme des fichiers ou des connexions) sont bien libérées.

### b. `.throw()`

La méthode `throw(type_exception, valeur, traceback)` permet de lever une exception à l'intérieur du générateur, au point où il est en pause.

```python
def gestion_erreur_coro():
    print("Démarrage...")
    while True:
        try:
            valeur = (yield)
            print(f"Reçu : {valeur}")
        except ValueError:
            print("--- Erreur de valeur gérée ! ---")

coro = gestion_erreur_coro()
next(coro)
coro.send(10)
coro.send(20)
coro.throw(ValueError) # Lève une ValueError dans le générateur
coro.send(30)

# Output:
# Démarrage...
# Reçu : 10
# Reçu : 20
# --- Erreur de valeur gérée ! ---
# Reçu : 30
```

Si l'exception n'est pas attrapée par le `try...except` à l'intérieur du générateur, elle se propage à l'appelant (celui qui a fait `.throw()`).

## 5. `yield from` : Déléguer à un Sous-Générateur

Introduit en Python 3.3, `yield from` simplifie grandement le code lorsqu'un générateur doit en appeler un autre. Il crée un canal transparent entre l'appelant extérieur et le sous-générateur.

Sans `yield from` :

```python
def sous_generateur():
    yield 1
    yield 2

def generateur_principal_complique():
    # On doit manuellement itérer et re-yield
    for item in sous_generateur():
        yield item
```

Avec `yield from` :

```python
def sous_generateur():
    val = (yield "Prêt à recevoir")
    print(f"Sous-générateur a reçu: {val}")
    return "Résultat du sous-générateur"

def generateur_principal():
    # yield from gère tout : amorçage, envoi de valeurs, réception de la valeur de retour
    resultat = yield from sous_generateur()
    print(f"Le sous-générateur a retourné : {resultat}")

# --- Utilisation ---
g = generateur_principal()

# 1. Amorçage : next(g) avance jusqu'au yield dans le sous_generateur
message = next(g)
print(f"Message du sous-générateur : {message}") # Prêt à recevoir

# 2. Envoi de valeur : g.send() envoie directement au sous_generateur
try:
    g.send("Valeur envoyée")
except StopIteration:
    # Quand le sous-générateur se termine avec 'return', une StopIteration est levée.
    # Le generateur_principal attrape cette exception et récupère la valeur de retour.
    pass

# Output:
# Message du sous-générateur : Prêt à recevoir
# Sous-générateur a reçu: Valeur envoyée
# Le sous-générateur a retourné : Résultat du sous-générateur
```

`yield from` gère automatiquement :

- La transmission des `send()` et `next()` au sous-générateur.
- La transmission des `yield` du sous-générateur à l'appelant.
- La gestion des `throw()` et `close()`.
- La récupération de la valeur de `return` du sous-générateur.

## Conclusion

Les générateurs avancés, avec `send()`, `throw()`, `close()` et `yield from`, sont le fondement historique des coroutines en Python. Ils permettent de créer des pipelines de traitement de données complexes et des systèmes basés sur des événements. Bien que la syntaxe `async/await` (qui est basée sur ces concepts) soit aujourd'hui la manière standard d'écrire du code asynchrone, comprendre le fonctionnement interne des générateurs en tant que coroutines est essentiel pour une maîtrise complète de la programmation concurrente en Python.
