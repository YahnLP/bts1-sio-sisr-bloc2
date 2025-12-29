---
author: ELP
title: Fiche Résumée du CSS
---

???+ question "1. Introduction au CSS"

    ???+ question "a) Qu'est-ce que le CSS ?"

        CSS (*Cascading Style Sheets*) est un langage qui permet de **mettre en forme** une page web.

        📌 **Le CSS permet de modifier :**

        - La **couleur** du texte et du fond.

        - La **taille et la police** des caractères.

        - L’**agencement des éléments**.

        - Les **marges, bordures et animations**.

        💡 **Pourquoi utiliser le CSS ?**

        - Il **sépare** le contenu (HTML) et la mise en forme.

        - Un fichier CSS peut modifier **plusieurs pages HTML en même temps**.
        
        - Il facilite la maintenance et l’évolution d’un site web.

???+ question "2. Où écrire le CSS ?"

    ???+ question "a) Trois méthodes d’intégration du CSS"

        | **Méthode** | **Description** | **Bonnes pratiques** |
        |------------|-----------------|---------------------|
        | **Externe (`.css`)** | Le CSS est stocké dans un fichier séparé et lié au HTML. | ✅ **Recommandé** |
        | **Interne (`<style>`)** | Le CSS est écrit directement dans le fichier HTML, dans `<head>`. | ⚠ À éviter sauf dépannage |
        | **En ligne (`style=""`)** | Le CSS est ajouté directement à l’intérieur des balises HTML. | ❌ Mauvaise pratique |

        **Exemple d’un fichier externe (`style.css`) :**
        ```css
        p {
            color: blue;
            font-size: 16px;
        }
        ```
        **Lier le fichier CSS dans le HTML (`index.html`) :**
        ```html
        <link rel="stylesheet" href="style.css">
        ```

???+ question "3. Sélecteurs et règles CSS"

    ???+ question "a) Structure d’une règle CSS"

        Une règle CSS suit la structure suivante :
        ```css
        sélecteur {
            propriété: valeur;
        }
        ```
        📌 **Exemple :**
        ```css
        h1 {
            color: red;
            text-align: center;
        }
        ```

    ???+ question "b) Les principaux sélecteurs"

        | **Sélecteur** | **Description** | **Exemple** |
        |-------------|---------------|-------------|
        | `*` | Sélectionne tous les éléments | `* { margin: 0; }` |
        | `p` | Sélectionne tous les `<p>` | `p { color: green; }` |
        | `.classe` | Sélectionne les éléments avec une classe spécifique | `.important { font-weight: bold; }` |
        | `#id` | Sélectionne un élément unique par son ID | `#titre { text-align: center; }` |

???+ question "4. Mise en forme du texte"

    ???+ question "a) Modifier la taille et la police"

        - **Taille (`font-size`)** : `px`, `%`, `em`, `rem`
        - **Police (`font-family`)** : `Arial`, `Verdana`, `sans-serif`

        📌 **Exemple :**
        ```css
        p {
            font-size: 14px;
            font-family: "Arial", sans-serif;
        }
        ```

    ???+ question "b) Modifier l’alignement et le style du texte"

        - **Alignement (`text-align`)** : `left`, `center`, `right`, `justify`
        - **Style (`font-style`)** : `normal`, `italic`
        - **Gras (`font-weight`)** : `normal`, `bold`
        - **Souligné (`text-decoration`)** : `none`, `underline`

        📌 **Exemple :**
        ```css
        h1 {
            text-align: center;
            font-style: italic;
            font-weight: bold;
            text-decoration: underline;
        }
        ```

???+ question "5. Couleurs et fonds"

    ???+ question "a) Définir une couleur de texte et d’arrière-plan"

        📌 **Trois méthodes pour définir une couleur :**
        ```css
        p { color: red; } /* Nom de couleur */
        p { color: #ff0000; } /* Hexadécimal */
        p { color: rgb(255, 0, 0); } /* RGB */
        ```

    ???+ question "b) Ajouter une image d’arrière-plan"

        📌 **Exemple :**
        ```css
        body {
            background-image: url("fond.jpg");
            background-size: cover;
        }
        ```

???+ question "6. Bordures et ombres"

    ???+ question "a) Ajouter une bordure"

        📌 **Syntaxe :**
        ```css
        div {
            border: 2px solid black;
        }
        ```

    ???+ question "b) Ajouter des ombres"

        - **Sur un texte (`text-shadow`)**
        - **Sur un bloc (`box-shadow`)**
        
        📌 **Exemple :**
        ```css
        h1 {
            text-shadow: 3px 3px 5px gray;
        }
        ```

???+ question "7. Le modèle des boîtes"

    ???+ question "a) Les quatre parties d’un bloc"

        | **Propriété** | **Rôle** |
        |-------------|--------|
        | `width` et `height` | Définissent la taille de l’élément |
        | `padding` | Espacement **interne** entre le contenu et la bordure |
        | `border` | Bordure de l’élément |
        | `margin` | Espacement **externe** autour de l’élément |

        📌 **Exemple :**
        ```css
        div {
            width: 200px;
            padding: 10px;
            border: 2px solid black;
            margin: 20px;
        }
        ```

???+ question "8. Positionnement et affichage"

    ???+ question "a) Les types de positionnement"

        | **Valeur** | **Description** |
        |---------|-------------|
        | `static` | Position par défaut |
        | `relative` | Décalage par rapport à sa position normale |
        | `absolute` | Position par rapport à son parent |
        | `fixed` | Position fixe même au défilement |

        📌 **Exemple :**
        ```css
        div {
            position: absolute;
            top: 50px;
            left: 100px;
        }
        ```

