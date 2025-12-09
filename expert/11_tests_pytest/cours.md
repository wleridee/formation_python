# Module : Les Tests avec Pytest

## Objectif

Ce module a pour but de vous initier à `pytest`, le framework de test le plus populaire et le plus puissant de l'écosystème Python. Vous apprendrez à écrire des tests simples, à utiliser les _fixtures_ pour préparer le contexte de vos tests, et à comprendre la philosophie de `pytest` qui privilégie la simplicité et la lisibilité.

## 1. Pourquoi Écrire des Tests ?

Les tests automatisés sont une partie cruciale du développement logiciel moderne. Ils permettent de :

- **Vérifier la correction** : S'assurer que votre code fait ce qu'il est censé faire.
- **Prévenir les régressions** : Garantir que les nouvelles modifications n'ont pas cassé des fonctionnalités existantes.
- **Faciliter le refactoring** : Vous pouvez modifier votre code en toute confiance, sachant que la suite de tests vous alertera si quelque chose ne va plus.
- **Servir de documentation** : Les tests montrent comment votre code est censé être utilisé.

## 2. `pytest` vs `unittest`

`unittest` est le framework de test inclus dans la bibliothèque standard de Python. `pytest` est un package tiers qui s'est imposé comme le standard de facto grâce à ses nombreux avantages :

- **Syntaxe plus simple** : Pas besoin de créer des classes qui héritent de `unittest.TestCase`. De simples fonctions suffisent.
- **Assertions plus claires** : On utilise l'instruction `assert` standard de Python. `pytest` se charge de fournir des messages d'erreur détaillés en cas d'échec.
- **Découverte de tests puissante** : `pytest` trouve et exécute automatiquement vos tests si vous suivez des conventions de nommage simples.
- **Fixtures** : Un système d'injection de dépendances élégant et puissant pour gérer l'état et le contexte des tests.
- **Écosystème de plugins riche** : Des centaines de plugins pour étendre les fonctionnalités de `pytest` (tests de couverture, tests asynchrones, intégration avec Django/Flask, etc.).

## 3. Écrire son Premier Test avec `pytest`

1.  **Installation :**

    ```bash
    pip install pytest
    ```

2.  **Conventions de nommage :**

    - Les fichiers de test doivent être nommés `test_*.py` ou `*_test.py`.
    - Les fonctions de test à l'intérieur de ces fichiers doivent être nommées `test_*()`.
    - Les classes de test (optionnelles) doivent être nommées `Test*`.

3.  **Exemple :**
    Créez un fichier `calculs.py` avec une fonction à tester :

    ```python
    # calculs.py
    def addition(a, b):
        return a + b
    ```

    Créez un fichier de test `test_calculs.py` dans le même dossier :

    ```python
    # test_calculs.py
    from calculs import addition

    # Une simple fonction de test
    def test_addition():
        # L'assertion est simple et directe
        assert addition(2, 3) == 5
        assert addition(-1, 1) == 0
        assert addition(-1, -1) == -2
    ```

4.  **Lancer les tests :**
    Ouvrez un terminal dans le dossier et lancez simplement la commande `pytest` :

    ```bash
    pytest
    ```

5.  **Résultat :**

    ```
    ============================= test session starts ==============================
    ...
    collected 1 item

    test_calculs.py .                                                        [100%]

    ============================== 1 passed in ...s ===============================
    ```

    Le `.` indique qu'un test a réussi. Si un test échoue, vous verrez un `F`.

### Exemple d'un test qui échoue

Modifions le test pour qu'il échoue :

```python
# test_calculs.py
def test_addition_echec():
    assert addition(2, 3) == 6 # Assertion incorrecte
```

`pytest` nous donne un rapport d'erreur très détaillé :

```
...
    def test_addition_echec():
>       assert addition(2, 3) == 6
E       assert 5 == 6
E        +  where 5 = addition(2, 3)

test_calculs.py:10: AssertionError
=========================== 1 failed, 1 passed in ...s ===========================
```

`pytest` a ré-écrit l'assertion pour nous montrer exactement quelles valeurs ont été comparées. C'est beaucoup plus informatif qu'un simple `AssertionError`.

## 4. Les Fixtures : Gérer le Contexte des Tests

