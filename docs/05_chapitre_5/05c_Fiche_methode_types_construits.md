---
author: ELP
title: 05c Fiche méthode Types construits
---


## <span style="color:blue;">Les Listes, Tuples et dictionnaires</span>

???+ question "1. Qu'est-ce que c'est ?"

    En **Python**, les *listes*, *tuples* et *dictionnaires* sont des structures de données permettant de stocker des collections d'éléments.

    - Une **liste** est une collection **modifiable** d'éléments.
    - Un **tuple** est une collection **non modifiable** (immuable).
    - Un **dictionnaire** est une collection **modifiable** de paires *clé-valeur*.

    **Syntaxe :**

    ```python
    # Liste
    ma_liste = [1, 2, 3, 4, 5]

    # Tuple
    mon_tuple = (1, 2, 3, 4, 5)

    # Dictionnaire
    mon_dictionnaire = {"nom": "Alice", "age": 25}
    ```

    ??? success "Exemple"

        ```python
        fruits = ["pomme", "banane", "cerise"]
        couleurs = ("rouge", "vert", "bleu")
        personne = {"nom": "Alice", "age": 25}
        print(fruits)
        print(couleurs)
        print(personne)
        ```
        *Affiche :*
        ```
        ['pomme', 'banane', 'cerise']
        ('rouge', 'vert', 'bleu')
        {'nom': 'Alice', 'age': 25}
        ```
        {{ IDE() }}

## <span style="color:blue;">Accéder aux éléments d'une liste/tuple</span>

???+ question "2. Accéder aux éléments avec `liste[nombre]`"

    Vous pouvez accéder à un élément en utilisant son **indice**. En Python, l'indexation commence à 0.

    **Syntaxe :**
    ```python
    element = liste[indice]
    ```

    ??? success "Exemple"

        ```python
        nombres = [10, 20, 30, 40, 50]
        print(nombres[0])  # Premier élément
        print(nombres[3])  # Quatrième élément
        ```
        *Affiche :*
        ```
        10
        40
        ```
        {{ IDE() }}


???+ question "3. Accéder aux éléments avec `liste[-nombre]`"

    L'utilisation d'un indice **négatif** permet d'accéder aux éléments à partir de la fin de la liste.

    **Syntaxe :**
    ```python
    element = liste[-indice]
    ```

    ??? success "Exemple"

        ```python
        nombres = [10, 20, 30, 40, 50]
        print(nombres[-1])  # Dernier élément
        print(nombres[-3])  # Troisième élément à partir de la fin
        ```
        *Affiche :*
        ```
        50
        30
        ```
        {{ IDE() }}


???+ question "4. Accéder à des sous-parties avec un slicing"

    Le **slicing** permet d'extraire une sous-liste en spécifiant un intervalle d'indices.

    **Syntaxe :**
    ```python
    sous_liste = liste[debut:fin]
    ```
    L'élément à l'indice **debut** est inclus, mais celui à l'indice **fin** est exclu.

    ??? success "Exemple"

        ```python
        nombres = [10, 20, 30, 40, 50]
        print(nombres[1:4])  # Éléments des indices 1 à 3
        print(nombres[:3])   # Du début à l'indice 2
        print(nombres[2:])   # De l'indice 2 à la fin
        ```
        *Affiche :*
        ```
        [20, 30, 40]
        [10, 20, 30]
        [30, 40, 50]
        ```
        {{ IDE() }}


???+ question "5. Utiliser un pas dans le slicing avec `liste[debut:fin:pas]`"

    Vous pouvez spécifier un **pas** pour sauter des éléments dans le slicing.

    **Syntaxe :**
    ```python
    sous_liste = liste[debut:fin:pas]
    ```

    ??? success "Exemple"

        ```python
        nombres = [10, 20, 30, 40, 50, 60, 70]
        print(nombres[::2])  # Tous les 2 éléments
        print(nombres[1:6:2])  # De l'indice 1 à 5 avec un pas de 2
        print(nombres[::-1])  # Liste inversée
        ```
        *Affiche :*
        ```
        [10, 30, 50, 70]
        [20, 40, 60]
        [70, 60, 50, 40, 30, 20, 10]
        ```
        {{ IDE() }}


???+ question "6. Accéder aux éléments de listes de listes ou tuples de tuples"

    Vous pouvez accéder aux éléments imbriqués en utilisant plusieurs indices.

    ??? success "Exemple"

        ```python
        liste_de_listes = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
        print(liste_de_listes[1][2])  # Accède à 6

        tuple_de_tuples = ((10, 20), (30, 40), (50, 60))
        print(tuple_de_tuples[2][0])  # Accède à 50
        ```
        *Affiche :*
        ```
        6
        50
        ```
        {{ IDE() }}

