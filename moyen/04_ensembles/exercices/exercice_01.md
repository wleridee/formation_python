# Exercice 01 : Gestion des Compétences des Employés

## Objectif

Cet exercice a pour but de vous faire utiliser les ensembles et leurs opérations (intersection, différence) pour comparer les compétences de deux employés.

## Contexte

Vous travaillez pour un service RH et vous devez analyser les compétences des employés pour un projet. Vous avez les compétences de deux employés, et vous voulez savoir :

- Quelles compétences ils ont en commun.
- Quelles compétences sont uniques à chaque employé.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `skill_analyzer.py`.

2.  **Définissez les compétences** de deux employés en utilisant des ensembles :

    ```python
    employee_a_skills = {"Python", "Git", "SQL", "HTML", "CSS"}
    employee_b_skills = {"Python", "JavaScript", "HTML", "React", "SQL"}
    ```

3.  **Trouvez les compétences communes** :

    - Calculez l'**intersection** des deux ensembles pour trouver les compétences que les deux employés possèdent.
    - Stockez le résultat dans un ensemble `common_skills`.
    - Affichez le résultat.

4.  **Trouvez les compétences uniques à chaque employé** :

    - Calculez la **différence** entre les compétences de l'employé A et celles de l'employé B pour trouver ce que seul l'employé A sait faire. Stockez le résultat dans `unique_to_a`.
    - Faites l'inverse pour trouver les compétences uniques à l'employé B. Stockez le résultat dans `unique_to_b`.
    - Affichez ces deux résultats.

5.  **Listez toutes les compétences requises pour le projet** :
    - Calculez l'**union** des deux ensembles pour obtenir un ensemble de toutes les compétences disponibles.
    - Stockez le résultat dans `total_skills`.
    - Affichez le résultat.

## Résultat Attendu

La sortie de votre script doit ressembler à ceci (l'ordre des éléments dans les ensembles n'a pas d'importance) :

```
Common skills: {'Python', 'SQL', 'HTML'}
Skills unique to Employee A: {'CSS', 'Git'}
Skills unique to Employee B: {'JavaScript', 'React'}
All skills required for the project: {'CSS', 'Python', 'JavaScript', 'Git', 'React', 'SQL', 'HTML'}
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# skill_analyzer.py

# 1. Définition des compétences
employee_a_skills = {"Python", "Git", "SQL", "HTML", "CSS"}
employee_b_skills = {"Python", "JavaScript", "HTML", "React", "SQL"}

# 2. Compétences communes (Intersection)
common_skills = employee_a_skills.intersection(employee_b_skills)
# Alternative : common_skills = employee_a_skills & employee_b_skills
print(f"Common skills: {common_skills}")

# 3. Compétences uniques (Différence)
unique_to_a = employee_a_skills.difference(employee_b_skills)
# Alternative : unique_to_a = employee_a_skills - employee_b_skills
print(f"Skills unique to Employee A: {unique_to_a}")

unique_to_b = employee_b_skills.difference(employee_a_skills)
# Alternative : unique_to_b = employee_b_skills - employee_a_skills
print(f"Skills unique to Employee B: {unique_to_b}")

# 4. Toutes les compétences (Union)
total_skills = employee_a_skills.union(employee_b_skills)
# Alternative : total_skills = employee_a_skills | employee_b_skills
print(f"All skills required for the project: {total_skills}")

```

</details>
