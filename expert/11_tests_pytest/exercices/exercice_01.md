# Exercice 01 : Tester une Classe avec des Fixtures et `parametrize`

## Objectif

Cet exercice a pour but de vous faire utiliser les fonctionnalités de base et intermédiaires de `pytest`, notamment les fixtures pour la préparation d'objets et `@pytest.mark.parametrize` pour tester plusieurs cas de figure.

## Contexte

Vous allez développer une classe `Portefeuille` qui gère un solde. Vous écrirez ensuite une suite de tests avec `pytest` pour valider son comportement dans différentes situations : dépôt, retrait, et retrait non autorisé.

## Énoncé

### Étape 1 : Créer la classe à tester

Créez un fichier nommé `portefeuille.py` et définissez la classe `Portefeuille`.

```python
# portefeuille.py

class SoldeInsuffisantError(Exception):
    """Exception levée lors d'un retrait supérieur au solde."""
    pass

class Portefeuille:
    """Classe pour gérer un solde simple."""

    def __init__(self, solde_initial: float = 0):
        if solde_initial < 0:
            raise ValueError("Le solde initial ne peut pas être négatif.")
        self._solde = solde_initial

    @property
    def solde(self) -> float:
        return self._solde

    def deposer(self, montant: float):
        if montant <= 0:
            raise ValueError("Le montant du dépôt doit être positif.")
        self._solde += montant

    def retirer(self, montant: float):
        if montant <= 0:
            raise ValueError("Le montant du retrait doit être positif.")
        if montant > self._solde:
            raise SoldeInsuffisantError("Le retrait ne peut pas excéder le solde.")
        self._solde -= montant
```

### Étape 2 : Créer le fichier de test et les fixtures

Créez un fichier `test_portefeuille.py`.

1.  **Importez `pytest` et la classe `Portefeuille`** (ainsi que les exceptions).

2.  **Créez deux fixtures** :
    - Une fixture `portefeuille_vide` qui retourne une instance de `Portefeuille` avec un solde de 0.
    - Une fixture `portefeuille_avec_20` qui retourne une instance de `Portefeuille` avec un solde de 20.

### Étape 3 : Écrire les tests

1.  **Test du solde initial** :

    - Écrivez un test `test_solde_initial` qui utilise la fixture `portefeuille_vide` et vérifie que son solde est bien de 0.
    - Écrivez un test `test_solde_initial_avec_montant` qui utilise la fixture `portefeuille_avec_20` et vérifie que son solde est bien de 20.

2.  **Test de la méthode `deposer`** :

    - Écrivez un test `test_depot` qui utilise la fixture `portefeuille_vide`, dépose 10, et vérifie que le nouveau solde est de 10.

3.  **Test de la méthode `retirer`** :

    - Écrivez un test `test_retrait` qui utilise la fixture `portefeuille_avec_20`, retire 10, et vérifie que le nouveau solde est de 10.

4.  **Test des exceptions** :

    - Écrivez un test `test_retrait_solde_insuffisant` qui utilise la fixture `portefeuille_avec_20` et vérifie qu'un `SoldeInsuffisantError` est bien levé lorsqu'on essaie de retirer 30. Utilisez `with pytest.raises(...)`.
    - Écrivez un test `test_creation_solde_initial_negatif` qui vérifie qu'une `ValueError` est levée lorsqu'on essaie de créer un `Portefeuille` avec un solde initial de -10.

5.  **Paramétrer les tests** :
    - Les tests pour les dépôts et retraits avec des montants invalides (négatifs ou nuls) sont de bons candidats pour la paramétrisation.
    - Créez un test `test_depot_montant_invalide` paramétré pour tester qu'un dépôt de 0 et de -10 lève bien une `ValueError`.
    - Créez un test `test_retrait_montant_invalide` paramétré de la même manière.

### Étape 4 : Lancer les tests

- Assurez-vous d'avoir `pytest` installé (`pip install pytest`).
- Depuis votre terminal, dans le dossier du projet, lancez la commande `pytest`.

## Résultat Attendu

Tous vos tests doivent passer. `pytest` devrait découvrir et exécuter tous les tests que vous avez écrits, y compris les cas paramétrés, et afficher un résumé indiquant que tous ont réussi.

```
============================= test session starts ==============================
...
collected 8 items

test_portefeuille.py ........                                              [100%]

============================== 8 passed in ...s ================================
```

_(Le nombre de tests peut varier si vous avez structuré votre test paramétré différemment, mais tous doivent passer)._

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# test_portefeuille.py

import pytest
from portefeuille import Portefeuille, SoldeInsuffisantError

# --- Fixtures ---

@pytest.fixture
def portefeuille_vide():
    """Retourne un portefeuille avec un solde de 0."""
    return Portefeuille()

@pytest.fixture
def portefeuille_avec_20():
    """Retourne un portefeuille avec un solde de 20."""
    return Portefeuille(20)

# --- Tests ---

def test_solde_initial(portefeuille_vide):
    assert portefeuille_vide.solde == 0

def test_solde_initial_avec_montant(portefeuille_avec_20):
    assert portefeuille_avec_20.solde == 20

def test_depot(portefeuille_vide):
    portefeuille_vide.deposer(10)
    assert portefeuille_vide.solde == 10

def test_retrait(portefeuille_avec_20):
    portefeuille_avec_20.retirer(10)
    assert portefeuille_avec_20.solde == 10

def test_retrait_solde_insuffisant(portefeuille_avec_20):
    with pytest.raises(SoldeInsuffisantError):
        portefeuille_avec_20.retirer(30)

def test_creation_solde_initial_negatif():
    with pytest.raises(ValueError):
        Portefeuille(-10)

@pytest.mark.parametrize("montant_invalide", [0, -10, -0.01])
def test_depot_montant_invalide(portefeuille_vide, montant_invalide):
    with pytest.raises(ValueError):
        portefeuille_vide.deposer(montant_invalide)

@pytest.mark.parametrize("montant_invalide", [0, -20])
def test_retrait_montant_invalide(portefeuille_avec_20, montant_invalide):
    with pytest.raises(ValueError):
        portefeuille_avec_20.retirer(montant_invalide)

```

</details>
