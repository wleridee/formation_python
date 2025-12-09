# Module : Écrire des Extensions C pour Python

## Objectif

Ce module a pour but de vous donner un aperçu de haut niveau sur la manière d'étendre l'interpréteur CPython avec du code écrit en C. Vous apprendrez pourquoi et quand créer une extension C, et vous verrez la structure de base d'un module d'extension simple.

**Note :** Ce sujet est vaste et complexe. L'objectif ici est de comprendre les concepts et le processus, pas de maîtriser l'API C de Python en une seule leçon.

## 1. Pourquoi Écrire des Extensions C ?

Il y a deux raisons principales pour lesquelles on étend Python avec du C (ou C++, Rust, etc.) :

1.  **Performance** : Pour des tâches très gourmandes en calcul (`CPU-bound`), le code C compilé est des ordres de grandeur plus rapide que le code Python interprété. En écrivant les goulots d'étranglement en C, on peut accélérer considérablement une application. C'est la raison d'être de bibliothèques comme NumPy, SciPy, et Pandas.

2.  **Intégration de bibliothèques existantes** : Pour utiliser une bibliothèque C existante depuis Python sans avoir à la ré-implémenter. On écrit une "couche d'adaptation" (wrapper) qui expose les fonctions de la bibliothèque C au monde Python.

## 2. L'API C de Python

CPython fournit une API C (`Python.h`) qui permet au code C d'interagir avec l'interpréteur Python. Cette API permet de :

- Créer et manipuler des objets Python (nombres, chaînes, listes, dictionnaires, etc.).
- Appeler des fonctions Python depuis le C.
- Définir de nouveaux types d'objets en C.
- Gérer le comptage de références et le garbage collector.
- Lever et attraper des exceptions Python.

Tous les objets en Python sont représentés en C par une structure `PyObject *`. C'est un pointeur vers une structure générique qui contient le compteur de références et un pointeur vers une structure de type spécifique.

## 3. Structure d'un Module d'Extension Simple

Créons un module C simple nommé `monmodule` qui expose une seule fonction, `monmodule.addition(a, b)`.

Le processus implique trois parties principales :

1.  Écrire la fonction C qui sera appelée.
2.  Définir la table des méthodes du module.
3.  Écrire la fonction d'initialisation du module.

### Étape 1 : Le Fichier Source C (`monmodule.c`)

```c
#define PY_SSIZE_T_CLEAN
#include <Python.h> // Doit être inclus en premier

// 1. La fonction C qui implémente la logique
// Le nom est statique pour qu'il ne soit pas visible en dehors de ce fichier.
static PyObject* monmodule_addition(PyObject* self, PyObject* args) {
    long a, b;
    long resultat;

    // PyArg_ParseTuple convertit les arguments Python (un tuple) en valeurs C.
    // "ll" signifie qu'on attend deux entiers longs (long).
    if (!PyArg_ParseTuple(args, "ll", &a, &b)) {
        return NULL; // Le parsing a échoué, une exception a été levée.
    }

    // La logique métier (ici, une simple addition)
    resultat = a + b;

    // PyLong_FromLong convertit le résultat C en un objet entier Python.
    return PyLong_FromLong(resultat);
}

// 2. La table des méthodes du module
// C'est un tableau qui liste les fonctions exportées par le module.
static PyMethodDef MonModuleMethods[] = {
    // { "nom_en_python", fonction_c, convention_d_appel, "docstring" }
    {"addition", monmodule_addition, METH_VARARGS, "Additionne deux entiers."},
    {NULL, NULL, 0, NULL} // Sentinelle de fin de tableau
};

// 3. La structure de définition du module
static struct PyModuleDef monmodule_definition = {
    PyModuleDef_HEAD_INIT,
    "monmodule", // Nom du module
    "Un module d'exemple qui additionne des entiers.", // Docstring du module
    -1,
    MonModuleMethods
};

// 4. La fonction d'initialisation du module
// Cette fonction est appelée lorsque Python fait 'import monmodule'.
// Le nom DOIT être PyInit_<nom_du_module>.
PyMODINIT_FUNC PyInit_monmodule(void) {
    return PyModule_Create(&monmodule_definition);
}
```

**Analyse du code :**

- `monmodule_addition` : C'est le "wrapper". Il prend deux `PyObject*` en argument. `self` est l'objet module, et `args` est un tuple contenant les arguments passés depuis Python.
- `PyArg_ParseTuple` : C'est la fonction standard pour extraire des valeurs C à partir du tuple d'arguments Python.
- `PyLong_FromLong` : C'est la fonction pour créer un objet entier Python à partir d'un `long` C. Il existe des fonctions similaires pour tous les types (`PyFloat_FromDouble`, `PyUnicode_FromString`, etc.).
- `MonModuleMethods` : Ce tableau fait le lien entre le nom de la fonction en Python (`"addition"`) et le pointeur vers la fonction C qui l'implémente (`monmodule_addition`).
- `PyInit_monmodule` : C'est le point d'entrée du module. Python l'appelle lors de l'import.

