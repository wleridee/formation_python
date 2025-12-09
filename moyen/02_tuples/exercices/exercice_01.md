# Exercice 01 : Gestion des Coordonnées Géographiques

## Objectif

Cet exercice a pour but de vous faire utiliser des tuples pour stocker des données qui ne doivent pas changer (des coordonnées géographiques) et de pratiquer le "tuple unpacking".

## Contexte

Vous travaillez sur une application de cartographie. Vous devez stocker les coordonnées (latitude, longitude) de plusieurs lieux célèbres. Comme ces coordonnées sont fixes, un tuple est la structure de données idéale pour garantir qu'elles ne seront pas modifiées accidentellement.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `geo_coordinates.py`.

2.  **Créez des tuples** pour représenter les coordonnées des lieux suivants :

    - `eiffel_tower` : (48.8584, 2.2945)
    - `statue_of_liberty` : (40.6892, -74.0445)
    - `taj_mahal` : (27.1751, 78.0421)

3.  **Stockez ces tuples dans une liste** nommée `locations`.

4.  **Écrivez une boucle `for`** pour itérer sur votre liste `locations`.

    - À l'intérieur de la boucle, utilisez le **tuple unpacking** pour assigner la latitude et la longitude à deux variables distinctes, `latitude` et `longitude`.
    - Pour chaque lieu, affichez ses coordonnées de manière formatée.

5.  **Tentez de modifier un tuple (pour voir l'erreur)** :
    - Après votre boucle, essayez de modifier la latitude de la Tour Eiffel :
      ```python
      # eiffel_tower[0] = 49.0
      ```
    - Mettez cette ligne en commentaire après avoir observé l'erreur `TypeError`. Ajoutez un commentaire expliquant pourquoi cela ne fonctionne pas.

## Résultat Attendu

La sortie de votre script (sans la ligne qui cause l'erreur) devrait être :

```
Location Coordinates:
 - Latitude: 48.8584, Longitude: 2.2945
 - Latitude: 40.6892, Longitude: -74.0445
 - Latitude: 27.1751, Longitude: 78.0421

---
Demonstrating tuple immutability:
Trying to change a tuple value will raise a TypeError.
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# geo_coordinates.py

# 1. Création des tuples
eiffel_tower = (48.8584, 2.2945)
statue_of_liberty = (40.6892, -74.0445)
taj_mahal = (27.1751, 78.0421)

# 2. Stockage dans une liste
locations = [eiffel_tower, statue_of_liberty, taj_mahal]

# 3. Boucle et unpacking
print("Location Coordinates:")
for location_tuple in locations:
    # Ici, on déballe le tuple de la localisation actuelle
    latitude, longitude = location_tuple
    print(f" - Latitude: {latitude}, Longitude: {longitude}")

print("\n---")
print("Demonstrating tuple immutability:")
# 4. Tentative de modification
# La ligne suivante est commentée car elle produit une erreur.
# eiffel_tower[0] = 49.0
# Décommenter la ligne ci-dessus lèvera une TypeError car les tuples sont immuables.
print("Trying to change a tuple value will raise a TypeError.")

```

</details>