## <span style="color:blue;">Opérations sur les listes/tuples</span>

???+ question "7. Méthodes `pop`, `append` et opérateur `in`"

    - **`append`** ajoute un élément à la fin de la liste.
    - **`pop`** retire et retourne le dernier élément de la liste.
    - **`in`** vérifie la présence d'un élément dans la liste.

    ??? success "Exemple"

        ```python
        fruits = ["pomme", "banane", "cerise"]
        fruits.append("orange")
        print(fruits)

        dernier = fruits.pop()
        print("Élément supprimé:", dernier)
        print(fruits)

        print("pomme" in fruits)  # Vérifie si "pomme" est dans la liste
        print("kiwi" in fruits)
        ```
        *Affiche :*
        ```
        ['pomme', 'banane', 'cerise', 'orange']
        Élément supprimé: orange
        ['pomme', 'banane', 'cerise']
        True
        False
        ```
        {{ IDE() }}

## <span style="color:blue;">Création de liste par compréhension</span>

???+ question "8. Création de listes par compréhension"

    La **compréhension de liste** est une manière concise de créer des listes en utilisant des boucles.

    **Syntaxe :**
    ```python
    nouvelle_liste = [expression for élément in iterable]
    ```

    ??? success "Exemple 1 : Doubler chaque élément d'une liste"

        ```python
        nombres = [1, 2, 3, 4, 5]
        doubles = [x * 2 for x in nombres]
        print(doubles)
        ```
        *Affiche :*
        ```
        [2, 4, 6, 8, 10]
        ```
        {{ IDE() }}

    ??? success "Exemple 2 : Filtrer les éléments pairs"

        ```python
        nombres = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
        pairs = [x for x in nombres if x % 2 == 0]
        print(pairs)
        ```
        *Affiche :*
        ```
        [2, 4, 6, 8, 10]
        ```
        {{ IDE() }}

    ??? success "Exemple 3 : Créer une liste de carrés"

        ```python
        carre = [x**2 for x in range(6)]
        print(carre)
        ```
        *Affiche :*
        ```
        [0, 1, 4, 9, 16, 25]
        ```
        {{ IDE() }}

## <span style="color:blue;">La boucle for sur les chaines/listes/tuples</span>

???+ question "9. Boucles `for` **❤️ Boucle `for` sur chaque élément d'une chaîne : ❤️**""

    ```python
    for elmt in chaine_de_caractere:
        print(elmt)
    ```

    ??? success "Exemple"
        ```python
        texte = "Python"
        for elmt in texte:
            print(elmt)
        ```
        *Affiche :*
        ```
        P
        y
        t
        h
        o
        n
        ```
        {{ IDE() }}

???+ question "10. Boucles `for` **❤️ Boucle `for` sur chaque élément d'une liste : ❤️**""

    ```python
    for elmt in [10, 20, 30, 40]:
        print(elmt)
    ```

    ??? success "Exemple"

        ```python
        nombres = [10, 20, 30, 40]
        for elmt in nombres:
            print(elmt)
        ```
        *Affiche :*
        ```
        10
        20
        30
        40
        ```
        
        {{ IDE() }}

???+ question "11. Boucles `for` **❤️ Boucle `for` sur chaque élément d'un tuple  ❤️**""

    ```python
    for elmt in ("rouge", "vert", "bleu"):
        print(elmt)
    ```

    ??? success "Exemple"

        ```python
        couleurs = ("rouge", "vert", "bleu")
        for elmt in couleurs:
            print(elmt)
        ```
        *Affiche :*
        ```
        rouge
        vert
        bleu
        ```
        {{ IDE() }}

???+ question "12. Boucles `for` **❤️ Boucle `for` sur chaque indice  d'une chaîne : ❤️**""

    ```python
    for i in range(len(chaine_de_caractere)):
        print(chaine_de_caractere[i])
    ```

    ??? success "Exemple"
        ```python
        texte = "Python"
        for i in range(len(texte)):
            print(texte[i])
        ```
        *Affiche :*
        ```
        P
        y
        t
        h
        o
        n
        ```
        {{ IDE() }}

???+ question "13. Boucles `for` **❤️ Boucle `for` sur chaque indice d'une liste : ❤️**""

    ```python
    for i in range(len(la_liste)):
        print(la_liste[i])
    ```

    ??? success "Exemple"

        ```python
        nombres = [10, 20, 30, 40]
        for i in range(len(nombres)):
            print(nombres[i])
        ```
        *Affiche :*
        ```
        10
        20
        30
        40
        ```
        
        {{ IDE() }}

