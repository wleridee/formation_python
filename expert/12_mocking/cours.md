# Module : Le Mocking avec `unittest.mock`

## Objectif

Ce module a pour but de vous initier au concept de **mocking** (simulation). Vous apprendrez à utiliser le module `unittest.mock` (qui s'intègre parfaitement avec `pytest`) pour isoler vos tests de leurs dépendances externes, comme les appels réseau, les bases de données ou les systèmes de fichiers.

## 1. Qu'est-ce que le Mocking ?

Un **test unitaire** a pour but de tester une petite unité de code (généralement une fonction ou une méthode) de manière **isolée**.

Cependant, de nombreuses fonctions ont des **dépendances externes** :

- Elles appellent une API via le réseau.
- Elles lisent ou écrivent dans une base de données.
- Elles interagissent avec le système de fichiers.

Ces dépendances posent plusieurs problèmes pour les tests unitaires :

- **Lenteur** : Un appel réseau peut prendre plusieurs secondes.
- **Non-déterminisme** : Une API peut être en panne ou retourner des données différentes.
- **État** : Les tests peuvent modifier une base de données, ce qui affecte les tests suivants.
- **Disponibilité** : On ne peut pas lancer les tests sans connexion réseau ou sans base de données configurée.

Le **mocking** est la technique qui consiste à remplacer ces dépendances par des **objets factices (mocks)** pendant les tests. Ces mocks simulent le comportement de la dépendance réelle de manière contrôlée et prédictible.

## 2. `unittest.mock.Mock` et `MagicMock`

La classe de base pour créer des mocks est `Mock`. `MagicMock` est une sous-classe de `Mock` qui implémente par défaut la plupart des méthodes "magiques" (dunder methods), ce qui la rend plus pratique dans la plupart des cas.

Un `MagicMock` est un objet caméléon :

- Il peut être appelé comme une fonction.
- On peut accéder à n'importe quel attribut sur lui (il le créera à la volée).
- On peut lui assigner un comportement (valeur de retour, exception à lever).
- Il enregistre comment il a été utilisé (avec quels arguments il a été appelé, combien de fois, etc.).

```python
from unittest.mock import MagicMock

# Créer un mock
mock_api = MagicMock()

# 1. Configurer une valeur de retour pour une méthode
mock_api.get_user.return_value = {"id": 1, "name": "Alice"}

# Utiliser le mock
user = mock_api.get_user(1)
print(user) # {"id": 1, "name": "Alice"}

# 2. Configurer un effet de bord (lever une exception)
mock_api.delete_user.side_effect = ConnectionError("Impossible de se connecter")

try:
    mock_api.delete_user(1)
except ConnectionError as e:
    print(e) # Impossible de se connecter

# 3. Vérifier comment le mock a été utilisé
# a-t-il été appelé ?
mock_api.get_user.assert_called()

# a-t-il été appelé une seule fois ?
mock_api.get_user.assert_called_once()

# a-t-il été appelé avec des arguments spécifiques ?
mock_api.get_user.assert_called_with(1)

# a-t-il été appelé avec n'importe quels arguments ?
from unittest.mock import ANY
mock_api.get_user.assert_called_with(ANY)
```

## 3. Remplacer des Objets avec `monkeypatch` ou `mock.patch`

Savoir créer un mock est une chose, mais comment l'injecter dans notre code pour remplacer la vraie dépendance ? `pytest` fournit une fixture très pratique pour cela : `monkeypatch`. `unittest.mock` a son propre outil, `patch`, qui fonctionne comme un décorateur ou un gestionnaire de contexte.

### Exemple à Tester

Imaginons une fonction qui dépend du module `requests` pour appeler une API.

```python
# mon_app.py
import requests

def get_todos_for_user(user_id):
    """Récupère la liste des tâches pour un utilisateur depuis une API externe."""
    response = requests.get(f"https://jsonplaceholder.typicode.com/todos?userId={user_id}")
    response.raise_for_status() # Lève une exception si le statut HTTP est une erreur
    return response.json()
```

Tester cette fonction directement est une mauvaise idée (lenteur, dépendance réseau). Nous allons donc "mocker" `requests.get`.

### Utilisation de la fixture `monkeypatch` de `pytest`

`monkeypatch` permet de modifier dynamiquement des classes, méthodes, variables d'environnement, etc., pour la durée d'un test, et garantit que tout sera restauré à son état original après le test.

```python
# test_mon_app.py
from mon_app import get_todos_for_user
from unittest.mock import MagicMock

def test_get_todos_for_user_success(monkeypatch):
    # 1. Créer un mock pour simuler la réponse de requests.get()
    mock_response = MagicMock()
    mock_response.status_code = 200
    # .json() est une méthode, donc on configure la valeur de retour de l'appel
    mock_response.json.return_value = [{"title": "Test Todo", "completed": False}]

    # 2. Créer un mock pour la fonction requests.get elle-même
    mock_get = MagicMock(return_value=mock_response)

    # 3. Utiliser monkeypatch pour remplacer 'requests.get' par notre mock
    # La chaîne est le chemin d'import complet de ce qu'on veut remplacer.
    monkeypatch.setattr("requests.get", mock_get)

    # 4. Appeler notre fonction. Elle appellera notre mock au lieu du vrai requests.get
    todos = get_todos_for_user(1)

    # 5. Vérifier les résultats
    assert len(todos) == 1
    assert todos[0]["title"] == "Test Todo"

    # 6. Vérifier que le mock a été appelé correctement
    mock_get.assert_called_once_with("https://jsonplaceholder.typicode.com/todos?userId=1")
    mock_response.raise_for_status.assert_called_once()

def test_get_todos_for_user_failure(monkeypatch):
    # Testons le cas où l'API retourne une erreur
    mock_response = MagicMock()
    # On configure la méthode .raise_for_status() pour qu'elle lève une exception
    mock_response.raise_for_status.side_effect = requests.exceptions.HTTPError

    mock_get = MagicMock(return_value=mock_response)
    monkeypatch.setattr("requests.get", mock_get)

    # On vérifie que notre fonction propage bien l'exception
    with pytest.raises(requests.exceptions.HTTPError):
        get_todos_for_user(1)
```

### Utilisation de `unittest.mock.patch`

`patch` est une alternative à `monkeypatch`. Il peut être utilisé comme un décorateur ou un gestionnaire de contexte.

```python
# test_mon_app_patch.py
from unittest.mock import patch
import pytest

# 'patch' comme décorateur. Le mock est passé en argument au test.
@patch("requests.get")
def test_get_todos_with_decorator(mock_get):
    # 'mock_get' est maintenant un MagicMock qui remplace requests.get

    # Configurer la chaîne de mocks
    mock_get.return_value.status_code = 200
    mock_get.return_value.json.return_value = [{"title": "Test Todo"}]

    todos = get_todos_for_user(1)

    assert len(todos) == 1
    mock_get.assert_called_once_with("https://jsonplaceholder.typicode.com/todos?userId=1")

def test_get_todos_with_context_manager():
    # 'patch' comme gestionnaire de contexte
    with patch("requests.get") as mock_get:
        mock_get.return_value.status_code = 200
        mock_get.return_value.json.return_value = [{"title": "Test Todo"}]

        todos = get_todos_for_user(1)

        assert len(todos) == 1
        mock_get.assert_called_once_with("https://jsonplaceholder.typicode.com/todos?userId=1")
```

**`monkeypatch` vs `patch` :**

- `monkeypatch` est une fixture `pytest`, elle est donc souvent plus naturelle à utiliser dans un écosystème `pytest`.
- `patch` fait partie de la bibliothèque standard et est plus portable si vous n'utilisez pas `pytest`.
- Les deux atteignent le même objectif. Le choix est souvent une question de préférence.

## Conclusion

Le mocking est une technique indispensable pour écrire des tests unitaires rapides, fiables et véritablement "unitaires". En remplaçant les dépendances externes par des objets `MagicMock` contrôlés, vous pouvez tester le comportement de votre code dans une multitude de scénarios (succès, échec, cas limites) sans dépendre de systèmes externes. La fixture `monkeypatch` de `pytest` et `unittest.mock.patch` sont les deux principaux outils pour mettre en œuvre cette technique de manière propre et efficace.
