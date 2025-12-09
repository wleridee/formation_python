# Module : Profilage et Optimisation avec `cProfile`

## Objectif

Ce module a pour but de vous apprendre à profiler votre code Python pour identifier les goulots d'étranglement. Vous découvrirez comment utiliser le module `cProfile` pour collecter des statistiques de performance et comment analyser ces données pour guider vos efforts d'optimisation.

> "L'optimisation prématurée est la racine de tous les maux." - Donald Knuth

Cette citation célèbre nous rappelle qu'il ne faut pas optimiser à l'aveugle. Le profilage est l'outil qui nous permet de savoir **où** et **quoi** optimiser.

## 1. Qu'est-ce que le Profilage ?

Le profilage est le processus d'analyse d'un programme pour déterminer quelles parties consomment le plus de ressources (temps CPU, mémoire, etc.). Un **profileur** est un outil qui collecte ces informations pendant l'exécution du programme.

En Python, le profileur standard pour le temps d'exécution est `cProfile`. Il est écrit en C, ce qui le rend très rapide et adapté pour profiler même des programmes complexes avec un surcoût minimal.

`cProfile` enregistre des informations sur chaque appel de fonction :

- Le nombre de fois où la fonction a été appelée.
- Le temps total passé dans la fonction.
- Le temps cumulé passé dans la fonction et toutes les fonctions qu'elle a appelées.

## 2. Utiliser `cProfile`

Il y a plusieurs manières d'utiliser `cProfile`.

### a. Depuis la Ligne de Commande

C'est la manière la plus simple de profiler un script entier sans modifier son code.

```bash
python -m cProfile -o stats.prof mon_script.py
```

- `python -m cProfile`: Exécute le module `cProfile` comme un script.
- `-o stats.prof`: (Optionnel) Sauvegarde les résultats du profilage dans un fichier binaire nommé `stats.prof`. C'est la méthode recommandée car elle permet une analyse plus poussée.
- `mon_script.py`: Le script à profiler.

Si vous n'utilisez pas l'option `-o`, les résultats sont affichés directement dans la console, mais ils peuvent être très longs et difficiles à lire.

### b. Dans le Code

Vous pouvez aussi utiliser `cProfile` directement dans votre code pour profiler une section spécifique.

```python
import cProfile
import pstats

def ma_fonction_lente():
    # ... du code à profiler ...
    total = 0
    for i in range(1000000):
        total += i
    return total

# Créer un objet Profile
profiler = cProfile.Profile()

# Activer le profileur
profiler.enable()

# Exécuter le code à profiler
ma_fonction_lente()

# Désactiver le profileur
profiler.disable()

# Sauvegarder les statistiques
profiler.dump_stats("stats.prof")
```

Cette approche est utile pour isoler une partie précise de votre application.

## 3. Analyser les Résultats avec `pstats`

Une fois que vous avez votre fichier de statistiques (`stats.prof`), le module `pstats` (Python Stats) devient votre meilleur ami. Il fournit une interface interactive pour trier et analyser les données.

Lancez l'interpréteur `pstats` depuis la ligne de commande :

```bash
python -m pstats stats.prof
```

Vous obtiendrez un prompt `(stats.prof)`. Voici les commandes les plus utiles :

- `stats [n]`: Affiche les statistiques. `n` est un entier optionnel pour limiter le nombre de lignes.
- `sort <clé>`: Trie le rapport selon une clé. C'est la commande la plus importante.
- `callers [n]`: Affiche les fonctions qui ont appelé d'autres fonctions.
- `callees [n]`: Affiche les fonctions qui ont été appelées par d'autres fonctions.
- `help`: Affiche l'aide.
- `quit`: Quitte l'interpréteur.

### Les Clés de Tri Essentielles