???+ question "14. Boucles `for` **❤️ Boucle `for` sur chaque indice d'un tuple  ❤️**""

    ```python
    for i in range(len(le_tuple)):
        print(le_tuple[i])
    ```

    ??? success "Exemple"

        ```python
        couleurs = ("rouge", "vert", "bleu")
        for i in in range(len(couleurs)):
            print(couleurs[i])
        ```
        *Affiche :*
        ```
        rouge
        vert
        bleu
        ```
        {{ IDE() }}

## <span style="color:blue;">La boucle while les listes/tuples</span>

???+ question "15. Boucle `while`"

    La boucle `while` répète des instructions tant qu'une condition est vraie.

    ```python
    while condition:
        # Code à répéter tant que la condition est vraie
    ```

    ??? success "Exemple avec une liste"

        ```python
        nombres = [1, 2, 3, 4, 5]
        i = 0
        while i < len(nombres):
            print(nombres[i])
            i += 1
        ```
        *Affiche :*
        ```
        1
        2
        3
        4
        5
        ```
        {{ IDE() }}

    ??? success "Exemple avec un tuple"

        ```python
        couleurs = ("rouge", "vert", "bleu")
        i = 0
        while i < len(couleurs):
            print(couleurs[i])
            i += 1
        ```
        *Affiche :*
        ```
        rouge
        vert
        bleu
        ```
        {{ IDE() }}





## <span style="color:blue;">Exercices sur les listes/tuples</span>

???+ question "Exercice 1 : Somme des valeurs d'une liste"

    Écris une fonction qui calcule la somme des valeurs d'une liste sans utiliser la fonction `sum()`.

    **Exemple :**

    Pour la liste `[1, 2, 3, 4, 5]`, la fonction doit retourner `15`.

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"
        On utilise une boucle `for` pour additionner chaque élément.

        ```python
        def somme_liste(liste):
            total = 0
            for valeur in liste:
                total += valeur
            return total

        print(somme_liste([1, 2, 3, 4, 5]))
        ```

        **Explication :**

        - La variable `total` est initialisée à 0.

        - La boucle `for` parcourt chaque élément de la liste et l'ajoute à `total`.

        - La somme finale est retournée.


???+ question "Exercice 2 : Moyenne des valeurs d'une liste"

    Écris une fonction qui calcule la moyenne des valeurs d'une liste sans utiliser `sum()`.

    **Exemple :**

    Pour la liste `[4, 8, 15, 16, 23, 42]`, la fonction doit retourner `18`.

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"
        On utilise la fonction de l'exercice précédent pour calculer la somme.

        ```python
        def moyenne_liste(liste):
            total = 0
            for valeur in liste:
                total += valeur
            return total / len(liste)

        print(moyenne_liste([4, 8, 15, 16, 23, 42]))
        ```

        **Explication :**

        - La somme des valeurs est calculée comme dans l'exercice 1.

        - Le total est ensuite divisé par la longueur de la liste pour obtenir la moyenne.


???+ question "Exercice 3 : Trouver le maximum dans une liste"

    Écris une fonction qui trouve la valeur maximale d'une liste sans utiliser `max()`.

    **Exemple :**

    Pour la liste `[10, 20, 5, 25, 15]`, la fonction doit retourner `25`.

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"
        On initialise le maximum avec le premier élément de la liste.

        ```python
        def max_liste(liste):
            maximum = liste[0]
            for valeur in liste:
                if valeur > maximum:
                    maximum = valeur
            return maximum

        print(max_liste([10, 20, 5, 25, 15]))
        ```

        **Explication :**

        - On commence avec le premier élément comme maximum.

        - La boucle compare chaque élément au maximum actuel et le remplace si un plus grand est trouvé.


???+ question "Exercice 4 : Recherche d'un élément dans une liste"

    Écris une fonction qui vérifie si un élément est présent dans une liste.

    **Exemple :**

    Pour la liste `[3, 6, 9, 12]` et l'élément `9`, la fonction doit retourner `True`.

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"
        On utilise une boucle pour vérifier chaque élément.

        ```python
        def recherche_element(liste, element):
            for valeur in liste:
                if valeur == element:
                    return True
            return False

        print(recherche_element([3, 6, 9, 12], 9))
        print(recherche_element([3, 6, 9, 12], 5))
        ```

        **Explication :**

        - La boucle vérifie chaque élément de la liste.

        - Si l'élément est trouvé, la fonction retourne `True`, sinon elle retourne `False` après avoir parcouru toute la liste.


