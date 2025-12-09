# Exercice 01 : Nettoyer un Fichier avec Black et Flake8

## Objectif

Cet exercice a pour but de vous faire utiliser `black` et `flake8` dans un workflow réaliste pour nettoyer un fichier Python qui contient à la fois des erreurs de style et des erreurs de logique.

## Contexte

Vous recevez un petit script d'un collègue. Le script fonctionne, mais il est difficile à lire, ne respecte pas le PEP 8, et contient du code inutile. Votre tâche est d'utiliser les outils de qualité de code pour le rendre propre et professionnel.

## Énoncé

### Étape 1 : Créer le fichier à nettoyer

Créez un fichier nommé `script_a_nettoyer.py` et copiez-y le code suivant :

```python
import sys
import math

def  CalculerAireCercle(rayon):
    if rayon<0:
        print( "Le rayon ne peut pas être négatif", file=sys.stderr)
        return None
    aire =math.pi * rayon**2
    return aire

def Saluer(nom, age):
    message = "Bonjour " + nom + ", vous avez " + str(age) + " ans."
    print(message)

# Test de la fonction
rayon_test=5
aire_calculee = CalculerAireCercle(rayon_test)

if aire_calculee is not None:
    print("L'aire d'un cercle de rayon {} est {:.2f}".format(rayon_test, aire_calculee))

# La fonction Saluer n'est jamais utilisée
```

### Étape 2 : Installer les outils

Si ce n'est pas déjà fait, installez `flake8` et `black` :

```bash
pip install flake8 black
```

### Étape 3 : Utiliser `flake8` pour identifier les problèmes

1.  Lancez `flake8` sur le fichier :
    ```bash
    flake8 script_a_nettoyer.py
    ```
2.  Observez la liste des erreurs. Vous devriez voir des erreurs concernant :
    - L'import de `sys` non utilisé (`F401`).
    - Des noms de fonctions qui ne respectent pas la convention `snake_case` (`N802`). (Note : cela peut nécessiter un plugin comme `pep8-naming`).
    - Des espaces manquants ou en trop.
    - Des lignes trop longues (si vous en aviez).
    - Une fonction `Saluer` définie mais non utilisée (`F841` si elle était locale, mais ici `flake8` pourrait ne pas le signaler pour une fonction globale).

### Étape 4 : Utiliser `black` pour formater le code

1.  Lancez `black` pour qu'il corrige automatiquement tous les problèmes de formatage :
    ```bash
    black script_a_nettoyer.py
    ```
2.  Ouvrez le fichier `script_a_nettoyer.py` et observez les changements. `black` aura corrigé :
    - Les espaces.
    - L'indentation.
    - Les sauts de ligne.
    - L'utilisation des guillemets.

### Étape 5 : Corriger manuellement les erreurs restantes

1.  Relancez `flake8` sur le fichier formaté par `black` :
    ```bash
    flake8 script_a_nettoyer.py
    ```
2.  La liste d'erreurs devrait être beaucoup plus courte. `black` ne corrige pas les erreurs de logique. Vous devriez encore voir des erreurs comme :

    - L'import de `sys` non utilisé.
    - La fonction `Saluer` non utilisée.
    - (Optionnel) Le nom de la fonction `CalculerAireCercle` qui n'est pas en `snake_case`.

3.  **Corrigez manuellement ces problèmes** :
    - Supprimez `import sys` car il n'est plus nécessaire après avoir enlevé l'affichage sur `stderr`.
    - Supprimez la fonction `Saluer` qui n'est jamais utilisée.
    - Renommez `CalculerAireCercle` en `calculer_aire_cercle` et mettez à jour son appel.
    - Profitez-en pour moderniser le formatage de la chaîne avec une f-string.

### Étape 6 : Vérification finale

1.  Relancez `flake8` une dernière fois :

    ```bash
    flake8 script_a_nettoyer.py
    ```

    La commande ne doit maintenant produire **aucune sortie**, indiquant que le fichier est propre.

2.  Relancez `black --check` pour confirmer que le formatage est correct :
    ```bash
    black --check script_a_nettoyer.py
    ```
    La commande doit afficher `1 file left unchanged`.

## Résultat Attendu

À la fin de l'exercice, votre fichier `script_a_nettoyer.py` doit ressembler à ceci :

```python
import math


def calculer_aire_cercle(rayon):
    """Calcule l'aire d'un cercle."""
    if rayon < 0:
        print("Le rayon ne peut pas être négatif.")
        return None
    aire = math.pi * rayon**2
    return aire


# Test de la fonction
rayon_test = 5
aire_calculee = calculer_aire_cercle(rayon_test)

if aire_calculee is not None:
    print(f"L'aire d'un cercle de rayon {rayon_test} est {aire_calculee:.2f}")

```

Ce fichier final est non seulement fonctionnel, mais aussi propre, lisible, et conforme aux standards de la communauté Python.

<details>
<summary>Cliquez ici pour voir le déroulement étape par étape</summary>

1.  **Fichier initial :** `script_a_nettoyer.py` (tel que donné dans l'énoncé).

2.  **`flake8 script_a_nettoyer.py` (sortie partielle) :**

    ```
    script_a_nettoyer.py:1:1: F401 'sys' imported but unused
    script_a_nettoyer.py:4:1: E302 expected 2 blank lines, found 1
    script_a_nettoyer.py:4:6: E211 whitespace before '('
    ...
    ```

3.  **`black script_a_nettoyer.py`**
    Le fichier devient :

    ```python
    import sys
    import math


    def CalculerAireCercle(rayon):
        if rayon < 0:
            print("Le rayon ne peut pas être négatif", file=sys.stderr)
            return None
        aire = math.pi * rayon**2
        return aire


    def Saluer(nom, age):
        message = "Bonjour " + nom + ", vous avez " + str(age) + " ans."
        print(message)


    # Test de la fonction
    rayon_test = 5
    aire_calculee = CalculerAireCercle(rayon_test)

    if aire_calculee is not None:
        print("L'aire d'un cercle de rayon {} est {:.2f}".format(rayon_test, aire_calculee))

    # La fonction Saluer n'est jamais utilisée
    ```

4.  **`flake8 script_a_nettoyer.py` (après `black`)**

    ```
    script_a_nettoyer.py:1:1: F401 'sys' imported but unused
    script_a_nettoyer.py:14:1: F841 local variable 'Saluer' is assigned to but never used
    ... (peut-être d'autres erreurs de nommage si plugins installés)
    ```

5.  **Corrections manuelles :**

    - On supprime `import sys` et `file=sys.stderr`.
    - On supprime la fonction `Saluer`.
    - On renomme `CalculerAireCercle` en `calculer_aire_cercle`.
    - On change le `.format` en f-string.

6.  **Fichier final :** (voir ci-dessus dans "Résultat Attendu").
    - `flake8 script_a_nettoyer.py` -> Aucune sortie.
    - `black --check script_a_nettoyer.py` -> `1 file left unchanged`.

</details>
