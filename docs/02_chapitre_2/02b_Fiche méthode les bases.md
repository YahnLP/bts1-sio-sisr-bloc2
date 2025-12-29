---
author: ELP
title: 02b Fiche Méthode Les bases
---


???+ question "1. Les conditions"

    Les conditions permettent d'exécuter des blocs de code différents en fonction de certaines situations.

    ???+ question "a) `if`"

        La condition `if` permet d'exécuter un bloc de code seulement si la condition est vraie.

        **Syntaxe :**
        ```python
        if condition:
            # Code à exécuter si la condition est vraie
        ```

        ??? success "Exemple"

            ```python
            age = 18
            if age >= 18:
                print("Vous êtes majeur.")  # Affiche "Vous êtes majeur."
            ```
            {{ IDE() }}

        ??? success "Exemple avec fonction"
            ```python
            def verifier_majorite(age):
                if age >= 18:
                    print("Vous êtes majeur.")  # Affiche "Vous êtes majeur."

            verifier_majorite(20)
            ```
            {{ IDE() }}
       
            

    ???+ question "b) `if / else`"

        La condition `if / else` permet de choisir entre deux blocs de code : l'un si la condition est vraie, l'autre sinon.

        **Syntaxe :**
        ```python
        if condition:
            # Code si la condition est vraie
        else:
            # Code si la condition est fausse
        ```

        ??? success "Exemple"

            ```python
            age = 16
            if age >= 18:
                print("Vous êtes majeur.")
            else:
                print("Vous êtes mineur.")  
            ```

            {{ IDE() }}

        ??? success "Exemple avec fonction"

            ```python
            def verifier_age(age):
                if age >= 18:
                    return "Vous êtes majeur."
                else:
                    return "Vous êtes mineur."  

            print(verifier_age(17))
            ```

            {{ IDE() }}

    ???+ question "c) `if / elif`"

        `elif` (abréviation de "else if") permet de tester plusieurs conditions différentes.

        **Syntaxe :**
        ```python
        if condition1:
            # Code si condition1 est vraie
        elif condition2:
            # Code si condition2 est vraie
        ```

        ??? success "Exemple"

            ```python
            note = 15
            if note >= 18:
                print("Excellent")
            elif note >= 12:
                print("Bien")  
            ```

            {{ IDE() }}

        ??? success "Exemple avec fonction"

            ```python
            def evaluer_note(note):
                if note >= 18:
                    return "Excellent"
                elif note >= 12:
                    return "Bien"

            print(evaluer_note(14))
            ```

            {{ IDE() }}

    ???+ question "d) `if / elif / else`"

        Permet de gérer plusieurs cas avec une condition finale pour tout ce qui ne correspond à aucune des conditions précédentes.

        **Syntaxe :**
        ```python
        if condition1:
            # Code si condition1 est vraie
        elif condition2:
            # Code si condition2 est vraie
        else:
            # Code si aucune condition n'est vraie
        ```

        ??? success "Exemple"

            ```python
            note = 8
            if note >= 18:
                print("Excellent")
            elif note >= 12:
                print("Bien")
            else:
                print("Peut mieux faire")  # Affiche "Peut mieux faire"
            ```

        ??? success "Exemple avec fonction"

            ```python
            def evaluer_performance(note):
                if note >= 18:
                    return "Excellent"
                elif note >= 12:
                    return "Bien"
                else:
                    return "Peut mieux faire"

            print(evaluer_performance(9))
            ```

            {{ IDE() }}



