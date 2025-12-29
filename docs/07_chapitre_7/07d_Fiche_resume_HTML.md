---
author: ELP
title: Fiche Résumée des Balises HTML
---

???+ question "1. Structure de base d'une page HTML"

    Une page HTML suit une structure standard avec des balises essentielles.

    ???+ question "a) Structure minimale d'une page HTML"

        **Syntaxe :**
        ```html
        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="utf-8">
            <title>Ma page HTML</title>
        </head>
        <body>
            <p>Bienvenue sur ma page !</p>
        </body>
        </html>
        ```

    ???+ question "b) Balises principales"

        | Balise | Description |
        |--------|-------------|
        | `<!DOCTYPE html>` | Indique l'utilisation de HTML5 |
        | `<html>` | Conteneur principal de la page |
        | `<head>` | Contient les métadonnées |
        | `<title>` | Titre affiché dans l'onglet du navigateur |
        | `<meta charset="utf-8">` | Encodage des caractères |
        | `<body>` | Contenu principal affiché sur la page |


???+ question "2. Les Titres et Paragraphes"

    ???+ question "a) Titres HTML"

        Il existe **6 niveaux de titres**, du plus important (`<h1>`) au moins important (`<h6>`).

        **Exemple :**
        ```html
        <h1>Titre principal</h1>
        <h2>Sous-titre</h2>
        <h3>Sous-sous-titre</h3>
        ```

    ???+ question "b) Paragraphes et sauts de ligne"

        - **Paragraphe** : `<p>`
        - **Retour à la ligne** : `<br>` (balise orpheline, pas de balise fermante)
        
        **Exemple :**
        ```html
        <p>Ceci est un paragraphe.</p>
        <p>Voici une phrase.<br>Voici une nouvelle ligne.</p>
        ```


???+ question "3. Mise en valeur du texte"

    ???+ question "a) Balises de mise en forme"

        | Balise | Effet |
        |--------|------|
        | `<strong>` | Texte en gras |
        | `<em>` | Texte en italique |
        | `<mark>` | Texte surligné |
        
        **Exemple :**
        ```html
        <p><strong>Important :</strong> Ne pas oublier !</p>
        <p><em>Ce texte est en italique.</em></p>
        <p><mark>Texte surligné</mark></p>
        ```


???+ question "4. Listes en HTML"

    ???+ question "a) Listes non ordonnées (puces)"

        **Exemple :**
        ```html
        <ul>
            <li>Pommes</li>
            <li>Bananes</li>
            <li>Cerises</li>
        </ul>
        ```

    ???+ question "b) Listes ordonnées (numérotées)"

        **Exemple :**
        ```html
        <ol>
            <li>Préparer les ingrédients</li>
            <li>Mélanger la pâte</li>
            <li>Cuire au four</li>
        </ol>
        ```


???+ question "5. Liens hypertexte"

    ???+ question "a) Lien vers un autre site (absolu)"

        **Exemple :**
        ```html
        <a href="https://example.com">Visitez ce site</a>
        ```
    
    ???+ question "b) Lien vers une page locale (relatif)"

        **Exemple :**
        ```html
        <a href="page2.html">Aller à la page 2</a>
        ```
    
    ???+ question "c) Lien vers une ancre sur la même page"

        **Exemple :**
        ```html
        <h2 id="section1">Section 1</h2>
        <a href="#section1">Aller à la section 1</a>
        ```
    
    ???+ question "d) Lien ouvrant une nouvelle fenêtre"

        **Exemple :**
        ```html
        <a href="https://example.com" target="_blank">Ouvrir dans un nouvel onglet</a>
        ```


???+ question "6. Les images"

    ???+ question "a) Insérer une image"

        **Exemple :**
        ```html
        <img src="image.jpg" alt="Description de l’image">
        ```

    ???+ question "b) Image cliquable"

        **Exemple :**
        ```html
        <a href="image_grande.jpg">
            <img src="image_mini.jpg" alt="Cliquez pour agrandir">
        </a>
        ```


???+ question "7. Tableaux"

    ???+ question "a) Structure d’un tableau"

        **Exemple :**
        ```html
        <table>
            <tr>
                <th>Nom</th>
                <th>Age</th>
            </tr>
            <tr>
                <td>Alice</td>
                <td>25</td>
            </tr>
            <tr>
                <td>Bob</td>
                <td>30</td>
            </tr>
        </table>
        ```


