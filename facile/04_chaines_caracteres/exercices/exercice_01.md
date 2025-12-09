# Exercice 01 : Nettoyage et Formatage de Données Textuelles

## Objectif

Cet exercice a pour but de vous faire pratiquer les méthodes de manipulation de chaînes de caractères les plus courantes pour "nettoyer" une donnée textuelle brute.

## Contexte

Imaginez que vous recevez des données d'un formulaire web. Les utilisateurs entrent souvent les données de manière incohérente (majuscules, espaces superflus, etc.). Votre tâche est de standardiser ces données.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `string_cleanup.py`.

2.  **Initialisez la variable suivante** au début de votre script. Elle représente une entrée utilisateur brute.

    ```python
    raw_data = "   osCaR wIldE   "
    ```

3.  **Effectuez les opérations de nettoyage suivantes**, en stockant le résultat de chaque étape dans une nouvelle variable :
    a. **`cleaned_data`** : Supprimez les espaces inutiles au début et à la fin de `raw_data`.
    b. **`formatted_data`** : Mettez la chaîne `cleaned_data` en casse "Titre" (la première lettre de chaque mot en majuscule et le reste en minuscule).

4.  **Créez un nom d'utilisateur** :
    a. **`username`** : À partir de `formatted_data`, créez un nom d'utilisateur en : - mettant toute la chaîne en minuscules, - remplaçant l'espace par un point (`.`).
    Le résultat attendu est `oscar.wilde`.

5.  **Affichez les résultats** :

    - Utilisez la fonction `print()` pour afficher la chaîne à chaque étape du processus, afin de bien voir les transformations.

    ```
    Donnée brute : '   osCaR wIldE   '
    Donnée nettoyée : 'Oscar Wilde'
    Nom d'utilisateur généré : 'oscar.wilde'
    ```

6.  **Vérification finale** :
    - Ajoutez une ligne qui vérifie si le `username` généré est bien égal à `"oscar.wilde"` et affichez le résultat (qui devrait être `True`).
    ```python
    print("Vérification du nom d'utilisateur :", username == "oscar.wilde")
    ```

## Résultat Attendu

L'exécution de votre script `string_cleanup.py` devrait produire la sortie suivante :

```
Donnée brute: '   osCaR wIldE   '
Donnée nettoyée: 'Oscar Wilde'
Nom d'utilisateur généré: 'oscar.wilde'
Vérification du nom d'utilisateur : True
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# string_cleanup.py

# Donnée initiale
raw_data = "   osCaR wIldE   "
print("Donnée brute:", f"'{raw_data}'") # On utilise des f-strings pour bien voir les espaces

# a. Nettoyage des espaces
cleaned_data = raw_data.strip()

# b. Formatage en casse "Titre"
formatted_data = cleaned_data.title()
print("Donnée nettoyée:", f"'{formatted_data}'")

# c. Création du nom d'utilisateur
# On peut enchaîner les méthodes !
username = formatted_data.lower().replace(" ", ".")
print("Nom d'utilisateur généré:", f"'{username}'")

# d. Vérification
print("Vérification du nom d'utilisateur :", username == "oscar.wilde")

```

</details>
