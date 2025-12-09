# Exercice 01 : Compteur de Mots dans un Fichier

## Objectif

Cet exercice a pour but de vous faire pratiquer la lecture de fichiers texte et la manipulation de chaînes de caractères pour compter la fréquence de chaque mot dans un fichier.

## Contexte

L'analyse de texte est une tâche courante en programmation. Une des opérations de base est de compter les occurrences de chaque mot pour analyser le contenu d'un texte. Vous allez écrire un script qui lit un fichier, le nettoie, et affiche les 10 mots les plus fréquents.

## Énoncé

### Étape 1 : Créer le fichier texte

Créez manuellement un fichier nommé `texte_a_analyser.txt` et copiez-y le contenu suivant :

```
Python est un langage de programmation puissant. Python est simple à apprendre.
Apprendre Python ouvre de nombreuses portes.
Le langage Python est utilisé dans le développement web, la science des données et l'intelligence artificielle.
La simplicité de Python est un de ses plus grands atouts.
```

### Étape 2 : Écrire le script Python

Créez un fichier Python nommé `compteur_mots.py`.

1.  **Définissez une fonction `compter_mots(nom_fichier)`**.

    - Cette fonction prend le nom du fichier en argument.
    - Elle doit retourner un dictionnaire où les clés sont les mots et les valeurs sont leur nombre d'occurrences.

2.  **Dans la fonction, lisez le fichier**.

    - Utilisez `with open(...)` pour ouvrir et lire le contenu du fichier. Assurez-vous de gérer le cas où le fichier n'existe pas avec un bloc `try...except FileNotFoundError`.

3.  **Nettoyez et séparez le texte en mots**.

    - Convertissez tout le texte en minuscules pour que "Python" et "python" soient comptés comme le même mot.
    - Supprimez les caractères de ponctuation simples comme le `.` et la `,`. Vous pouvez utiliser la méthode `replace()`.
    - Séparez le texte en une liste de mots en utilisant la méthode `split()`.

4.  **Comptez les mots**.

    - Initialisez un dictionnaire vide, par exemple `frequences = {}`.
    - Itérez sur votre liste de mots.
    - Pour chaque mot, mettez à jour le dictionnaire. Si le mot est déjà dans le dictionnaire, incrémentez sa valeur. Sinon, ajoutez-le avec une valeur de 1. (L'utilisation de `dict.get(mot, 0) + 1` est une manière élégante de le faire).

5.  **Triez les résultats**.

    - Une fois le dictionnaire de fréquences rempli, vous devez le trier par valeur (fréquence) dans l'ordre décroissant.
    - La méthode `sorted()` avec une fonction `lambda` comme clé est parfaite pour cela. `sorted(dictionnaire.items(), key=lambda item: item[1], reverse=True)`.
    - Cela retournera une liste de tuples `(mot, frequence)`.

6.  **Affichez les 10 mots les plus courants**.
    - Dans le bloc principal (`if __name__ == '__main__':`), appelez votre fonction.
    - Affichez les 10 premiers éléments de la liste triée.

## Résultat Attendu

L'exécution de `compteur_mots.py` doit produire une sortie similaire à celle-ci (l'ordre des mots ayant la même fréquence peut varier) :

```
Les 10 mots les plus fréquents sont :
- python: 5
- est: 4
- de: 3
- la: 3
- un: 2
- langage: 2
- à: 1
- programmation: 1
- puissant: 1
- simple: 1
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# compteur_mots.py

import string

def compter_mots(nom_fichier: str) -> list:
    """
    Lit un fichier, compte la fréquence de chaque mot,
    et retourne une liste de (mot, fréquence) triée par fréquence.
    """
    try:
        with open(nom_fichier, 'r', encoding='utf-8') as f:
            contenu = f.read()
    except FileNotFoundError:
        print(f"Erreur : Le fichier '{nom_fichier}' n'a pas été trouvé.")
        return []

    # 1. Mettre en minuscules
    contenu = contenu.lower()

    # 2. Supprimer la ponctuation
    # Une manière plus robuste que des replace() successifs
    # string.punctuation contient '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~'
    for char in string.punctuation:
        contenu = contenu.replace(char, '')

    # 3. Séparer en mots
    mots = contenu.split()

    # 4. Compter les fréquences
    frequences = {}
    for mot in mots:
        frequences[mot] = frequences.get(mot, 0) + 1

    # 5. Trier le dictionnaire par valeur (fréquence)
    mots_tries = sorted(frequences.items(), key=lambda item: item[1], reverse=True)

    return mots_tries

if __name__ == "__main__":
    nom_du_fichier = "texte_a_analyser.txt"
    mots_plus_frequents = compter_mots(nom_du_fichier)

    if mots_plus_frequents:
        print("Les 10 mots les plus fréquents sont :")
        # Affiche les 10 premiers mots
        for mot, freq in mots_plus_frequents[:10]:
            print(f"- {mot}: {freq}")

```

</details>
