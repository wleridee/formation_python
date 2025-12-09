# Exercice 01 : Le Jeu du Juste Prix

## Objectif

Cet exercice a pour but de vous faire utiliser une boucle `while` pour créer un petit jeu interactif où l'utilisateur doit deviner un nombre secret.

## Contexte

Vous allez programmer un jeu simple :

1.  Le programme choisit un nombre secret.
2.  L'utilisateur a un nombre limité de tentatives pour deviner ce nombre.
3.  Après chaque tentative, le programme indique si le nombre à deviner est plus grand ou plus petit.
4.  La partie se termine si l'utilisateur trouve le nombre ou s'il n'a plus de tentatives.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `guessing_game.py`.

2.  **Initialisez les variables du jeu** :

    ```python
    secret_number = 42
    max_attempts = 5
    current_attempt = 0
    ```

3.  **Écrivez une boucle `while`** qui continue tant que l'utilisateur a encore des tentatives (`current_attempt < max_attempts`).

4.  **À l'intérieur de la boucle** :
    a. Incrémentez `current_attempt` de 1.
    b. Utilisez la fonction `input()` pour demander à l'utilisateur de deviner un nombre. Affichez le numéro de la tentative actuelle. - _Astuce :_ `input()` retourne toujours une **chaîne de caractères**. Vous devrez la convertir en nombre entier avec `int()`.
    c. Utilisez une structure `if`/`elif`/`else` pour comparer la devinette de l'utilisateur (`guess`) avec le `secret_number` : - Si `guess` est plus petit que `secret_number`, affichez "Too low!". - Si `guess` est plus grand que `secret_number`, affichez "Too high!". - Si `guess` est égal à `secret_number`, affichez un message de victoire et **arrêtez la boucle** avec `break`.

5.  **Après la boucle**, ajoutez une condition pour vérifier si le joueur a gagné ou perdu.
    - Si le joueur a trouvé le bon nombre, il a déjà reçu son message de victoire.
    - Si la boucle s'est terminée parce que le nombre maximum de tentatives a été atteint, affichez un message de défaite révélant le nombre secret.
    - _Astuce :_ Vous pouvez utiliser une variable "drapeau" (flag) ou simplement vérifier si le nombre de tentatives a atteint le maximum.

## Résultat Attendu

Voici un exemple de partie où l'utilisateur gagne :

```
--- Guessing Game ---
Attempt 1/5 - Enter your guess: 50
Too high!
Attempt 2/5 - Enter your guess: 25
Too low!
Attempt 3/5 - Enter your guess: 40
Too low!
Attempt 4/5 - Enter your guess: 42
Congratulations! You found the secret number 42 in 4 attempts!
```

Voici un exemple de partie où l'utilisateur perd :

```
--- Guessing Game ---
Attempt 1/5 - Enter your guess: 1
Too low!
Attempt 2/5 - Enter your guess: 2
Too low!
Attempt 3/5 - Enter your guess: 3
Too low!
Attempt 4/5 - Enter your guess: 4
Too low!
Attempt 5/5 - Enter your guess: 5
Too low!
Sorry, you've run out of attempts. The secret number was 42.
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# guessing_game.py

secret_number = 42
max_attempts = 5
current_attempt = 0
has_won = False # Variable "drapeau" pour savoir si le joueur a gagné

print("--- Guessing Game ---")

while current_attempt < max_attempts:
    current_attempt += 1

    # Demande à l'utilisateur et convertit l'entrée en entier
    guess_str = input(f"Attempt {current_attempt}/{max_attempts} - Enter your guess: ")
    guess = int(guess_str)

    # Compare la devinette au nombre secret
    if guess < secret_number:
        print("Too low!")
    elif guess > secret_number:
        print("Too high!")
    else: # guess == secret_number
        print(f"Congratulations! You found the secret number {secret_number} in {current_attempt} attempts!")
        has_won = True
        break # Sort de la boucle car le joueur a gagné

# Après la boucle, on vérifie si le joueur n'a PAS gagné
if not has_won:
    print(f"Sorry, you've run out of attempts. The secret number was {secret_number}.")

```

</details>
