# Exercice 01 : Le Videur de Boîte de Nuit

## Objectif

Cet exercice a pour but de vous faire utiliser une structure conditionnelle `if`/`elif`/`else` pour implémenter une logique de décision simple.

## Contexte

Vous êtes le développeur du logiciel pour un videur de boîte de nuit. Vous devez écrire un programme qui décide si un client peut entrer ou non, en fonction de son âge et s'il est accompagné.

## Règles d'Admission

1.  Le client doit avoir 18 ans ou plus pour entrer.
2.  Cependant, il y a une exception : si le client a exactement 17 ans, il peut entrer, mais **uniquement** s'il est accompagné (`is_accompanied` est `True`).
3.  Dans tous les autres cas (moins de 17 ans), l'entrée est refusée.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `bouncer.py`.

2.  **Déclarez les variables suivantes** au début de votre script. Vous changerez leurs valeurs pour tester tous les cas de figure.

    ```python
    age = 17
    is_accompanied = True
    ```

3.  **Écrivez une structure `if`/`elif`/`else`** qui implémente les règles d'admission.

    - Utilisez les opérateurs logiques (`and`) et de comparaison (`>=`, `==`) pour construire vos conditions.
    - Pour chaque cas, affichez un message clair :
      - `"Welcome, you can enter."`
      - `"You can enter, but you must be accompanied."`
      - `"Sorry, you are too young to enter."`

4.  **Testez votre code** en modifiant les valeurs des variables `age` et `is_accompanied` pour couvrir les scénarios suivants :
    - `age = 25`, `is_accompanied = False` (devrait être autorisé)
    - `age = 17`, `is_accompanied = True` (devrait être autorisé avec la mention spéciale)
    - `age = 17`, `is_accompanied = False` (devrait être refusé)
    - `age = 16`, `is_accompanied = True` (devrait être refusé)

## Résultat Attendu

- Pour `age = 25`, `is_accompanied = False` -> `"Welcome, you can enter."`
- Pour `age = 17`, `is_accompanied = True` -> `"You can enter, but you must be accompanied."`
- Pour `age = 17`, `is_accompanied = False` -> `"Sorry, you are too young to enter."`
- Pour `age = 16`, `is_accompanied = True` -> `"Sorry, you are too young to enter."`

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# bouncer.py

age = 25
is_accompanied = False

print(f"Testing with age: {age} and accompanied: {is_accompanied}")

# L'ordre des conditions est important. On traite le cas général d'abord.
if age >= 18:
    print("Welcome, you can enter.")
# Ensuite, on traite le cas spécial pour les 17 ans.
elif age == 17 and is_accompanied:
    print("You can enter, but you must be accompanied.")
# 'else' attrape tous les autres cas (moins de 18 ans sans être dans le cas spécial).
else:
    print("Sorry, you are too young to enter.")

```

**Note sur la logique :** L'ordre dans lequel vous écrivez vos conditions `if`/`elif` est crucial. Il est souvent plus simple de gérer les cas les plus spécifiques ou les plus généraux en premier, en fonction de la logique. Dans cet exemple, commencer par `age >= 18` simplifie grandement la suite.

</details>
