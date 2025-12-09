# Exercice 01 : Générateur de Balises HTML

## Objectif

Cet exercice a pour but de vous faire utiliser `*args` et `**kwargs` pour créer une fonction flexible capable de générer des balises HTML.

## Contexte

En HTML, une balise est composée d'un nom (ex: `p`, `div`, `a`) et peut avoir de multiples attributs (ex: `class="main"`, `id="intro"`, `href="/"`). Vous allez créer une fonction qui prend le nom de la balise, son contenu, et un nombre variable d'attributs pour générer la chaîne de caractères HTML correspondante.

## Énoncé

1.  **Créez un nouveau fichier Python** nommé `html_tag_generator.py`.

2.  **Définissez une fonction** nommée `create_html_tag` qui accepte les paramètres suivants dans cet ordre :

    - `tag_name` : Le nom de la balise (ex: "p").
    - `content` : Le contenu textuel de la balise.
    - `**attributes` : Un dictionnaire pour capturer tous les attributs HTML nommés.

3.  **À l'intérieur de la fonction** :
    a. Commencez par construire la partie ouvrante de la balise. Par exemple, si `tag_name` est "p", la base est `<p`.

    b. Itérez sur les paires clé-valeur du dictionnaire `attributes` (`**kwargs`). Pour chaque attribut, ajoutez ` nom_attribut="valeur_attribut"` à la chaîne de la balise. - _Exemple :_ Si `attributes` est `{'class': 'text-bold', 'id': 'p1'}`, vous devez ajouter ` class="text-bold" id="p1"`. - N'oubliez pas l'espace avant chaque attribut.

    c. Fermez la balise ouvrante avec `>`.

    d. Ajoutez le `content`.

    e. Ajoutez la balise fermante, qui est `</` + `tag_name` + `>`.

    f. **Retournez** la chaîne de caractères HTML complète.

4.  **Testez votre fonction** avec les appels suivants et affichez les résultats :
    - Une balise `p` simple : `create_html_tag("p", "This is a paragraph.")`
    - Une balise `a` avec des attributs : `create_html_tag("a", "Click here", href="/home", target="_blank")`
    - Une balise `div` avec une classe et un id : `create_html_tag("div", "This is a section.", id="main-section", class_name="container")`
      - _Note :_ Comme `class` est un mot-clé réservé en Python, on utilise souvent `class_name` comme nom de paramètre et on le traite spécialement. Pour cet exercice, vous pouvez générer `class_name="..."` ou, en bonus, le convertir en `class="..."`.

## Résultat Attendu

```
<p>This is a paragraph.</p>
<a href="/home" target="_blank">Click here</a>
<div id="main-section" class_name="container">This is a section.</div>
```

## Bonus

Modifiez votre fonction pour qu'elle gère correctement l'attribut `class`. Si la fonction reçoit un argument nommé `class_name`, elle doit le transformer en un attribut `class` dans la balise HTML.

### Résultat Attendu (avec bonus)

```
<p>This is a paragraph.</p>
<a href="/home" target="_blank">Click here</a>
<div id="main-section" class="container">This is a section.</div>
```

<details>
<summary>Cliquez ici pour voir un exemple de code de solution</summary>

```python
# html_tag_generator.py

def create_html_tag(tag_name, content, **attributes):
    """
    Generates an HTML tag with content and dynamic attributes.

    Args:
        tag_name (str): The name of the HTML tag (e.g., 'p', 'div').
        content (str): The text content inside the tag.
        **attributes: Keyword arguments representing HTML attributes.
                      Use 'class_name' for the 'class' attribute.

    Returns:
        str: The complete HTML tag as a string.
    """
    # Commence la balise ouvrante
    tag_parts = [f"<{tag_name}"]

    # Gère le cas spécial de 'class' (Bonus)
    if 'class_name' in attributes:
        attributes['class'] = attributes.pop('class_name')

    # Ajoute les attributs
    for key, value in attributes.items():
        tag_parts.append(f' {key}="{value}"')

    # Termine la balise
    tag_parts.append(f">{content}</{tag_name}>")

    # Joint toutes les parties en une seule chaîne
    return "".join(tag_parts)

# --- Tests ---

# Test 1: Balise p simple
p_tag = create_html_tag("p", "This is a paragraph.")
print(p_tag)

# Test 2: Balise a avec attributs
a_tag = create_html_tag("a", "Click here", href="/home", target="_blank")
print(a_tag)

# Test 3: Balise div avec class_name (pour le bonus)
div_tag = create_html_tag("div", "This is a section.", id="main-section", class_name="container")
print(div_tag)
```

</details>
