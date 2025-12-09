# Exercice 01 : Mise en Place de votre Premier Projet

## Objectif

Cet exercice vous guidera à travers les étapes essentielles pour créer un projet Python propre et isolé en utilisant un environnement virtuel.

## Énoncé

### Partie 1 : Création du Projet et de l'Environnement

1.  **Créez un dossier de projet** :

    - Ouvrez votre terminal.
    - Créez un nouveau dossier nommé `learning_python`.
    - Naviguez à l'intérieur de ce dossier avec la commande `cd learning_python`.

2.  **Créez un environnement virtuel** :

    - Utilisez le module `venv` de Python pour créer un environnement virtuel nommé `venv`.
    - **Commande :** `python3 -m venv venv`

3.  **Vérifiez la création** :
    - Listez le contenu du dossier `learning_python`. Vous devriez voir un nouveau sous-dossier nommé `venv`.

### Partie 2 : Activation et Utilisation de l'Environnement

1.  **Activez l'environnement virtuel** :

    - Utilisez la commande appropriée pour votre système d'exploitation :
      - **macOS/Linux :** `source venv/bin/activate`
      - **Windows (PowerShell) :** `venv\Scripts\Activate.ps1` (vous devrez peut-être autoriser l'exécution des scripts avec `Set-ExecutionPolicy Unrestricted -Scope Process`).
      - **Windows (cmd.exe) :** `venv\Scripts\activate.bat`
    - Votre prompt de terminal doit maintenant commencer par `(venv)`.

2.  **Installez une bibliothèque** :

    - Nous allons installer une bibliothèque simple et amusante appelée `cowsay`.
    - **Commande :** `pip install cowsay`

3.  **Vérifiez les paquets installés** :
    - Pour voir ce qui est installé dans votre environnement, utilisez la commande `pip list` ou `pip freeze`. Vous devriez voir `cowsay` dans la liste.

### Partie 3 : Écriture et Exécution d'un Script

1.  **Ouvrez le dossier dans VS Code** :

    - Avec votre terminal toujours dans le dossier `learning_python`, tapez `code .` pour ouvrir le projet dans VS Code.

2.  **Sélectionnez l'interpréteur Python** :

    - VS Code devrait détecter automatiquement l'environnement virtuel. Si une fenêtre pop-up apparaît vous demandant si vous voulez utiliser l'interpréteur de `venv`, cliquez sur "Oui".
    - Sinon, ouvrez la Palette de Commandes (`Ctrl+Shift+P` ou `Cmd+Shift+P`), tapez "Python: Select Interpreter" et choisissez l'interpréteur qui a `('venv': venv)` dans son nom.

3.  **Créez un fichier Python** :

    - Créez un nouveau fichier nommé `app.py`.
    - Copiez-y le code suivant :

    ```python
    # app.py
    import cowsay

    cowsay.cow("Hello, I'm learning Python!")
    ```

4.  **Exécutez le script** :

    - Ouvrez le terminal intégré de VS Code (`Ctrl+` ou `Cmd+`). Il devrait automatiquement être dans l'environnement virtuel activé.
    - Exécutez votre script avec la commande :

    ```bash
    python app.py
    ```

5.  **Désactivez l'environnement** :
    - Une fois que vous avez vu la vache parler, tapez `deactivate` dans le terminal pour quitter l'environnement virtuel.

## Résultat Attendu

L'exécution de `python app.py` doit afficher une vache ASCII dans votre terminal avec le message "Hello, I'm learning Python!".

<details>
<summary>Exemple de sortie</summary>

```
 ___________________________
< Hello, I'm learning Python! >
 ---------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

</details>
