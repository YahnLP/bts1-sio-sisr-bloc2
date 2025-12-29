---
author: ELP
title: Fiche Résumée du JavaScript
---

???+ question "1. Introduction au JavaScript"

    ???+ question "a) Qu'est-ce que JavaScript ?"

        JavaScript est un **langage de programmation** utilisé pour rendre les pages web interactives.

        📌 **Caractéristiques principales :**

        - **Langage interprété** (exécuté par le navigateur)

        - **Interaction avec HTML et CSS**
        
        - **Gestion des événements** (clics, formulaires, animations, etc.)

    ???+ question "b) Où écrire le JavaScript ?"

        Il existe trois manières d’intégrer JavaScript à une page HTML :
        
        | **Méthode** | **Description** |
        |------------|-----------------|
        | **Interne (`<script>`)** | Code inclus directement dans le fichier HTML |
        | **Externe (`.js`)** | Fichier séparé, inclus avec `<script src="script.js">` |
        | **En ligne (`onclick="..."`)** | Directement dans une balise HTML (peu recommandé) |

        📌 **Exemple d’inclusion externe :**
        ```html
        <script src="script.js"></script>
        ```

???+ question "2. Variables et Types de données"

    ???+ question "a) Déclaration des variables"

        | **Mot-clé** | **Description** |
        |------------|-----------------|
        | `var` | Ancienne méthode, portée globale ou fonction |
        | `let` | Portée limitée au bloc où elle est définie |
        | `const` | Variable **constante** qui ne peut pas être modifiée |

        📌 **Exemple :**
        ```js
        let nom = "Alice";
        const age = 25;
        ```

    ???+ question "b) Types de données"

        | **Type** | **Exemple** |
        |---------|------------|
        | `Number` | `let x = 10;` |
        | `String` | `let txt = "Bonjour";` |
        | `Boolean` | `let estVrai = true;` |
        | `Array` | `let fruits = ["Pomme", "Banane"];` |
        | `Object` | `let personne = {nom: "Alice", age: 25};` |

???+ question "3. Opérateurs et Conditions"

    ???+ question "a) Opérateurs arithmétiques"

        📌 **Principaux opérateurs :**
        ```js
        let a = 10, b = 5;
        console.log(a + b); // Addition
        console.log(a - b); // Soustraction
        console.log(a * b); // Multiplication
        console.log(a / b); // Division
        console.log(a % b); // Modulo
        ```

    ???+ question "b) Conditions (`if` / `else` / `switch`)

        📌 **Exemple avec `if` :**
        ```js
        let age = 18;
        if (age >= 18) {
            console.log("Majeur");
        } else {
            console.log("Mineur");
        }
        ```
        📌 **Exemple avec `switch` :**
        ```js
        let fruit = "Banane";
        switch (fruit) {
            case "Pomme":
                console.log("C'est une pomme");
                break;
            case "Banane":
                console.log("C'est une banane");
                break;
            default:
                console.log("Fruit inconnu");
        }
        ```

???+ question "4. Boucles"

    ???+ question "a) `for`"

        📌 **Exemple :**
        ```js
        for (let i = 0; i < 5; i++) {
            console.log("Itération n°" + i);
        }
        ```
    
    ???+ question "b) `while` et `do while`"

        📌 **Exemple `while` :**
        ```js
        let x = 0;
        while (x < 3) {
            console.log(x);
            x++;
        }
        ```
        📌 **Exemple `do while` :**
        ```js
        let y = 0;
        do {
            console.log(y);
            y++;
        } while (y < 3);
        ```

???+ question "5. Fonctions"

    ???+ question "a) Déclaration d'une fonction"

        📌 **Exemple de fonction classique :**
        ```js
        function saluer(nom) {
            return "Bonjour, " + nom;
        }
        console.log(saluer("Alice"));
        ```

    ???+ question "b) Fonctions fléchées (ES6)"

        📌 **Exemple :**
        ```js
        const saluer = (nom) => "Bonjour, " + nom;
        console.log(saluer("Bob"));
        ```

???+ question "6. Manipulation du DOM"

    ???+ question "a) Sélection d'éléments HTML"

        | **Méthode** | **Description** |
        |------------|-----------------|
        | `getElementById("id")` | Sélectionne un élément par son ID |
        | `getElementsByClassName("classe")` | Sélectionne une collection d'éléments |
        | `querySelector("selector")` | Sélectionne le premier élément correspondant |

        📌 **Exemple :**
        ```js
        document.getElementById("titre").textContent = "Nouveau titre";
        ```

    ???+ question "b) Modification du style"

        📌 **Exemple :**
        ```js
        document.getElementById("monDiv").style.color = "red";
        ```

???+ question "7. Gestion des événements"

    ???+ question "a) Ajouter un événement"

        📌 **Exemple :**
        ```js
        document.getElementById("monBouton").addEventListener("click", function() {
            alert("Bouton cliqué !");
        });
        ```
