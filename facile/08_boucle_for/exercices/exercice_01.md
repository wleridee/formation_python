# Exercice 01 : Le Compteur de Voyelles

## Objectif

Cet exercice a pour but de vous faire utiliser une boucle `for` pour itérer sur une chaîne de caractères et une condition `if` pour compter les occurrences de certains caractères.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `vowel_counter.py`.

2.  **Initialisez les variables suivantes** :

    - Une variable `sentence` contenant la chaîne de caractères : `"Python is a versatile language"`.
    - Une variable `vowels` contenant la chaîne de caractères `"aeiou"`.
    - Une variable `vowel_count` initialisée à `0`.

3.  **Écrivez une boucle `for`** qui itère sur chaque caractère de la variable `sentence`.

    - **Important** : Pour éviter les problèmes de casse (majuscules/minuscules), il est plus simple de travailler avec une version entièrement en minuscules de la phrase.

4.  **À l'intérieur de la boucle**, utilisez une condition `if` pour vérifier si le caractère actuel est une voyelle.

    - _Astuce :_ Vous pouvez vérifier si un caractère est présent dans une autre chaîne avec l'opérateur `in`. Par exemple : `if char in vowels:`.

5.  **Si le caractère est une voyelle**, incrémentez le compteur `vowel_count` de 1.

6.  **Après la boucle**, affichez le résultat final de manière claire.

## Résultat Attendu

La sortie de votre script doit être :

```
The sentence is: 'Python is a versatile language'
The number of vowels is: 9
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# vowel_counter.py

# 1. Initialisation
sentence = "Python is a versatile language"
vowels = "aeiou"
vowel_count = 0

# 2. Boucle et comptage
# On itère sur la version en minuscules de la phrase pour simplifier la comparaison.
for char in sentence.lower():
    # On vérifie si le caractère est dans notre chaîne de voyelles
    if char in vowels:
        # Si oui, on incrémente le compteur
        vowel_count += 1

# 3. Affichage du résultat
print(f"The sentence is: '{sentence}'")
print(f"The number of vowels is: {vowel_count}")

```

</details>