???+ question "Exercice 5 : Multiplication par addition"

    Écris une fonction qui multiplie deux nombres en utilisant seulement des additions.

    **Exemple :**

    Pour les nombres `4` et `3`, la fonction doit retourner `12`.

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"
        On utilise une boucle `for` pour ajouter un nombre à lui-même plusieurs fois.

        ```python
        def multiplication(a, b):
            result = 0
            for i in range(b):
                result += a
            return result

        print(multiplication(4, 3))
        ```

        **Explication :**

        - La boucle s'exécute `b` fois, ajoutant `a` à chaque itération.

        - Le résultat final est équivalent à `a * b`.

???+ question "Exercice 6 : Doubler les éléments d'une liste"

    Écris une fonction qui double chaque élément d'une liste en utilisant `for i in range(len(liste))`.

    **Exemple :**

    Pour la liste `[1, 2, 3, 4]`, la fonction doit retourner `[2, 4, 6, 8]`.

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"
        On utilise l'indice pour accéder et modifier chaque élément.

        ```python
        def doubler_elements(liste):
            for i in range(len(liste)):
                liste[i] *= 2
            return liste

        print(doubler_elements([1, 2, 3, 4]))
        ```

        **Explication :**

        - La boucle `for i in range(len(liste))` permet d'accéder à chaque indice de la liste.

        - Chaque élément est multiplié par 2 et la liste modifiée est retournée.

## <span style="color:blue;">join() et split() sur les chaine de caractère</span>

???+ question "16. Utiliser join et split sur les chaînes de caractères"

    Les méthodes **`join`** et **`split`** permettent de manipuler des chaînes de caractères et des listes.

    - **`join`** : Combine les éléments d'une liste en une seule chaîne.
    - **`split`** : Divise une chaîne en une liste d'éléments.

    **Syntaxe :**
    ```python
    chaine = separateur.join(liste)
    liste = chaine.split(separateur)
    ```

    ??? success "Exemple avec `join`"

        ```python
        mots = ["Bonjour", "le", "monde"]
        phrase = " ".join(mots)
        print(phrase)
        ```
        *Affiche :*
        ```
        Bonjour le monde
        ```
        {{ IDE() }}

    ??? success "Exemple avec `split`"

        ```python
        phrase = "Bonjour le monde"
        mots = phrase.split(" ")
        print(mots)
        ```
        *Affiche :*
        ```
        ['Bonjour', 'le', 'monde']
        ```
        {{ IDE() }}

## <span style="color:blue;">Les dictionnaires</span>

???+ question "17. Les dictionnaires"

    Les **dictionnaires** sont des collections de paires *clé-valeur*.

    **Syntaxe :**
    ```python
    mon_dictionnaire = {"cle1": valeur1, "cle2": valeur2}
    ```

    ??? success "Exemple"

        ```python
        etudiant = {"nom": "Bob", "age": 20, "matiere": "Maths"}
        print(etudiant)
        ```
        *Affiche :*
        ```
        {'nom': 'Bob', 'age': 20, 'matiere': 'Maths'}
        ```
        {{ IDE() }}

???+ question "18. Ajouter une valeur à un dictionnaire"

    Vous pouvez ajouter une nouvelle paire *clé-valeur* à un dictionnaire.

    **Syntaxe :**
    ```python
    dictionnaire["nouvelle_cle"] = nouvelle_valeur
    ```

    ??? success "Exemple"

        ```python
        etudiant = {"nom": "Bob", "age": 20}
        etudiant["matiere"] = "Maths"
        print(etudiant)
        ```
        *Affiche :*
        ```
        {'nom': 'Bob', 'age': 20, 'matiere': 'Maths'}
        ```
        {{ IDE() }}

???+ question "19. Création d'un dictionnaire par compréhension"

    Vous pouvez créer des dictionnaires à l'aide de **compréhensions**.

    **Syntaxe :**
    ```python
    dictionnaire = {cle: valeur for cle, valeur in iterable}
    ```

    ??? success "Exemple"

        ```python
        carres = {x: x**2 for x in range(5)}
        print(carres)
        ```
        *Affiche :*
        ```
        {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
        ```
        {{ IDE() }}

