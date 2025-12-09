# Exercice 01 : Mocker un Appel API pour Tester une Fonction

## Objectif

Cet exercice a pour but de vous faire utiliser la fixture `monkeypatch` de `pytest` pour mocker un appel réseau et tester une fonction qui dépend d'une API externe.

## Contexte

Vous allez écrire une fonction qui récupère la date et l'heure actuelles à partir de l'API de [WorldTimeAPI](http://worldtimeapi.org/). Cette API retourne un objet JSON avec des informations sur l'heure dans un fuseau horaire donné.

Votre fonction devra parser cette réponse pour extraire la date et l'heure. Comme nous ne voulons pas que nos tests dépendent d'une connexion internet et d'une API externe, vous allez mocker l'appel à `requests.get` pour simuler la réponse de l'API.

**Exemple de réponse de l'API pour l'Europe/Paris :**
URL : `http://worldtimeapi.org/api/timezone/Europe/Paris`
Réponse JSON (simplifiée) :

```json
{
  "datetime": "2023-10-27T10:30:00.123456+02:00",
  "timezone": "Europe/Paris",
  "utc_offset": "+02:00"
}
```

## Énoncé

### Étape 1 : Créer la fonction à tester

Créez un fichier `time_app.py` avec la fonction suivante :

```python
# time_app.py
import requests

def get_current_time(timezone: str) -> str:
    """
    Récupère l'heure actuelle pour un fuseau horaire donné depuis l'API WorldTimeAPI
    et retourne la partie 'datetime' de la réponse.
    """
    try:
        response = requests.get(f"http://worldtimeapi.org/api/timezone/{timezone}")
        # Lève une exception pour les codes d'erreur HTTP (4xx ou 5xx)
        response.raise_for_status()
        data = response.json()
        return data["datetime"]
    except requests.exceptions.RequestException as e:
        # En cas d'erreur réseau ou HTTP, on retourne un message d'erreur
        return f"Erreur lors de la récupération de l'heure : {e}"
```

### Étape 2 : Créer le fichier de test

Créez un fichier `test_time_app.py`.

1.  **Importez les modules nécessaires** : `pytest`, `requests`, `MagicMock` de `unittest.mock`, et la fonction `get_current_time`.

2.  **Écrivez un test pour le cas de succès**.

    - Nommez la fonction `test_get_current_time_success`.
    - Elle doit prendre `monkeypatch` en argument.
    - **Configurez le mock** :
      - Créez un `MagicMock` pour simuler l'objet `response`.
      - Définissez la valeur de retour de la méthode `json()` de ce mock pour qu'elle soit un dictionnaire simulant une réponse réussie de l'API (par exemple, `{"datetime": "2023-01-01T12:00:00+01:00"}`).
    - **Créez un mock pour `requests.get`** qui retourne votre `mock_response`.
    - **Appliquez le patch** : Utilisez `monkeypatch.setattr("requests.get", ...)` pour remplacer la vraie fonction `get` par votre mock.
    - **Appelez la fonction testée** : `get_current_time("Europe/Paris")`.
    - **Vérifiez le résultat** : Assurez-vous que la fonction retourne bien la chaîne de caractères `datetime` que vous avez définie dans votre mock.
    - **Vérifiez l'appel** : Assurez-vous que `requests.get` a bien été appelé une fois avec l'URL correcte.

3.  **Écrivez un test pour le cas d'échec (erreur HTTP)**.
    - Nommez la fonction `test_get_current_time_http_error`.
    - Elle doit aussi prendre `monkeypatch` en argument.
    - **Configurez le mock** :
      - Cette fois, configurez la méthode `raise_for_status()` du `mock_response` pour qu'elle lève une `requests.exceptions.HTTPError`. Utilisez l'argument `side_effect`.
    - **Appliquez le patch** comme précédemment.
    - **Appelez la fonction testée**.
    - **Vérifiez le résultat** : Assurez-vous que la fonction retourne une chaîne de caractères contenant le message "Erreur".

### Étape 3 : Lancer les tests

- Installez `pytest` et `requests` (`pip install pytest requests`).
- Lancez `pytest` depuis votre terminal.

## Résultat Attendu

Les deux tests doivent passer, prouvant que vous avez testé à la fois le chemin de succès et le chemin d'erreur de votre fonction, sans jamais effectuer un seul appel réseau réel.

```
============================= test session starts ==============================
...
collected 2 items

test_time_app.py ..                                                        [100%]

============================== 2 passed in ...s ================================
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# test_time_app.py

import pytest
import requests
from unittest.mock import MagicMock
from time_app import get_current_time

def test_get_current_time_success(monkeypatch):
    """
    Teste le cas où l'API répond correctement.
    """
    # 1. Préparation des données de simulation
    fake_response_data = {"datetime": "2023-01-01T12:00:00+01:00"}

    # 2. Création et configuration des mocks
    mock_response = MagicMock()
    # La méthode .json() du mock doit retourner nos fausses données
    mock_response.json.return_value = fake_response_data
    # On s'assure que raise_for_status() ne fait rien
    mock_response.raise_for_status.return_value = None

    # Le mock pour la fonction get doit retourner notre mock_response
    mock_get = MagicMock(return_value=mock_response)

    # 3. Remplacement de la vraie fonction par le mock
    monkeypatch.setattr(requests, "get", mock_get)

    # 4. Appel de la fonction à tester
    timezone = "Europe/Paris"
    result = get_current_time(timezone)

    # 5. Assertions
    # Le résultat est-il correct ?
    assert result == "2023-01-01T12:00:00+01:00"
    # La fonction a-t-elle été appelée correctement ?
    mock_get.assert_called_once_with(f"http://worldtimeapi.org/api/timezone/{timezone}")
    mock_response.raise_for_status.assert_called_once()


def test_get_current_time_http_error(monkeypatch):
    """
    Teste le cas où l'API retourne une erreur HTTP (ex: 404, 500).
    """
    # 1. Création et configuration des mocks
    mock_response = MagicMock()
    # On configure raise_for_status pour qu'elle lève une exception
    error_message = "Not Found"
    mock_response.raise_for_status.side_effect = requests.exceptions.HTTPError(error_message)

    mock_get = MagicMock(return_value=mock_response)

    # 2. Remplacement
    monkeypatch.setattr(requests, "get", mock_get)

    # 3. Appel
    result = get_current_time("Zone/Invalide")

    # 4. Assertions
    # Le résultat doit contenir le message d'erreur
    assert "Erreur" in result
    assert error_message in result

```

</details>
