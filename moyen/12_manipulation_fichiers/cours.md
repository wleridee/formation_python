# Module : Manipulation de Fichiers

## Objectif

Ce module a pour but de vous apprendre à lire et écrire dans des fichiers en Python. Vous découvrirez la bonne manière d'ouvrir et de fermer des fichiers en utilisant le gestionnaire de contexte `with`, et comment interagir avec des fichiers texte et des fichiers structurés comme le CSV.

## 1. Lire un Fichier Texte

La manière la plus courante et la plus sûre de travailler avec des fichiers est d'utiliser l'instruction `with`. Elle garantit que le fichier sera automatiquement fermé à la fin du bloc, même si des erreurs se produisent.

```python
# 'r' est le mode : 'r' pour read (lecture), 'w' pour write (écriture), 'a' pour append (ajout).
# 't' pour text mode (par défaut), 'b' pour binary mode.
# Donc 'r' est équivalent à 'rt'.

try:
    with open('mon_fichier.txt', 'r', encoding='utf-8') as f:
        # 'f' est l'objet fichier

        # Lire le fichier entier d'un coup
        contenu = f.read()
        print("--- Contenu complet ---")
        print(contenu)

    with open('mon_fichier.txt', 'r', encoding='utf-8') as f:
        # Lire le fichier ligne par ligne (plus efficace pour les gros fichiers)
        print("\n--- Lecture ligne par ligne ---")
        for ligne in f:
            # .strip() enlève les espaces et sauts de ligne en début/fin de chaîne
            print(ligne.strip())

    with open('mon_fichier.txt', 'r', encoding='utf-8') as f:
        # Lire toutes les lignes dans une liste
        lignes = f.readlines()
        print("\n--- Lignes dans une liste ---")
        print(lignes)

except FileNotFoundError:
    print("Erreur : Le fichier n'a pas été trouvé.")

```

**Note sur `encoding='utf-8'` :** Il est crucial de toujours spécifier l'encodage. `utf-8` est le standard le plus universel aujourd'hui et gère la plupart des caractères (accents, symboles, etc.). Omettre cet argument peut causer des erreurs sur différents systèmes d'exploitation qui n'ont pas le même encodage par défaut.

## 2. Écrire dans un Fichier Texte

### a. Mode Écriture (`'w'`)

Le mode `'w'` (write) ouvre un fichier pour l'écriture. **Attention :** Si le fichier existe déjà, son contenu sera **écrasé**. S'il n'existe pas, il sera créé.

```python
lignes_a_ecrire = [
    "Première ligne.\n",
    "Deuxième ligne.\n",
    "Troisième ligne.\n"
]

with open('nouveau_fichier.txt', 'w', encoding='utf-8') as f:
    # Écrire une seule chaîne
    f.write("Hello, World!\n")

    # Écrire une liste de chaînes
    f.writelines(lignes_a_ecrire)

# Vérifions le contenu
with open('nouveau_fichier.txt', 'r', encoding='utf-8') as f:
    print(f.read())
```

**Résultat dans `nouveau_fichier.txt` :**

```
Hello, World!
Première ligne.
Deuxième ligne.
Troisième ligne.
```

### b. Mode Ajout (`'a'`)

Le mode `'a'` (append) ouvre un fichier pour ajouter du contenu à la fin. Si le fichier n'existe pas, il est créé. Le contenu existant n'est pas effacé.

```python
with open('nouveau_fichier.txt', 'a', encoding='utf-8') as f:
    f.write("Ceci est une ligne ajoutée.\n")

# Vérifions à nouveau le contenu
with open('nouveau_fichier.txt', 'r', encoding='utf-8') as f:
    print(f.read())
```

**Nouveau résultat dans `nouveau_fichier.txt` :**

```
Hello, World!
Première ligne.
Deuxième ligne.
Troisième ligne.
Ceci est une ligne ajoutée.
```

## 3. Travailler avec les Chemins de Fichiers (`pathlib`)

Manipuler les chemins de fichiers avec de simples chaînes de caractères peut être source d'erreurs, notamment entre Windows (qui utilise `\`) et Linux/macOS (qui utilisent `/`).

Le module `pathlib`, introduit en Python 3.4, offre une approche orientée objet pour gérer les chemins de fichiers de manière portable.

```python
from pathlib import Path

# Créer un objet Path
# Le / fonctionne sur tous les systèmes, pathlib le convertira correctement.
chemin = Path('dossier') / 'sous_dossier' / 'mon_fichier.txt'

print(f"Chemin complet : {chemin}")
print(f"Nom du fichier : {chemin.name}")
print(f"Extension : {chemin.suffix}")
print(f"Dossier parent : {chemin.parent}")

# Créer les dossiers parents s'ils n'existent pas
chemin.parent.mkdir(parents=True, exist_ok=True)

# Utiliser le chemin directement avec open()
with open(chemin, 'w', encoding='utf-8') as f:
    f.write("Fichier créé avec pathlib.")

# Vérifier si un fichier existe
if chemin.exists():
    print(f"Le fichier '{chemin}' existe.")
```

## 4. Travailler avec des Fichiers CSV

CSV (Comma-Separated Values) est un format texte très courant pour stocker des données tabulaires. Le module `csv` de la bibliothèque standard facilite grandement sa manipulation.

### a. Lire un fichier CSV

Imaginons un fichier `personnes.csv` :

```csv
nom,age,ville
Alice,30,Paris
Bob,25,Lyon
Charlie,35,Marseille
```

**Code Python pour le lire :**

```python
import csv

with open('personnes.csv', 'r', encoding='utf-8', newline='') as f:
    # DictReader lit chaque ligne comme un dictionnaire
    lecteur_csv = csv.DictReader(f)

    for ligne in lecteur_csv:
        # ligne est un dictionnaire ordonné (OrderedDict)
        print(f"{ligne['nom']} a {ligne['age']} ans et vit à {ligne['ville']}.")
```

**`newline=''`** est important pour éviter des problèmes de sauts de ligne sur différents systèmes.

### b. Écrire un fichier CSV

```python
import csv

# Données à écrire (liste de dictionnaires)
donnees = [
    {'nom': 'David', 'age': 40, 'ville': 'Nantes'},
    {'nom': 'Eve', 'age': 28, 'ville': 'Bordeaux'}
]

# Noms des colonnes (doivent correspondre aux clés des dictionnaires)
noms_colonnes = ['nom', 'age', 'ville']

with open('nouvelles_personnes.csv', 'w', encoding='utf-8', newline='') as f:
    # DictWriter écrit des dictionnaires dans un fichier CSV
    ecrivain_csv = csv.DictWriter(f, fieldnames=noms_colonnes)

    # Écrit la ligne d'en-tête
    ecrivain_csv.writeheader()

    # Écrit les lignes de données
    ecrivain_csv.writerows(donnees)

print("Fichier 'nouvelles_personnes.csv' créé.")
```

## Conclusion

La manipulation de fichiers est une compétence essentielle en programmation. Python la rend particulièrement simple et sûre grâce au gestionnaire de contexte `with`. En utilisant `pathlib` pour gérer les chemins et le module `csv` pour les données tabulaires, vous disposez d'outils robustes pour interagir avec le système de fichiers de manière portable et efficace.