## 4. Compiler l'Extension

Compiler une extension C pour Python est la partie la plus délicate. La méthode standard est d'utiliser le module `distutils` ou `setuptools` de Python.

On crée un fichier `setup.py` :

```python
# setup.py
from distutils.core import setup, Extension

# Décrit notre module d'extension
mon_module_extension = Extension(
    'monmodule', # Nom du module tel qu'importé en Python
    sources=['monmodule.c'] # Liste des fichiers source C
)

# Fonction setup
setup(
    name='MonModule',
    version='1.0',
    description='Un module d\'exemple',
    ext_modules=[mon_module_extension] # Liste des extensions à compiler
)
```

Ensuite, on compile depuis la ligne de commande :

```bash
python setup.py build
```

Cette commande va :

1.  Trouver le compilateur C approprié sur votre système (GCC, Clang, MSVC...).
2.  Compiler `monmodule.c` en un fichier objet.
3.  Lier le fichier objet avec la bibliothèque Python pour créer une **bibliothèque partagée**.
    - Sur Linux : `monmodule.so`
    - Sur macOS : `monmodule.so`
    - Sur Windows : `monmodule.pyd`

Le fichier compilé se trouvera dans un sous-dossier `build/lib...`.

Pour une installation propre, on utilise :

```bash
python setup.py install
# ou pour le développement :
python setup.py develop
# ou pour créer une wheel (paquet distribuable) :
pip wheel .
```

## 5. Utiliser l'Extension en Python

Une fois le module compilé et installé dans l'environnement, on peut l'utiliser comme n'importe quel autre module Python :

```python
import monmodule

# Afficher la docstring du module
print(monmodule.__doc__)
# Un module d'exemple qui additionne des entiers.

# Afficher la docstring de la fonction
print(monmodule.addition.__doc__)
# Additionne deux entiers.

# Appeler la fonction C
resultat = monmodule.addition(10, 25)
print(f"Le résultat est : {resultat}") # Le résultat est : 35

# Essayer avec des types incorrects
try:
    monmodule.addition("a", "b")
except TypeError as e:
    print(f"Erreur attrapée : {e}")
# PyArg_ParseTuple a levé une TypeError automatiquement.
```

## 6. Libérer le GIL

Pour les tâches `CPU-bound`, le vrai gain de performance vient de la capacité du code C à libérer le GIL, permettant à d'autres threads Python de s'exécuter.

On utilise une paire de macros pour cela :

```c
static PyObject* ma_fonction_cpu_intensive(PyObject* self, PyObject* args) {
    // ... parser les arguments ...

    Py_BEGIN_ALLOW_THREADS
    // --- Début de la section sans GIL ---

    // Ici, on ne peut PAS utiliser l'API C de Python.
    // On travaille uniquement avec des variables et des structures de données C.
    // C'est ici qu'on place le calcul lourd.

    // --- Fin de la section sans GIL ---
    Py_END_ALLOW_THREADS

    // ... créer l'objet Python de retour ...
    return resultat_python;
}
```

Pendant que le code entre `Py_BEGIN_ALLOW_THREADS` et `Py_END_ALLOW_THREADS` s'exécute, le GIL est libéré. Si ce code est dans un thread, la PVM peut scheduler un autre thread Python pour qu'il s'exécute en parallèle.

## 7. Alternatives Modernes

Écrire des extensions C à la main est puissant mais verbeux et sujet aux erreurs (notamment la gestion manuelle des références). Des outils modernes simplifient grandement ce processus :

- **Cython** : Un sur-ensemble de Python qui permet d'ajouter des types C statiques au code Python. Cython "compile" ce code en un module C optimisé. C'est l'outil le plus populaire pour accélérer du code Python.
- **pybind11** : Une bibliothèque C++ "header-only" qui simplifie grandement la création de wrappers pour du code C++.
- **CFFI (C Foreign Function Interface)** : Permet d'appeler du code C directement depuis Python, sans écrire de code C "wrapper". Idéal pour intégrer des bibliothèques C existantes.
- **Rust avec PyO3 ou Maturin** : Rust est un langage moderne et sûr qui devient une alternative très populaire au C/C++ pour écrire des extensions Python performantes.

## Conclusion

Écrire des extensions C est la méthode ultime pour repousser les limites de performance de Python et pour s'interfacer avec l'écosystème C existant. Bien que le processus manuel soit complexe, il offre un contrôle total sur le comportement du code. Comprendre les bases de l'API C de Python vous donne une vision plus profonde du fonctionnement de l'interpréteur et vous permet d'apprécier la "magie" derrière les grandes bibliothèques scientifiques comme NumPy. Pour des besoins pratiques, des outils comme Cython ou pybind11 sont souvent des choix plus productifs.
