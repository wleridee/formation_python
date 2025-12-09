# Exercice 01 : Comparaison du Bytecode pour Différentes Structures de Données

## Objectif

Cet exercice a pour but de vous faire utiliser le module `dis` pour comparer le bytecode généré pour des opérations similaires sur des listes et des tuples. Cela vous aidera à comprendre pourquoi certaines opérations sont plus efficaces sur un type de données que sur un autre.

## Contexte

Les listes sont des structures de données muables, tandis que les tuples sont immuables. Cette différence fondamentale a un impact direct sur le bytecode généré par l'interpréteur et, par conséquent, sur les performances.

Vous allez comparer deux opérations :

1.  La création d'une structure de données littérale.
2.  L'ajout d'un élément à une structure de données existante.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `bytecode_comparison.py`.

2.  **Importez le module `dis`**.

3.  **Définissez quatre fonctions simples** :

    a. `creer_liste()`: Cette fonction doit simplement retourner une liste littérale, par exemple `[1, 2, 3]`.
    b. `creer_tuple()`: Cette fonction doit retourner un tuple littéral, par exemple `(1, 2, 3)`.
    c. `ajouter_a_liste(ma_liste)`: Cette fonction prend une liste en argument et lui ajoute un élément en utilisant l'opérateur `+=`, par exemple `ma_liste += [4]`.
    d. `ajouter_a_tuple(mon_tuple)`: Cette fonction prend un tuple en argument et lui "ajoute" un élément en utilisant `+=`, par exemple `mon_tuple += (4,)`.

4.  **Utilisez `dis.dis()` pour inspecter chaque fonction**.

    - Ajoutez des `print` pour séparer et légender clairement la sortie de chaque désassemblage.
    - Appelez `dis.dis()` sur chacune des quatre fonctions que vous avez créées.

5.  **Analysez et commentez les différences**.
    - Regardez le bytecode pour `creer_liste` et `creer_tuple`. Quelle est la différence principale ? Laquelle semble la plus directe ?
    - Comparez le bytecode pour `ajouter_a_liste` et `ajouter_a_tuple`. Concentrez-vous sur l'instruction `INPLACE_ADD`. Comment Python gère-t-il cette opération pour un type muable par rapport à un type immuable ? Que pouvez-vous en déduire sur l'efficacité de "l'ajout" à un tuple ?

## Résultat Attendu

Votre script doit afficher le bytecode désassemblé pour les quatre fonctions. Vous devriez être capable de voir et d'expliquer les différences clés.

```
--- Création de Liste ---
  ...
  2           0 LOAD_CONST               1 (1)
              2 LOAD_CONST               2 (2)
              4 LOAD_CONST               3 (3)
              6 BUILD_LIST               3
              8 RETURN_VALUE

--- Création de Tuple ---
  ...
  6           0 LOAD_CONST               4 ((1, 2, 3))
              2 RETURN_VALUE

--- Ajout à une Liste (muable) ---
  ...
 10           0 LOAD_FAST                0 (ma_liste)
              2 LOAD_CONST               1 (4)
              4 BUILD_LIST               1
              6 INPLACE_ADD
              8 STORE_FAST               0 (ma_liste)
             ...

--- Ajout à un Tuple (immuable) ---
  ...
 14           0 LOAD_FAST                0 (mon_tuple)
              2 LOAD_CONST               2 ((4,))
              4 INPLACE_ADD
              6 STORE_FAST               0 (mon_tuple)
             ...
```

**Questions pour votre analyse :**

1.  **Création :** Pourquoi la création du tuple utilise-t-elle `LOAD_CONST` directement pour toute la structure, alors que la liste est construite élément par élément avec `BUILD_LIST` ? (Indice : immuabilité et optimisation par le compilateur).
2.  **Ajout :** L'instruction `INPLACE_ADD` est présente dans les deux cas. Cependant, pour le tuple, cette opération implique la création d'un tout nouvel objet tuple et la réassignation. Pour la liste, elle modifie l'objet existant. Comment le bytecode reflète-t-il cela (ou ne le reflète-t-il pas directement, forçant à connaître la sémantique de l'opcode) ?

<details>
<summary>Cliquez ici pour voir un exemple de code de solution et d'analyse</summary>

```python
# bytecode_comparison.py

import dis

# --- Fonctions pour la création ---

def creer_liste():
    return [1, 2, 3]

def creer_tuple():
    return (1, 2, 3)

# --- Fonctions pour l'ajout ---

def ajouter_a_liste(ma_liste):
    ma_liste += [4]
    return ma_liste

def ajouter_a_tuple(mon_tuple):
    mon_tuple += (4,)
    return mon_tuple

# --- Désassemblage ---

print("--- Création de Liste ---")
dis.dis(creer_liste)
# Analyse : La liste est construite dynamiquement à l'exécution.
# Chaque élément est chargé comme une constante, puis BUILD_LIST les assemble.

print("\n--- Création de Tuple ---")
dis.dis(creer_tuple)
# Analyse : Le tuple entier est une constante. Parce qu'il est immuable,
# le compilateur peut le créer une seule fois et le charger directement.
# C'est plus rapide.

print("\n--- Ajout à une Liste (muable) ---")
dis.dis(ajouter_a_liste)
# Analyse : INPLACE_ADD pour une liste modifie l'objet en place.
# C'est une opération efficace en termes de mémoire.

print("\n--- Ajout à un Tuple (immuable) ---")
dis.dis(ajouter_a_tuple)
# Analyse : INPLACE_ADD pour un tuple ne peut pas modifier l'objet.
# En coulisses, Python exécute l'équivalent de BINARY_ADD :
# il crée un nouveau tuple en concaténant l'ancien et le nouveau,
# puis l'assigne à la variable. C'est moins efficace.

```

**Conclusion de l'analyse :**

- **Création :** La création de tuples littéraux est plus rapide que celle de listes littérales car les tuples, étant immuables, peuvent être pré-calculés par le compilateur et stockés comme une seule constante.
- **Modification :** L'ajout à une liste (`INPLACE_ADD`) est une opération en place, rapide et efficace. L'opération sémantiquement similaire sur un tuple est en réalité une opération de création/concaténation, beaucoup plus coûteuse en temps et en mémoire car elle implique la création d'un nouvel objet.

</details>
