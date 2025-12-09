# Module : Installation et Environnement

## 1. Quoi : L'environnement de développement

Un environnement de développement est l'ensemble des outils que vous utilisez pour écrire, tester et exécuter votre code. Pour Python, les composants essentiels sont :

- **L'interpréteur Python** : Le programme qui exécute votre code.
- **Un éditeur de code ou un IDE** : Un logiciel pour écrire votre code.
- **Un terminal** : Pour lancer des commandes.
- **Un gestionnaire de paquets (`pip`)** : Pour installer des bibliothèques externes.
- **Un gestionnaire d'environnement virtuel (`venv`)** : Pour isoler les dépendances de vos projets.

## 2. Pourquoi : L'importance d'un bon environnement

- **Reproductibilité** : Un environnement bien configuré garantit que votre code fonctionnera de la même manière sur votre machine, sur celle de vos collègues et sur le serveur de production.
- **Isolation** : Les environnements virtuels empêchent les conflits de dépendances. Imaginez que le Projet A nécessite la version 1.0 d'une bibliothèque, et le Projet B la version 2.0. Sans isolation, c'est impossible à gérer.
- **Efficacité** : Un bon éditeur de code avec des fonctionnalités comme l'autocomplétion, le débogage et l'intégration de Git vous rendra beaucoup plus productif.

## 3. Comment : Mettre en place votre environnement

### A. Installation de Python

- **Windows** :
  1.  Allez sur le [site officiel de Python](https://www.python.org/downloads/windows/).
  2.  Téléchargez le dernier installateur stable.
  3.  Exécutez l'installateur. **Très important : Cochez la case "Add Python to PATH"** avant de cliquer sur "Install Now".
- **macOS** :
  - Python est souvent pré-installé, mais il est recommandé d'installer une version plus récente.
  - La méthode la plus simple est d'utiliser [Homebrew](https://brew.sh/). Une fois Homebrew installé, ouvrez un terminal et tapez `brew install python3`.
- **Linux** :
  - Python est presque toujours pré-installé. Pour installer la dernière version, utilisez le gestionnaire de paquets de votre distribution (ex: `sudo apt update && sudo apt install python3` sur Debian/Ubuntu).

Pour vérifier l'installation, ouvrez un terminal et tapez `python3 --version` (ou `python --version` sur Windows).

### B. Choix d'un Éditeur de Code

- **VS Code (Visual Studio Code)** :
  - ✅ **Bon** : Gratuit, léger, extrêmement populaire et extensible. C'est le choix recommandé pour cette formation.
  - Installez-le depuis le [site officiel](https://code.visualstudio.com/).
  - Installez l'extension Python de Microsoft depuis la marketplace de VS Code pour activer des fonctionnalités comme l'autocomplétion, le linting et le débogage.
- **PyCharm** :
  - ✅ **Bon** : Un IDE (Environnement de Développement Intégré) très puissant, spécialisé pour Python.
  - La version "Community" est gratuite, tandis que la version "Professional" est payante et offre plus de fonctionnalités (notamment pour le développement web).

### C. Utilisation de l'Environnement Virtuel (`venv`)

C'est une étape cruciale pour tout projet Python.

1.  **Créez un dossier pour votre projet** :

    ```bash
    mkdir my_python_project
    cd my_python_project
    ```

2.  **Créez l'environnement virtuel** :

    - Le module `venv` est inclus avec Python. La commande crée un dossier (ici, `venv`) qui contiendra une copie de l'interpréteur Python et un espace pour installer des bibliothèques.

    ```bash
    python3 -m venv venv
    ```

3.  **Activez l'environnement virtuel** :

    - Vous devez "activer" l'environnement pour que votre terminal l'utilise.
    - **Windows (cmd.exe)** : `venv\Scripts\activate.bat`
    - **Windows (PowerShell)** : `venv\Scripts\Activate.ps1`
    - **macOS/Linux** : `source venv/bin/activate`
    - Une fois activé, votre prompt de terminal devrait afficher `(venv)` au début.

4.  **Installez des paquets** :

    - `pip` est le gestionnaire de paquets de Python.
    - Les paquets installés ne le seront que dans l'environnement virtuel actif.

    ```bash
    pip install requests
    ```

5.  **Désactivez l'environnement** :
    - Quand vous avez fini de travailler, vous pouvez désactiver l'environnement pour revenir à votre shell normal.
    ```bash
    deactivate
    ```

---

**Checklist de Démarrage d'un Projet :**

1.  [ ] Créer le dossier du projet.
2.  [ ] `cd` dans le dossier.
3.  [ ] `python3 -m venv venv`
4.  [ ] `source venv/bin/activate` (ou équivalent Windows)
5.  [ ] Commencer à coder !