- `tottime` (Total Time) : **Temps total passé dans la fonction elle-même**, sans compter le temps passé dans les sous-fonctions qu'elle appelle. C'est la métrique la plus importante pour trouver les fonctions qui sont lentes _par elles-mêmes_.
- `cumtime` (Cumulative Time) : **Temps cumulé passé dans la fonction ET dans toutes les sous-fonctions qu'elle appelle**. Utile pour identifier les points de départ des opérations coûteuses.
- `ncalls` (Number of Calls) : Nombre de fois où la fonction a été appelée. Une fonction rapide appelée des millions de fois peut devenir un goulot d'étranglement.
- `pcalls` (Primitive Calls) : Nombre d'appels non récursifs.

**Workflow typique d'analyse :**

1.  Lancer `pstats stats.prof`.
2.  Taper `sort tottime`.
3.  Taper `stats 10` pour voir les 10 fonctions où le plus de temps a été passé.
4.  Analyser la première ligne. Si c'est une fonction de votre code, vous avez trouvé un candidat à l'optimisation.
5.  Taper `sort cumtime`.
6.  Taper `stats 10` pour voir les 10 fonctions qui sont à l'origine des chaînes d'appels les plus longues.

### Exemple d'Analyse

Imaginons le script `test.py` suivant :

```python
# test.py
def fonction_rapide():
    pass

def fonction_lente():
    total = 0
    for _ in range(100000):
        total += 1

def fonction_intermediaire():
    for _ in range(10):
        fonction_lente()

def main():
    for _ in range(100):
        fonction_rapide()
    fonction_intermediaire()

main()
```

Après avoir lancé `python -m cProfile -o test.prof test.py` et `python -m pstats test.prof` :

```
(test.prof) sort tottime
(test.prof) stats 5
```

La sortie ressemblerait à ceci :

```
   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
     1000    0.045    0.000    0.045    0.000 test.py:5(fonction_lente)
        1    0.001    0.001    0.046    0.046 test.py:9(fonction_intermediaire)
      ...
```

- **Analyse de `tottime`** : On voit immédiatement que `fonction_lente` est la fonction où le programme passe le plus de temps intrinsèquement (0.045s). C'est notre cible n°1 pour l'optimisation.

```
(test.prof) sort cumtime
(test.prof) stats 5
```

La sortie ressemblerait à ceci :

```
   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.046    0.046 test.py:17(<module>)
        1    0.000    0.000    0.046    0.046 test.py:13(main)
        1    0.001    0.001    0.046    0.046 test.py:9(fonction_intermediaire)
     1000    0.045    0.000    0.045    0.000 test.py:5(fonction_lente)
      ...
```

- **Analyse de `cumtime`** : On voit que `main`, `fonction_intermediaire` et le module lui-même ont un temps cumulé élevé. Cela nous montre la chaîne d'appels qui mène à la fonction lente. `main` appelle `fonction_intermediaire`, qui appelle `fonction_lente`.

## 4. Visualiser les Résultats

Lire les sorties textuelles de `pstats` est puissant mais peut être austère. Il existe des outils pour visualiser ces données de manière graphique.

- **SnakeViz** : Un outil très populaire qui crée une visualisation interactive et solaire (sunburst) des données de profilage.

  1.  `pip install snakeviz`
  2.  `snakeviz stats.prof` (ou `python -m snakeviz stats.prof`)
      Cela ouvre une page web dans votre navigateur où vous pouvez explorer les données de manière intuitive.

- **gprof2dot** et **Graphviz** : Ces outils permettent de générer un graphe d'appel où les nœuds sont colorés en fonction du temps passé.
  1.  `pip install gprof2dot`
  2.  Installer Graphviz (via `brew`, `apt`, etc.)
  3.  `python -m cProfile -o stats.prof mon_script.py`
  4.  `gprof2dot -f pstats stats.prof | dot -Tpng -o output.png`

## Conclusion

Le profilage est une étape non négociable de toute optimisation sérieuse. `cProfile` est l'outil standard de la bibliothèque Python pour mesurer les performances temporelles de votre code. En l'utilisant conjointement avec `pstats` ou des outils de visualisation comme SnakeViz, vous pouvez obtenir des informations précises sur les parties de votre code qui nécessitent une attention particulière. N'oubliez jamais : **Mesurez avant d'optimiser**.