???+ question "2. Les chaînes de caractères"

    Les chaînes de caractères sont des séries de caractères entourées de guillemets (simples ou doubles).

    ???+ question "a) Accès à un caractère avec `chaine[nombre]`"

        L'indexation commence à 0.

        ??? success "Exemple"

            ```python
            chaine = "Python"
            print(chaine[0])  # Affiche 'P'
            print(chaine[3])  # Affiche 'h'
            ```
            {{ IDE() }}

        ??? success "Exemple avec fonction"

            ```python
            def afficher_premier_caractere(chaine):
                return chaine[0]  # Affiche le premier caractère de la chaîne

            print(afficher_premier_caractere("Bonjour"))  # Affiche 'B'
            ```
            {{ IDE() }}

    ???+ question "b) Accès avec indexation négative `chaine[-nombre]`"

        Les index négatifs permettent de partir de la fin de la chaîne.

        ??? success "Exemple"

            ```python
            chaine = "Python"
            print(chaine[-1])  # Affiche 'n'
            print(chaine[-3])  # Affiche 'h'
            ```
            {{ IDE() }}

        ??? success "Exemple avec fonction"

            ```python
            def afficher_dernier_caractere(chaine):
                return chaine[-1]  # Affiche le dernier caractère de la chaîne

            print(afficher_dernier_caractere("Bonjour"))  # Affiche 'r'
            ```
            {{ IDE() }}

    ???+ question "c) Tranches (slices) `chaine[début:fin]`"

        Permet d'extraire une partie de la chaîne. Le caractère à l'index `fin` n'est **pas** inclus.

        ??? success "Exemple"

            ```python
            chaine = "Python"
            print(chaine[0:3])  # Affiche 'Pyt'
            print(chaine[2:5])  # Affiche 'tho'
            ```
            {{ IDE() }}

        ??? success "Exemple avec fonction"

            ```python
            def extraire_tranche(chaine, debut, fin):
                return chaine[debut:fin]  # Affiche la tranche de la chaîne

            print(extraire_tranche("Programmation", 0, 6))  # Affiche 'Progra'
            ```
            {{ IDE() }}

    ???+ question "d) Tranches avec pas `chaine[début:fin:pas]`"

        Permet de sauter des caractères selon un pas.

        ??? success "Exemple"

            ```python
            chaine = "Python"
            print(chaine[0:6:2])  # Affiche 'Pto'
            print(chaine[::-1])    # Affiche 'nohtyP' (chaîne inversée)
            ```
            {{ IDE() }}

        ??? success "Exemple avec fonction"

            ```python
            def inverser_chaine(chaine):
                return chaine[::-1]  # Affiche la chaîne inversée

            print(inverser_chaine("Python"))  # Affiche 'nohtyP'
            ```
            {{ IDE() }}




???+ question "3. Boucles `for`"

    Les boucles `for` permettent de répéter des instructions pour chaque élément d'une séquence.

    ???+ question "a) `for elmt in chaine_de_caractere`"

        Itère sur chaque caractère de la chaîne.

        ??? success "Exemple"

            ```python
            chaine = "Python"
            for elmt in chaine:
                print(elmt)  # Affiche chaque lettre sur une nouvelle ligne
            ```
            *Affiche : P, y, t, h, o, n.*

            {{ IDE() }}

        ??? success "Exemple avec fonction"

            ```python
            def afficher_caracteres(chaine):
                for elmt in chaine:
                    print(elmt)  # Affiche chaque caractère de la chaîne

            print(afficher_caracteres("Bonjour"))  # Affiche B, o, n, j, o, u, r
            ```
            {{ IDE() }}

    ???+ question "b) `for i in range(len(chaine_de_caractere))`"

        Utilise les index pour accéder aux caractères.

        ??? success "Exemple"

            ```python
            for i in range(len(chaine)):
                print("Caractère à l'index", i, ":", chaine[i])
            ```
            *Affiche :*
            ```
            Caractère à l'index 0 : P
            Caractère à l'index 1 : y
            Caractère à l'index 2 : t
            Caractère à l'index 3 : h
            Caractère à l'index 4 : o
            Caractère à l'index 5 : n
            ```
            {{ IDE() }}

        ??? success "Exemple avec fonction"

            ```python
            def afficher_avec_index(chaine):
                for i in range(len(chaine)):
                    print("Index", i, ":", chaine[i])  # Affiche l'index et le caractère correspondant

            afficher_avec_index("Python")  # Affiche les caractères avec leurs index
            ```
            {{ IDE() }}



???+ question "4. Boucle `while`"

    La boucle `while` répète des instructions tant qu'une condition est vraie.

    **Syntaxe :**
    ```python
    while condition:
        # Code à répéter tant que la condition est vraie
    ```

    ??? success "Exemple"

        ```python
        compteur = 0
        while compteur < 5:
            print("Compteur :", compteur)  # Affiche la valeur du compteur
            compteur += 1
        ```
        *Affiche :*
        ```
        Compteur : 0
        Compteur : 1
        Compteur : 2
        Compteur : 3
        Compteur : 4
        ```
        {{ IDE() }}

    ??? success "Exemple avec fonction"

        ```python
        def compter_jusqu_a(n):
            compteur = 0
            while compteur < n:
                print("Compteur :", compteur)  # Affiche la valeur du compteur
                compteur += 1

        compter_jusqu_a(3)  # Affiche 0, 1, 2
        ```
        {{ IDE() }}



