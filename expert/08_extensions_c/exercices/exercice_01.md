# Exercice 01 : Création et Compilation d'un Module C Simple

## Objectif

Cet exercice a pour but de vous guider à travers les étapes complètes de création, compilation et utilisation d'une extension C simple pour Python. Vous allez implémenter un module `systeminfo` qui expose une fonction `get_platform()` retournant la plateforme du système d'exploitation sur laquelle Python a été compilé.

## Contexte

L'API C de Python fournit des macros et des variables pour obtenir des informations sur le système. La macro `Py_GetPlatform()` retourne une chaîne de caractères (`const char*`) décrivant la plateforme (ex: "linux", "darwin" pour macOS, "win32" pour Windows).

Vous allez créer un module C qui rend cette information accessible directement depuis Python.

## Prérequis

- Un compilateur C (GCC ou Clang sur Linux/macOS, MSVC sur Windows).
- Les en-têtes de développement Python.
  - Sur Debian/Ubuntu : `sudo apt-get install python3-dev`
  - Sur Fedora/CentOS : `sudo dnf install python3-devel`
  - Sur macOS, ils sont généralement inclus avec les outils de ligne de commande Xcode.
  - Sur Windows, assurez-vous de cocher "Python development headers" lors de l'installation de Python.

## Énoncé

### Étape 1 : Créer la structure du projet

Créez un nouveau dossier pour votre projet et placez-y deux fichiers vides :

- `systeminfo_module.c`
- `setup.py`

### Étape 2 : Écrire le code de l'extension C

Ouvrez `systeminfo_module.c` et écrivez le code C nécessaire. Inspirez-vous de l'exemple du cours.

1.  **Inclure `Python.h`**.
2.  **Écrire la fonction C `systeminfo_get_platform`**.
    - Elle ne prend pas d'arguments, donc `PyObject* args` ne sera pas utilisé.
    - Utilisez `Py_GetPlatform()` pour obtenir la chaîne de caractères de la plateforme.
    - Utilisez `PyUnicode_FromString()` pour convertir cette chaîne C en un objet chaîne de caractères Python.
    - Retournez cet objet Python.
3.  **Créer la table des méthodes `SystemInfoMethods`**.
    - Déclarez une seule méthode :
      - Nom Python : `"get_platform"`
      - Fonction C : `systeminfo_get_platform`
      - Type d'appel : `METH_NOARGS` (car la fonction ne prend pas d'arguments).
      - Docstring : `"Retourne la plateforme du système (ex: 'linux', 'darwin')."`
4.  **Créer la structure de définition du module `systeminfo_definition`**.
    - Nom du module : `"systeminfo"`
    - Docstring du module : `"Un module d'exemple pour obtenir des informations système."`
5.  **Écrire la fonction d'initialisation `PyInit_systeminfo`**.

### Étape 3 : Écrire le script de compilation

Ouvrez `setup.py` et écrivez le script pour `distutils`.

1.  Importez `setup` et `Extension` depuis `distutils.core`.
2.  Créez un objet `Extension` :
    - Nom du module : `"systeminfo"`
    - Fichiers source : `['systeminfo_module.c']`
3.  Appelez `setup()` en lui passant les informations nécessaires, y compris votre objet `Extension` dans la liste `ext_modules`.

### Étape 4 : Compiler et Installer le module

Ouvrez un terminal dans le dossier de votre projet.

1.  **Compiler le module**.

    ```bash
    python setup.py build
    ```

    Vérifiez qu'un dossier `build` a été créé et qu'il contient votre bibliothèque partagée (`.so` ou `.pyd`).

2.  **Installer le module dans votre environnement de développement**.
    Cette commande crée un lien symbolique vers le module compilé, ce qui vous permet de le tester sans avoir à le réinstaller après chaque modification.
    ```bash
    python setup.py develop
    ```

### Étape 5 : Tester le module en Python

1.  **Lancez un interpréteur Python** depuis le même terminal.
2.  **Importez et utilisez votre module**.

    ```python
    import systeminfo

    # Testez la fonction
    platform = systeminfo.get_platform()
    print(f"La plateforme détectée par le module C est : '{platform}'")

    # Vérifiez la docstring
    print("\nDocstring du module:")
    print(systeminfo.__doc__)

    print("\nDocstring de la fonction:")
    print(systeminfo.get_platform.__doc__)
    ```

## Résultat Attendu

L'exécution du script de test Python doit afficher la plateforme de votre système, ainsi que les docstrings que vous avez définies dans votre code C.

```
La plateforme détectée par le module C est : 'darwin'

Docstring du module:
Un module d'exemple pour obtenir des informations système.

Docstring de la fonction:
Retourne la plateforme du système (ex: 'linux', 'darwin').
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

**Fichier `systeminfo_module.c` :**

```c
#define PY_SSIZE_T_CLEAN
#include <Python.h>

// 1. Implémentation de la fonction
static PyObject* systeminfo_get_platform(PyObject* self, PyObject* args) {
    // Pas besoin de PyArg_ParseTuple car on n'a pas d'arguments (METH_NOARGS)

    // Py_GetPlatform() retourne un const char*
    const char* platform_str = Py_GetPlatform();

    // Convertit la chaîne C en objet chaîne de caractères Python
    return PyUnicode_FromString(platform_str);
}

// 2. Table des méthodes
static PyMethodDef SystemInfoMethods[] = {
    {
        "get_platform", // Nom en Python
        systeminfo_get_platform, // Fonction C
        METH_NOARGS, // Pas d'arguments
        "Retourne la plateforme du système (ex: 'linux', 'darwin')." // Docstring
    },
    {NULL, NULL, 0, NULL} // Sentinelle
};

// 3. Définition du module
static struct PyModuleDef systeminfo_definition = {
    PyModuleDef_HEAD_INIT,
    "systeminfo", // Nom du module
    "Un module d'exemple pour obtenir des informations système.", // Docstring
    -1,
    SystemInfoMethods
};

// 4. Fonction d'initialisation
PyMODINIT_FUNC PyInit_systeminfo(void) {
    return PyModule_Create(&systeminfo_definition);
}
```

**Fichier `setup.py` :**

```python
from distutils.core import setup, Extension

systeminfo_extension = Extension(
    'systeminfo',
    sources=['systeminfo_module.c']
)

setup(
    name='SystemInfo',
    version='1.0',
    description='Module pour obtenir la plateforme système.',
    ext_modules=[systeminfo_extension]
)
```

</details>
