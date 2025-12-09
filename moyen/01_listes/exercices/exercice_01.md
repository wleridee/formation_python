# Exercice 01 : Gestion d'une Liste de Tâches (To-Do List)

## Objectif

Cet exercice a pour but de vous faire manipuler une liste en utilisant les opérations de base : ajout, suppression, affichage et modification, dans le contexte d'une simple application de liste de tâches.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `todo_list.py`.

2.  **Initialisez une liste vide** nommée `tasks` pour stocker vos tâches.

3.  **Ajoutez les tâches initiales** suivantes à votre liste, une par une, en utilisant la méthode `.append()`:

    - `"Learn Python basics"`
    - `"Practice list manipulations"`
    - `"Build a simple application"`

4.  **Affichez la liste** :

    - Affichez un titre "My To-Do List:"
    - Utilisez une boucle `for` pour afficher chaque tâche sur une nouvelle ligne.

5.  **Marquez une tâche comme terminée** :

    - Imaginez que vous avez terminé la première tâche. Modifiez l'élément à l'index 0 pour qu'il devienne `"Learn Python basics - DONE"`.

6.  **Ajoutez une nouvelle tâche urgente** :

    - Ajoutez la tâche `"Review list methods"` au **début** de la liste en utilisant la méthode `.insert()`.

7.  **Supprimez une tâche** :

    - Supposons que la tâche `"Build a simple application"` n'est plus pertinente. Supprimez-la de la liste en utilisant la méthode `.remove()`.

8.  **Affichez la liste finale** :
    - Affichez un titre "Final To-Do List:"
    - Affichez à nouveau chaque tâche de la liste mise à jour.

## Résultat Attendu

La sortie de votre script doit être :

```
My To-Do List:
Learn Python basics
Practice list manipulations
Build a simple application

Final To-Do List:
Review list methods
Learn Python basics - DONE
Practice list manipulations
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# todo_list.py

# 1. Initialisation
tasks = []

# 2. Ajout des tâches initiales
tasks.append("Learn Python basics")
tasks.append("Practice list manipulations")
tasks.append("Build a simple application")

# 3. Affichage de la liste initiale
print("My To-Do List:")
for task in tasks:
    print(task)

# Ajout d'un saut de ligne pour la clarté
print("\n")

# 4. Marquer une tâche comme terminée
tasks[0] = "Learn Python basics - DONE"

# 5. Ajouter une tâche urgente au début
tasks.insert(0, "Review list methods")

# 6. Supprimer une tâche par sa valeur
tasks.remove("Build a simple application")

# 7. Affichage de la liste finale
print("Final To-Do List:")
for task in tasks:
    print(task)
```

</details>
