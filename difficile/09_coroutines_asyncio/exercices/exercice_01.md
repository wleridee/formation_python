# Exercice 01 : Simulation de Téléchargements Concurrents

## Objectif

Cet exercice a pour but de vous faire utiliser `asyncio` et `asyncio.gather` pour simuler le téléchargement de plusieurs fichiers de manière concurrente. Vous pourrez ainsi constater le gain de temps par rapport à une approche synchrone.

## Contexte

Imaginez que vous devez télécharger une liste de fichiers depuis des URLs. Chaque téléchargement prend un temps variable. Si vous les téléchargez les uns après les autres (de manière synchrone), le temps total sera la somme de tous les temps de téléchargement.

Avec `asyncio`, vous pouvez lancer tous les téléchargements "en même temps". Le temps total sera alors approximativement celui du téléchargement le plus long.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `concurrent_downloads.py`.

2.  **Définissez une coroutine `download_file`**.

    - Elle doit être définie avec `async def`.
    - Elle prend un argument `file_name` (une chaîne de caractères).
    - Elle simule un temps de téléchargement variable. Pour cela :
      - Générez un temps de pause aléatoire entre 0.5 et 3.0 secondes en utilisant `random.uniform(0.5, 3.0)`.
      - Affichez un message indiquant le début du téléchargement, par exemple : `f"Downloading {file_name}..."`.
      - Utilisez `await asyncio.sleep()` avec le temps de pause calculé pour simuler l'opération I/O.
      - Une fois la pause terminée, affichez un message de fin, par exemple : `f"✓ Finished downloading {file_name} in {delay:.2f}s"`.
      - La coroutine doit retourner le nom du fichier téléchargé.

3.  **Définissez une coroutine principale `main`**.

    - Elle doit être définie avec `async def`.
    - Créez une liste de noms de fichiers à "télécharger", par exemple : `["file1.zip", "file2.img", "file3.iso", "file4.pdf"]`.
    - Enregistrez le temps de début avec `time.time()`.
    - Créez une liste de tâches (coroutines) en appelant `download_file` pour chaque nom de fichier dans votre liste.
    - Utilisez `await asyncio.gather(*tasks)` pour exécuter toutes les tâches de manière concurrente.
    - Enregistrez le temps de fin.
    - Affichez le temps total d'exécution.

4.  **Lancez l'exécution**.
    - À la fin de votre script, utilisez `asyncio.run(main())` pour démarrer le programme.
    - N'oubliez pas d'importer les modules nécessaires : `asyncio`, `random`, et `time`.

## Résultat Attendu

L'ordre des messages de début et de fin peut varier, mais le schéma général devrait être le suivant : tous les téléchargements commencent presque en même temps, puis se terminent en fonction de leur délai aléatoire. Le temps total doit être proche du délai le plus long (environ 3 secondes), et non de la somme de tous les délais.

```
Downloading file1.zip...
Downloading file2.img...
Downloading file3.iso...
Downloading file4.pdf...
✓ Finished downloading fileX.xxx in 0.XXs
✓ Finished downloading fileY.yyy in 1.YYs
✓ Finished downloading fileZ.zzz in 2.ZZs
✓ Finished downloading fileW.www in 2.WWs
---
All files downloaded concurrently.
Total time: 2.98 seconds.
Downloaded files: ['file1.zip', 'file2.img', 'file3.iso', 'file4.pdf']
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# concurrent_downloads.py

import asyncio
import random
import time

async def download_file(file_name: str) -> str:
    """Coroutine to simulate a file download with a random delay."""
    delay = random.uniform(0.5, 3.0)
    print(f"Downloading {file_name}...")
    await asyncio.sleep(delay)
    print(f"✓ Finished downloading {file_name} in {delay:.2f}s")
    return file_name

async def main():
    """Main coroutine to run concurrent downloads."""
    files_to_download = [
        "file1.zip",
        "file2.img",
        "file3.iso",
        "file4.pdf"
    ]

    start_time = time.time()

    # Create a list of coroutine objects (tasks)
    tasks = [download_file(file) for file in files_to_download]

    # Run all tasks concurrently and wait for them to complete
    downloaded_files = await asyncio.gather(*tasks)

    end_time = time.time()

    print("---")
    print("All files downloaded concurrently.")
    print(f"Total time: {end_time - start_time:.2f} seconds.")
    print(f"Downloaded files: {downloaded_files}")

if __name__ == "__main__":
    asyncio.run(main())

```

</details>
