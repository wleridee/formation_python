# Module : La Boucle `while`

## 1. Quoi : La Boucle `while`

La boucle `while` (qui signifie "tant que") exécute un bloc de code de manière répétée **tant qu'une condition spécifiée reste vraie**.

Contrairement à la boucle `for` qui itère sur une séquence finie, la boucle `while` est utilisée lorsque le nombre d'itérations n'est pas connu à l'avance.

## 2. Pourquoi : Gérer les conditions indéterminées

La boucle `while` est idéale pour des scénarios comme :

- Attendre une entrée utilisateur valide.
- Exécuter un programme jusqu'à ce que l'utilisateur décide de quitter.
- Traiter des données provenant d'une source en temps réel (ex: un flux réseau) jusqu'à ce qu'un signal d'arrêt soit reçu.
- Implémenter des algorithmes où la condition d'arrêt dépend du résultat des calculs précédents.

## 3. Comment : La Syntaxe

La syntaxe de base est : `while <condition>:`

```python
# On initialise une variable de contrôle AVANT la boucle.
count = 0

# La boucle continuera tant que 'count' est inférieur à 5.
while count < 5:
    print(f"Current count: {count}")
    # ⚠️ Très important : On doit modifier la variable de contrôle
    # à l'intérieur de la boucle pour éviter une boucle infinie.
    count += 1

print("Loop finished.")
```

**Sortie :**

```
Current count: 0
Current count: 1
Current count: 2
Current count: 3
Current count: 4
Loop finished.
```

### Le Piège de la Boucle Infinie

Si la condition de la boucle `while` ne devient jamais `False`, la boucle s'exécutera pour toujours. C'est une erreur courante.

```python
# ❌ Mauvais : Boucle infinie !
# 'is_running' est toujours True, la condition ne change jamais.
is_running = True
while is_running:
    print("Running...")
```

Pour arrêter une boucle infinie dans votre terminal, vous devez généralement utiliser `Ctrl+C`.

### Utilisation de `break` et `continue`

Les instructions `break` et `continue` fonctionnent de la même manière que dans une boucle `for`.

- `break` : Sort immédiatement de la boucle `while`.
- `continue` : Saute le reste de l'itération actuelle et retourne à la vérification de la condition `while`.

`break` est souvent utilisé pour créer une "boucle infinie contrôlée", ce qui est un modèle de conception courant.

```python
# Modèle de boucle "infinie contrôlée"
while True: # Crée une boucle qui s'exécute pour toujours...
    user_input = input("Enter a command (or 'quit' to exit): ")

    if user_input == "quit":
        print("Exiting program.")
        break # ...jusqu'à ce que cette condition soit remplie.

    if user_input == "hello":
        print("Hello to you too!")
        continue # On a traité la commande, on repart au début de la boucle.

    print(f"Unknown command: '{user_input}'")
```

### Le bloc `else` dans une boucle `while`

Similaire à la boucle `for`, le bloc `else` d'une boucle `while` s'exécute **uniquement si la boucle se termine parce que sa condition est devenue fausse**, et non à cause d'un `break`.

```python
countdown = 5
while countdown > 0:
    print(countdown)
    countdown -= 1
else:
    # Ce bloc s'exécute car la boucle s'est terminée
    # naturellement (countdown n'est plus > 0).
    print("Liftoff!")
```

## 4. `for` vs `while` : Quand utiliser quoi ?

- **Utilisez une boucle `for`** quand :

  - Vous avez une séquence existante (liste, chaîne, etc.) à parcourir.
  - Vous savez exactement combien de fois vous voulez que la boucle s'exécute (en utilisant `range()`).
  - C'est le cas le plus courant en Python.

- **Utilisez une boucle `while`** quand :
  - Vous ne savez pas combien d'itérations seront nécessaires.
  - La condition d'arrêt dépend d'un événement externe (comme une entrée utilisateur) ou d'un état qui change à l'intérieur de la boucle.

✅ **Bonne pratique** : Préférez toujours une boucle `for` si vous le pouvez. Elle est généralement plus simple, plus lisible et moins sujette aux erreurs de boucles infinies.