???+ question "20. Parcourir un dictionnaire avec keys(), values() et items()"

    Vous pouvez parcourir les clés, les valeurs ou les paires *clé-valeur* d'un dictionnaire.

    **Syntaxe :**
    ```python
    # Clés
    for cle in dictionnaire.keys():
        print(cle)

    # Valeurs
    for valeur in dictionnaire.values():
        print(valeur)

    # Clés et valeurs
    for cle, valeur in dictionnaire.items():
        print(cle, valeur)
    ```

    ??? success "Exemple"

        ```python
        etudiant = {"nom": "Alice", "age": 25, "matiere": "Physique"}

        print("Clés :")
        for cle in etudiant.keys():
            print(cle)

        print("Valeurs :")
        for valeur in etudiant.values():
            print(valeur)

        print("Paires clé-valeur :")
        for cle, valeur in etudiant.items():
            print(cle, valeur)
        ```
        *Affiche :*
        ```
        Clés :
        nom
        age
        matiere

        Valeurs :
        Alice
        25
        Physique

        Paires clé-valeur :
        nom Alice
        age 25
        matiere Physique
        ```
        {{ IDE() }}

???+ question "21. Opérateur in, fonction len, suppression avec del"

    Vous pouvez vérifier l'existence d'une clé, mesurer la taille d'un dictionnaire et supprimer des paires *clé-valeur*.

    **Syntaxe :**
    ```python
    # Vérification d'une clé
    "cle" in dictionnaire

    # Taille du dictionnaire
    len(dictionnaire)

    # Suppression d'une clé

    del dictionnaire["cle"]
    ```

    ??? success "Exemple"

        ```python
        etudiant = {"nom": "Alice", "age": 25, "matiere": "Physique"}

        # Vérification
        print("age" in etudiant)  # True
        print("adresse" in etudiant)  # False

        # Taille
        print(len(etudiant))  # 3

        # Suppression
        del etudiant["matiere"]
        print(etudiant)
        ```
        *Affiche :*
        ```
        True
        False
        3
        {'nom': 'Alice', 'age': 25}
        ```
        {{ IDE() }}

## <span style="color:blue;">Exercices sur les dictionnaires</span>


???+ question "Exercice 1 : Somme des valeurs d'un dictionnaire"

    Écris une fonction qui calcule la somme des valeurs dans un dictionnaire.

    **Exemple :**

    Pour le dictionnaire `{'a': 5, 'b': 10, 'c': 15}`, la fonction doit retourner `30`.

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"
        On utilise `values()` pour accéder aux valeurs.

        ```python
        def somme_dictionnaire(dico):
            total = 0
            for valeur in dico.values():
                total += valeur
            return total

        print(somme_dictionnaire({'a': 5, 'b': 10, 'c': 15}))
        ```

        **Explication :**

        - La méthode `values()` permet de récupérer toutes les valeurs du dictionnaire.

        - On additionne chaque valeur à `total`.





???+ question "Exercice 2 : Calcul de la moyenne des notes"

    Écris une fonction qui calcule la moyenne des notes dans un dictionnaire où les clés sont les matières et les valeurs sont les notes.

    **Exemple :**

    Pour le dictionnaire `{'math': 14, 'physique': 16, 'chimie': 12}`, la fonction doit retourner `14`.

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"
        On utilise `values()` pour récupérer les notes.

        ```python
        def moyenne_notes(dico):
            total = 0
            for note in dico.values():
                total += note
            return total / len(dico)

        print(moyenne_notes({'math': 14, 'physique': 16, 'chimie': 12}))
        ```

        **Explication :**

        - On additionne toutes les notes et on divise par le nombre de matières.

???+ question "Exercice 3 : Matière avec la meilleure note"

    Écris une fonction qui retourne la matière avec la meilleure note dans un dictionnaire où les clés sont les matières et les valeurs sont les notes.

    **Exemple :**

    Pour le dictionnaire `{'math': 14, 'physique': 16, 'chimie': 12}`, la fonction doit retourner `'physique'`.

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"
        On utilise `items()` pour comparer les paires clé-valeur.

        ```python
        def meilleure_matiere(dico):
            meilleure_note = -1
            for matiere, note in dico.items():
                if note > meilleure_note:
                    meilleure_note = note
                    meilleure_matiere = matiere
            return meilleure_matiere

        print(meilleure_matiere({'math': 14, 'physique': 16, 'chimie': 12}))
        ```

        **Explication :**

        - On initialise `meilleure_note` à -1 pour s'assurer que toute note dans le dictionnaire sera plus grande.
        - La boucle parcourt chaque paire (matière, note) et met à jour les variables si une note plus élevée est trouvée.
        - La fonction retourne la matière avec la meilleure note.