Les **fixtures** sont le cœur de la puissance de `pytest`. Une fixture est une fonction qui prépare des données ou un état nécessaire pour un test. `pytest` les exécute avant le test qui en a besoin et "injecte" leur résultat dans le test.

On déclare une fixture avec le décorateur `@pytest.fixture`.

### Exemple simple de fixture

Imaginons que plusieurs tests ont besoin d'une liste de nombres de départ.

```python
# test_liste.py
import pytest

@pytest.fixture
def liste_nombres():
    """Une fixture qui fournit une liste de nombres pour les tests."""
    print("\n(Création de la liste via la fixture)")
    return [1, 2, 3, 4, 5]

def test_longueur_liste(liste_nombres):
    """Teste la longueur de la liste fournie par la fixture."""
    # 'liste_nombres' est le résultat de la fixture
    assert len(liste_nombres) == 5

def test_somme_liste(liste_nombres):
    """Teste la somme de la liste fournie par la fixture."""
    assert sum(liste_nombres) == 15
```

- Pour utiliser une fixture, on l'ajoute simplement comme argument à la fonction de test.
- `pytest` voit l'argument, cherche une fixture avec le même nom, l'exécute, et passe sa valeur de retour au test.
- Par défaut, la fixture est exécutée **une fois pour chaque test** qui l'utilise, garantissant l'isolation des tests.

### Portée des Fixtures (`scope`)

On peut contrôler la fréquence d'exécution d'une fixture avec l'argument `scope`.

- `scope='function'` (défaut) : Exécutée une fois par test.
- `scope='class'` : Exécutée une fois par classe de test.
- `scope='module'` : Exécutée une fois par fichier de test.
- `scope='session'` : Exécutée une fois pour toute la session de test.

C'est utile pour des opérations de configuration coûteuses (ex: connexion à une base de données, création d'un gros objet).

```python
@pytest.fixture(scope='module')
def connexion_db():
    """Fixture coûteuse, exécutée une seule fois pour tout le module."""
    print("\n(Connexion à la base de données...)")
    db = ... # Code de connexion
    yield db # 'yield' est utilisé pour le nettoyage
    print("\n(Fermeture de la connexion à la base de données...)")
    db.close()
```

L'utilisation de `yield` dans une fixture permet de séparer la partie "setup" (avant `yield`) de la partie "teardown" (après `yield`), qui sera exécutée à la fin de la portée de la fixture.

## 5. Paramétrer les Tests

Le décorateur `@pytest.mark.parametrize` permet d'exécuter le même test avec plusieurs jeux de données différents.

```python
import pytest

@pytest.mark.parametrize("a, b, attendu", [
    (2, 3, 5),      # Test 1
    (-1, 1, 0),     # Test 2
    (0, 0, 0),      # Test 3
    (-5, -5, -10),  # Test 4
])
def test_addition_parametrise(a, b, attendu):
    assert addition(a, b) == attendu
```

Ce code définit **un seul test**, mais `pytest` l'exécutera **quatre fois**, une pour chaque tuple de données. C'est une manière très propre et lisible de tester de nombreux cas.

## 6. Quelques Fonctionnalités Utiles

- **Tester les exceptions** :

  ```python
  def test_division_par_zero():
      with pytest.raises(ZeroDivisionError):
          1 / 0
  ```

  Le test réussit si une `ZeroDivisionError` est levée dans le bloc `with`.

- **Marqueurs (`markers`)** : Organiser les tests en catégories.

  ```python
  @pytest.mark.slow
  def test_tres_lent():
      ...

  @pytest.mark.network
  def test_appel_api():
      ...
  ```

  On peut ensuite lancer uniquement une catégorie de tests : `pytest -m slow`.

- **Fichier `conftest.py`** : Un fichier spécial où l'on peut placer des fixtures pour qu'elles soient disponibles pour tous les tests d'un dossier et de ses sous-dossiers.

## Conclusion

`pytest` est un outil qui transforme l'écriture de tests en une tâche agréable et productive. En utilisant des fonctions de test simples, des assertions claires, et le puissant système de fixtures, vous pouvez construire des suites de tests robustes et maintenables. L'investissement dans l'apprentissage de `pytest` est l'un des plus rentables pour un développeur Python souhaitant améliorer la qualité de son code.
