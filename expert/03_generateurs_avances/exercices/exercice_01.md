# Exercice 01 : Coroutine pour Filtrer des Données

## Objectif

Cet exercice a pour but de vous faire créer une coroutine (basée sur un générateur) qui reçoit des données, les filtre selon un critère, et les envoie à une autre coroutine "cible". Cela illustre comment chaîner des coroutines pour créer un pipeline de traitement.

## Contexte

Les pipelines de coroutines sont un patron de conception puissant pour traiter des flux de données. Chaque coroutine agit comme une étape de traitement : elle reçoit des données de la source (ou de l'étape précédente), effectue une opération, et passe le résultat à l'étape suivante.

Dans cet exercice, vous allez construire un pipeline simple à deux étapes :

1.  Une coroutine qui filtre des nombres pour ne garder que les multiples d'un certain nombre.
2.  Une coroutine "puits" (`sink`) qui reçoit les données filtrées et les affiche.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `coroutine_pipeline.py`.

2.  **Créez une coroutine "puits" `print_sink`**.

    - Cette coroutine doit être décorée avec un décorateur d'amorçage (voir ci-dessous) pour éviter d'avoir à appeler `next()` manuellement.
    - Elle doit tourner dans une boucle infinie (`while True`).
    - À chaque itération, elle attend de recevoir une valeur avec `(yield)`.
    - Elle affiche la valeur reçue avec un préfixe, par exemple : `f"Sink: a reçu {valeur}"`.

3.  **Créez un décorateur d'amorçage `@coroutine`**.

    - Ce décorateur prend une fonction générateur en argument.
    - Il définit une fonction _wrapper_ qui :
      - Crée le générateur en appelant la fonction.
      - Amorce le générateur en appelant `next()` dessus.
      - Retourne le générateur amorcé.
    - Ce décorateur simplifiera grandement l'utilisation de nos coroutines.

4.  **Créez la coroutine de filtrage `filter_multiple`**.

    - Décorez-la avec `@coroutine`.
    - Elle doit accepter deux arguments : `multiple` (l'entier pour le test de divisibilité) et `target` (la coroutine cible où envoyer les résultats).
    - Dans une boucle infinie, elle doit :
      - Attendre de recevoir un nombre avec `nombre = (yield)`.
      - Vérifier si `nombre` est un multiple de `multiple`.
      - Si c'est le cas, envoyer ce nombre à la coroutine `target` en utilisant `target.send(nombre)`.

5.  **Mettez en place le pipeline dans le bloc principal**.
    - Créez une instance de la coroutine puits `print_sink`.
    - Créez une instance de la coroutine de filtrage `filter_multiple`, en lui passant `3` comme multiple et le `print_sink` comme cible.
    - Envoyez une série de nombres (de 1 à 10, par exemple) à la coroutine de filtrage en utilisant une boucle `for` et la méthode `send()`.

## Résultat Attendu

Seuls les nombres multiples de 3 doivent être affichés par la coroutine "puits".

```
Sink: a reçu 3
Sink: a reçu 6
Sink: a reçu 9
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# coroutine_pipeline.py

from functools import wraps

def coroutine(func):
    """Décorateur pour amorcer automatiquement une coroutine."""
    @wraps(func)
    def wrapper(*args, **kwargs):
        gen = func(*args, **kwargs)
        next(gen) # Amorce le générateur
        return gen
    return wrapper

@coroutine
def print_sink():
    """Coroutine qui agit comme un puits de données, affichant ce qu'elle reçoit."""
    print("Sink: Prêt à recevoir des données.")
    try:
        while True:
            valeur = (yield)
            print(f"Sink: a reçu {valeur}")
    except GeneratorExit:
        print("Sink: Fermeture.")

@coroutine
def filter_multiple(multiple: int, target):
    """
    Coroutine qui reçoit des nombres, filtre les multiples de 'multiple',
    et les envoie à 'target'.
    """
    print(f"Filter: Prêt à filtrer les multiples de {multiple}.")
    try:
        while True:
            nombre = (yield)
            if nombre % multiple == 0:
                target.send(nombre)
    except GeneratorExit:
        target.close() # Propage la fermeture à la cible

# --- Mise en place du pipeline ---
if __name__ == "__main__":
    # 1. Créer le puits (la fin du pipeline)
    sink = print_sink()

    # 2. Créer le filtre, en le connectant au puits
    # Le filtre enverra ses résultats à 'sink'
    filtrage = filter_multiple(3, sink)

    # 3. Envoyer des données au début du pipeline (le filtre)
    print("\nEnvoi des données au pipeline...")
    for i in range(1, 11):
        print(f"Source: envoi de {i}")
        filtrage.send(i)

    # 4. Fermer le pipeline
    print("\nFermeture du pipeline.")
    filtrage.close()

```

</details>
