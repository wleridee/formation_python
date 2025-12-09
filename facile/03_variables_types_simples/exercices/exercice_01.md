# Exercice 01 : Déclaration et Manipulation de Variables

## Objectif

Cet exercice a pour but de vous familiariser avec la déclaration de variables de différents types simples et l'affichage de leurs valeurs et de leurs types.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `variables_practice.py`.

2.  **Déclarez les variables suivantes** dans ce fichier :

    - Une variable `movie_title` contenant le nom de votre film préféré (type `str`).
    - Une variable `release_year` contenant son année de sortie (type `int`).
    - Une variable `movie_rating` contenant sa note sur 10 (utilisez un nombre à virgule, type `float`).
    - Une variable `is_classic` qui est `True` si le film est sorti avant l'an 2000, et `False` sinon (type `bool`).

3.  **Affichez les informations** :

    - Utilisez la fonction `print()` pour afficher la valeur de chaque variable.
    - Ajoutez du texte à vos `print()` pour que la sortie soit lisible. Par exemple : `print("Titre du film :", movie_title)`.

4.  **Vérifiez les types** :

    - Utilisez la fonction `print()` en combinaison avec la fonction `type()` pour afficher le type de chaque variable.
    - Par exemple : `print("Type de la variable movie_title :", type(movie_title))`.

5.  **Exécutez votre script** depuis le terminal :
    ```bash
    python variables_practice.py
    ```

## Résultat Attendu

La sortie dans votre terminal devrait ressembler à ceci (avec les valeurs de votre film préféré) :

```
Titre du film : Inception
Année de sortie : 2010
Note : 8.8
Est un classique ? : False
---
Type de la variable movie_title : <class 'str'>
Type de la variable release_year : <class 'int'>
Type de la variable movie_rating : <class 'float'>
Type de la variable is_classic : <class 'bool'>
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# variables_practice.py

# 1. Déclaration des variables
movie_title = "The Matrix"
release_year = 1999
movie_rating = 8.7
is_classic = True # Le film est sorti avant 2000

# 2. Affichage des valeurs
print("--- Informations sur le film ---")
print("Titre du film :", movie_title)
print("Année de sortie :", release_year)
print("Note :", movie_rating)
print("Est un classique ? :", is_classic)

print("\n--- Types des variables ---") # \n crée un saut de ligne pour la lisibilité

# 3. Affichage des types
print("Type de la variable movie_title :", type(movie_title))
print("Type de la variable release_year :", type(release_year))
print("Type de la variable movie_rating :", type(movie_rating))
print("Type de la variable is_classic :", type(is_classic))
```

</details>
