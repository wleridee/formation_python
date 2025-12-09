# Exercice 01 : Comptage de la Fréquence des Mots

## Objectif

Cet exercice a pour but de vous faire utiliser un dictionnaire pour compter la fréquence d'apparition de chaque mot dans une phrase. C'est un problème classique en traitement de texte.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `word_frequency.py`.

2.  **Initialisez la variable suivante** :

    ```python
    text = "this is a simple sentence with simple words this is not complex"
    ```

3.  **Préparez le texte** :

    - Pour pouvoir compter les mots, vous devez d'abord transformer la chaîne de caractères en une **liste de mots**. Utilisez la méthode `.split()` des chaînes de caractères, qui sépare la chaîne par les espaces.

    ```python
    words = text.split()
    ```

4.  **Initialisez un dictionnaire vide** nommé `frequency_counter` qui stockera les mots comme clés et leur fréquence comme valeurs.

5.  **Écrivez une boucle `for`** qui itère sur votre liste `words`.

    - À l'intérieur de la boucle, pour chaque `word` :
      - Vérifiez si le mot est **déjà une clé** dans votre `frequency_counter`.
      - S'il y est, **incrémentez** sa valeur (son compteur) de 1.
      - S'il n'y est pas, **ajoutez-le** au dictionnaire avec une valeur de 1.
    - _Astuce :_ La méthode `.get(key, 0)` est parfaite ici. `frequency_counter.get(word, 0)` retournera la valeur actuelle du mot s'il existe, ou `0` s'il n'existe pas.

6.  **Après la boucle**, affichez le dictionnaire de fréquences.

## Résultat Attendu

La sortie de votre script doit être (l'ordre peut varier si vous utilisez une version de Python antérieure à 3.7) :

```
Word Frequencies:
{'this': 2, 'is': 2, 'a': 1, 'simple': 2, 'sentence': 1, 'with': 1, 'words': 1, 'not': 1, 'complex': 1}
```

## Bonus (Optionnel)

Affichez les résultats de manière plus lisible, un mot par ligne, trié par ordre alphabétique des mots.

- _Astuce :_ Vous pouvez obtenir une liste triée des clés d'un dictionnaire avec `sorted(my_dict)`.

### Résultat Attendu (avec bonus)

```
Word Frequencies (sorted):
 - a: 1
 - complex: 1
 - is: 2
 - not: 1
 - sentence: 1
 - simple: 2
 - this: 2
 - with: 1
 - words: 1
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# word_frequency.py

text = "this is a simple sentence with simple words this is not complex"

# 1. Préparation du texte
words = text.split()

# 2. Initialisation du dictionnaire
frequency_counter = {}

# 3. Boucle et comptage
for word in words:
    # La méthode .get(word, 0) retourne la valeur actuelle si 'word' est dans le dict,
    # sinon elle retourne 0. On ajoute ensuite 1 au résultat.
    # C'est une manière très concise de gérer les deux cas (clé existante ou non).
    frequency_counter[word] = frequency_counter.get(word, 0) + 1

# 4. Affichage du résultat
print("Word Frequencies:")
print(frequency_counter)


# --- BONUS ---
print("\nWord Frequencies (sorted):")
# On récupère les clés, on les trie, puis on itère sur cette liste triée
for word in sorted(frequency_counter):
    # On accède à la valeur correspondante dans le dictionnaire original
    count = frequency_counter[word]
    print(f" - {word}: {count}")
```

</details>
