---
author: ELP
title: 03 Mise au point des scripts et gestion des exceptions
---


**Table des matières**

1. [Les bonnes pratiques : documenter les fonctions ](#_page0_x40.00_y447.92)
2. [Les tests](#_page1_x40.00_y350.92)
3. [Les préconditions et les postconditions](#_page5_x40.00_y567.92)
4. [Les bibliothèques ou modules](#_page6_x40.00_y233.92)
5. [Gestion des exceptions](#_page9_x40.00_y401.92)
6. [Exercices](#_page13_x40.00_y375.92)

## **<H2 STYLE="COLOR:BLUE;">1.  Les bonnes pratiques : <a name="_page0_x40.00_y447.92"></a>documenter les fonctions</h2>** 

### **<H3 STYLE="COLOR:GREEN;">1.1. Qu’est-ce qu’une docstring ?</h3>**

Une **docstring** est un texte placé juste après l’en-tête d’une fonction en Python. Elle sert à expliquer :

1. **Ce que fait la fonction** ;

2. **Comment l’utiliser** (informations sur les paramètres et le résultat attendu) ;

3. **Les conditions d’utilisation pour éviter les erreurs**.

Elle est essentielle pour rendre le code **plus lisible** et **plus compréhensible**.




### **<H3 STYLE="COLOR:GREEN;">1.2. Comment écrire une docstring ?</h3>**

???+ question "Activité n°1 : Ajouter une docstring"

    **Tester :**

    ```python
    def factorielle(n):
        """
        Fonction qui retourne la factorielle d'un nombre.
        :param n: int
        :return: int
        CU (conditions d'utilisation) : n >= 0
        >>> factorielle(0)
        1
        >>> factorielle(4)
        24
        """
        resultat = 1
        for i in range(2, n + 1):
            resultat = resultat * i
        return resultat
    ```

    **Exécuter :**
    
    ```python
    help(factorielle)
    ```

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"

        **Résultat :**
        ```
        Help on function factorielle in module __main__:

        factorielle(n)
            Fonction qui retourne la factorielle d'un nombre.
            :param n: int
            :return: int
            CU (conditions d'utilisation) : n >= 0
            >>> factorielle(0)
            1
            >>> factorielle(4)
            24
        ```

    **Explication :**

    - La **docstring** est affichée avec la commande `help(factorielle)`.

    - Elle décrit clairement **l’objectif** et **les conditions d’utilisation**.




## **<H2 STYLE="COLOR:BLUE;">2. Les<a name="_page1_x40.00_y350.92"></a> tests</h2>** 

Les tests permettent de **vérifier** qu’un programme ne produit pas d’erreur et qu’il effectue bien la tâche attendue.
  

### **<H3 STYLE="COLOR:GREEN;">2.1. Les<a name="_page1_x40.00_y523.92"></a> tests simples avec assert</h3>**

L’instruction `assert` est utilisée pour **vérifier rapidement** que le programme fonctionne correctement.

???+ question "Activité n°2 : Vérifier une fonction avec `assert`"

    **Tester :**

    ```python
    def carre(x):
        return x * x

    assert carre(3) == 9  # Vérifie que le carré de 3 est bien 9
    assert carre(0) == 0  # Vérifie que le carré de 0 est bien 0
    assert carre(-2) == 4 # Vérifie que le carré de -2 est bien 4
    ```

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"

        **Résultat :** *(Aucune sortie si tout est correct !)*

    **Explication :**

    - Si **tous les tests passent**, le programme continue.

    - Si **un test échoue**, Python affiche une **erreur** et s’arrête.



???+ question "Activité n°3 : Vérifier une division euclidienne"

    **Tester :**

    ```python
    def division_euclidienne(a, b):   
        if a >= 0 and b > 0 and type(a) == int and type(b) == int:   
            return a // b, a % b   # Quotient et reste
        else:   
            return -1   # Code d'erreur si les valeurs ne sont pas correctes

    if __name__ == '__main__':
        assert division_euclidienne(10, 2) == (5, 0)
        assert division_euclidienne(-10, 7) == -1
        assert division_euclidienne(10.3, 4) == -1
        assert division_euclidienne(3, 0) == -1
    ```

    ??? success "Python"
        {{ IDE() }}

 

**Explication :**

- `assert` teste plusieurs cas et arrête le programme **si un test échoue**.

- La condition `if __name__ == '__main__':` garantit que ces tests ne s’exécutent **que si le fichier est exécuté directement**.

???+ question "Activité n°4 : Vérifier une division euclidienne"

    **Tester :**

    ```python
    def division_euclidienne(a, b):   
        if a >= 0 and b > 0 and type(a) == int and type(b) == int:   
            return a // b, a % b   # Quotient et reste
        else:   
            return -1   # Code d'erreur si les valeurs ne sont pas correctes

    if __name__ == '__main__':
        assert division_euclidienne(10, 2) == (5, 1)
        assert division_euclidienne(-10, 7) == -1
        assert division_euclidienne(10.3, 4) == -1
        assert division_euclidienne(3, 0) == -1
    ```

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"

        **Explication :**

        assert division_euclidienne(10, 2) == (5, 1)  # Test invalide, Python montre une erreur, car le reste de `10 divisé par 2` est `0` et non `1`.


   


### **<H3 STYLE="COLOR:GREEN;">2.2. Les<a name="_page2_x40.00_y650.92"></a> tests avec doctest</h3>** 



`doctest` permet de **vérifier automatiquement** que les exemples donnés dans la docstring sont corrects.

???+ question "Activité n°5 : Tester avec `doctest`"

    **Tester :**

    ```python
    import doctest

    def factorielle(n):
        """
        Fonction qui retourne la factorielle d'un nombre.
        :param n: int
        :return: int
        CU (conditions d'utilisation) : n >= 0

        >>> factorielle(0)
        1
        >>> factorielle(4)
        24
        """
        resultat = 1
        for i in range(2, n + 1):
            resultat = resultat * i
        return resultat

    doctest.testmod()
    ```

    ??? success "Python"
        ![ALLER SUR BASTHON](https://console.basthon.fr/)

    ??? success "Solution"

        **Explication :**

        - `doctest` recherche les **triples chevrons `>>>`** dans la docstring.

        - Il exécute les exemples et compare le **résultat attendu** et **le résultat réel**.
        
        - Si tout est correct, **rien ne s’affiche**.

---

???+ question "Activité n°6 : Introduire une erreur dans `doctest`"

    **Tester :**

    ```python
    import doctest

    def factorielle(n):
        """
        Fonction qui retourne la factorielle d'un nombre.
        :param n: int
        :return: int
        CU (conditions d'utilisation) : n >= 0

        >>> factorielle(0)
        100000  # ERREUR VOLONTAIRE
        >>> factorielle(4)
        24
        """
        resultat = 1
        for i in range(2, n + 1):
            resultat = resultat * i
        return resultat

    doctest.testmod()
    ```

    ??? success "Python"
        ![ALLER SUR BASTHON](https://console.basthon.fr/)

    ??? success "Solution"

        **Explication :**

        - Doctest va signaler une erreur, car `factorielle(0)` ne renvoie **pas** `100000`.

        - C’est une **façon rapide de vérifier** si les résultats des tests sont corrects.

---

???+ question "Activité n°7 : Activer le mode `verbose` dans `doctest`"

    **Tester :**

    ```python
    import doctest

    def factorielle(n):
        """
        Fonction qui retourne la factorielle d'un nombre.
        :param n: int
        :return: int
        CU (conditions d'utilisation) : n >= 0

        >>> factorielle(0)
        1
        >>> factorielle(4)
        24
        """
        resultat = 1
        for i in range(2, n + 1):
            resultat = resultat * i
        return resultat

    doctest.testmod(verbose=True)
    ```

    ??? success "Python"
        ![ALLER SUR BASTHON](https://console.basthon.fr/)

    ??? success "Solution"

        **Explication :**

        - En mode **verbose**, doctest affiche **tous les tests**, qu’ils réussissent ou échouent.

        - **Utile pour voir ce qui est testé**.







## **<H2 STYLE="COLOR:BLUE;">3. Les<a name="_page5_x40.00_y567.92"></a> préconditions et les postconditions</h2>** 

Les assertions permettent de **vérifier les résultats** tout au long de l'exécution d'un programme, et pas seulement **test par test**.

???+ question "Activité n°8 :"

    **Tester :**

    ```python
    from math import sqrt

    def square_root(x):
        """
        Fonction qui calcule la racine carrée d'un nombre positif.
        :param x: int ou float (doit être >= 0)
        :return: float
        Précondition : x >= 0
        Postcondition : Le carré du résultat doit être égal à x
        """
        assert x >= 0, "Précondition non respectée : x doit être positif ou nul"
        racine = sqrt(x)
        assert racine**2 == x, "Postcondition non respectée : erreur dans le calcul"
        return racine

    print(square_root(25))  # Doit afficher 5.0
    print(square_root(-25)) # Doit lever une erreur
    ```

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"

        **Explication :**

        - **Précondition** `assert x >= 0` : empêche le calcul si `x` est négatif.

        - **Postcondition** `assert racine**2 == x` : vérifie la validité du résultat.

        - Si une **précondition ou postcondition** échoue, **le programme s’arrête immédiatement avec un message d’erreur**.



Pour plus de précisions :[ https://www.youtube.com/watch?v=DRVoh5XiAZo ](https://www.youtube.com/watch?v=DRVoh5XiAZo)



## **<H2 STYLE="COLOR:BLUE;">4. Les<a name="_page6_x40.00_y233.92"></a> bibliothèques ou modules</h2>** 
### **<H3 STYLE="COLOR:GREEN;">4.1. Qu’est<a name="_page6_x40.00_y255.92"></a> ce que c’est ?</h3>** 

Une bibliothèque en Python est un fichier contenant du **code réutilisable**. Ce code peut être sous forme de fonctions, classes ou variables. Pour utiliser une bibliothèque, on doit **l’importer** dans notre programme. 

Une bibliothèque est une **collection de modules**. C'est un ensemble plus large qui peut contenir **plusieurs fichiers** Python, **chacun étant un module**.

### **<H3 STYLE="COLOR:GREEN;">4.2. Importer des bibliothèques (modules) standards<a name="_page6_x40.00_y498.92"></a></h3>** 



Voici un exemple d’utilisation d’une bibliothèque Python standard, le module ```math``` qui contient des fonctions mathématiques comme sqrt() (racine carrée) ou ```random``` qui contient randint.
On peut noter les 2 manières d’importer le module : 

- import `nom_du_module`

- from `nom_du_module` import `nom_de_la_fonction`


```
>>> import math as m
>>> m.sqrt(25) 

>>> import math
>>> math.sqrt(25) 

>>> from math import sqrt
>>> sqrt(25) 

>>> import random 
>>> x = random.randint(0,10)  

>>> from random import randint 
>>> randint(0,10) 

>>> from random import * 
>>> randint(0,10) 
>>> randrange(10,20,3) #retourne un entier entre 10 et 19 et un pas =3 
```

???+ question "Activité n°9 :"

    **Tester :**

    ```python
    import math
    import random

    print(math.sqrt(25))  # Racine carrée de 25

    x = random.randint(0, 10)  # Génère un nombre aléatoire entre 0 et 10
    print("Nombre aléatoire :", x)
    ```

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"

        **Explication :**

        - **`import math`** permet d’accéder aux fonctions mathématiques.

        - **`import random`** permet d'utiliser des fonctions pour générer des nombres aléatoires.





  
## **<H2 STYLE="COLOR:BLUE;">5. Gestion<a name="_page9_x40.00_y401.92"></a> des exceptions</h2>** 

Ce sont les erreurs que peut rencontrer Python en exécutant le programme. Ces erreurs peuvent être **interceptées** très facilement et dans certains cas indispensables. 

### **<H3 STYLE="COLOR:GREEN;">5.1. Lever<a name="_page9_x40.00_y467.92"></a> d’exception</h3>** 

Une **exception** est une erreur qui se produit lors de l’exécution du programme. Si elle n'est pas gérée, **le programme s’arrête brutalement**.
 
???+ question "Activité n°10 :"

    **Tester :**

    ```python
    print(1 / 0)  # Division par zéro
    ```

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"

        **Explication :**

        - Python **lève une exception `ZeroDivisionError`**, car **on ne peut pas diviser par zéro**.



### **<H3 STYLE="COLOR:GREEN;">5.2. Gestions<a name="_page9_x40.00_y734.92"></a> des exceptions</h3>**

On peut **éviter l'arrêt brutal** d'un programme en capturant les erreurs avec `try` et `except`.


#### **<H4 STYLE="COLOR:MAGENTA;">5.2.1. Forme<a name="_page10_x40.00_y106.92"></a> minimale du bloc try</h4>**

???+ question "Activité n°11 : Gestion des erreurs d’entrée utilisateur"

    ```python
    annee = input('Entrer une année : ')

    try:
        annee = int(annee)  # Convertit l'entrée en entier
        print("Pas de problème :", annee)
    except ValueError:
        print("Erreur : Vous devez entrer un nombre entier.")
    ```

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"

        **Explication :**

        - **Si l’utilisateur entre un nombre**, tout fonctionne.

        - **Si l’utilisateur entre du texte**, une erreur est capturée et un message explicite s’affiche.




#### **<H4 STYLE="COLOR:MAGENTA;">5.2.2. Interception<a name="_page10_x40.00_y547.92"></a> d’exceptions particulières</h4> Dans le cas d’une division :** 

On peut capturer **différents types d’erreurs** séparément.

???+ question "Activité n°12 : Gérer plusieurs erreurs"

    ```python
    numerateur = input('Numérateur ? ')
    denominateur = input('Dénominateur ? ')

    try:
        resultat = int(numerateur) / int(denominateur)
    except ValueError:
        print("Erreur : Vous devez entrer des nombres.")
    except ZeroDivisionError:
        print("Erreur : Division par zéro impossible.")
    else:
        print("Le résultat est :", resultat)
    ```

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"

        **Explication :**

        - `ValueError` : L'utilisateur a saisi un texte au lieu d’un nombre.

        - `ZeroDivisionError` : L’utilisateur a essayé de diviser par zéro.

        - `else` : S’exécute **uniquement si aucun `except` n’a été déclenché**.






#### **<H4 STYLE="COLOR:MAGENTA;">5.2.3. <a name="_page11_x40.00_y338.92"></a>`finally` : Toujours exécuté</h4>**

???+ question "Activité n°13 : Utilisation de `finally`"

    ```python
    numerateur = input('Numérateur ? ')
    denominateur = input('Dénominateur ? ')

    try:
        resultat = int(numerateur) / int(denominateur)
    except ValueError:
        print("Erreur : Vous devez entrer des nombres.")
    except ZeroDivisionError:
        print("Erreur : Division par zéro impossible.")
    else:
        print("Le résultat est :", resultat)
    finally:
        print("Fin de l’exécution du programme.")
    ```

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"

        **Explication :**

        - `finally` affiche **un message final**, que **le programme ait réussi ou non**.



#### **<H4 STYLE="COLOR:MAGENTA;">5.2.4. <a name="_page12_x40.00_y346.92"></a>Lever une exception avec `raise`</h4>**

On peut **déclencher une exception volontairement** si une valeur est incorrecte.
???+ question "Activité n°14 : Déclencher une erreur volontairement"

    ```python
    annee = input("Saisissez une année supérieure à 0 : ")

    try:
        annee = int(annee)
        if annee >= 3000 or annee < 0:
            raise ValueError("L'année doit être comprise entre 0 et 2999.")
    except ValueError as e:
        print("Erreur :", e)
    ```

    ??? success "Python"
        {{ IDE() }}

    ??? success "Solution"

        **Explication :**

        - **Si l’utilisateur entre `3000`**, l’exception est déclenchée volontairement.
        
        - **Le message d’erreur est personnalisé.**

    
## **<H2 STYLE="COLOR:BLUE;">6. Exercices<a name="_page13_x40.00_y375.92"></a></h2>** 
=> **CAPYTALE Le code vous sera donné par votre enseignant**

!!! abstract "Exercice 1"  
    On considère la fonction multiplier_par_deux(x) qui prend en paramètre x et qui renvoie son double. Ecrire un script de cette fonction avec : 


    - Sa documentation dans le docstring comme dans le cours. Ne pas mettre d’exemples 

    - 4 tests en assert  : avec 0, avec -1, avec 3.2 et avec ‘a’ 



!!! abstract "Exercice 2" 

    On considère une fonction somme_carres(x) qui prend en paramètre x (entier strictement positif) et renvoie la somme des x premiers carrés non nuls.  

    Ecrire un script de cette fonction avec : 


    - Sa documentation dans le docstring comme dans l’activité 1. Ne pas mettre d’exemples 

    - 3 tests en assert (comme dans l’activité 3) : avec 1, avec 2 et avec 3 


!!! abstract "Exercice 3" 

    La fonction précédente à pour condition d’utilisation : x doit être strictement positif. Déclencher une exception et capturer là si le nombre entrée est 0 ou négatif. 

    Exemple : 
    ```
    >>> somme_carres(5)
    55

    >>> somme_carres(0)
    Le nombre entré est nul ou négatif

    >>> somme_carres(-2)
    Le nombre entré est nul ou négatif
    ```



!!! abstract "Exercice 4"  
    
    On part du script suivant qui permet d’inverser un nombre 

    ```python
    try:
        chaine = input('Entrer un nombre : ') # permet de pouvoir gérer l’exception ValueError
        nombre = float(chaine)
        inverse = 1.0/nombre
    except ValueError:
        #ce bloc est exécuté si une exception de type ValueError est levée dans le bloc try
        print(chaine, "n'est pas un nombre !")

    except ZeroDivisionError:
    #ce bloc est exécuté si une exception de type ZeroDivisionError est levée dans le bloc try
        print("Division par zéro !")
    else:
        #on arrive ici si aucune exception n'est levée dans le bloc try
        print("L'inverse de", nombre, "est :", inverse) 
    ```


    Compléter le script précédent de manière à ressaisir le nombre en cas d’erreur. On pourra englober le script dans une boucle while. Par exemple : 

    Tester avec :

    - 'a'

    - relancer le programme et tester avec => 'salut !'

    - relancer le programme et tester avec => 0

    - relancer le programme et tester avec => 2

    Aide : penser à une boucle infinie et au mot clé break.  

!!! abstract "Exercice 5" 

    Ecrire un script qui calcule la racine carrée d’un nombre, avec gestion des exceptions. Utiliser la fonction sqrt() du module math.  

    Par exemple : 
    ``` 
    Entrer un nombre : >? go
    go n'est pas un nombre valide !
    Entrer un nombre : >? -5.26
    -5.26 n'est pas un nombre valide !
    Entrer un nombre : >? 16
    La racine de  16.0 est : 4.0
    ```

    Tester avec :

    - "go"

    - relancer le programme et tester avec => -5.26

    - relancer le programme et tester avec => 16

    Remarquez bien qu’on demande le nombre à l’utilisateur *jusqu'à* ce qu’il convienne. 

[Documentation sur les exceptions ](http://docs.python.org/3/tutorial/errors.html#exceptions)
Source :[ Fabrice Sincère ](http://fsincere.free.fr/isn/python/cours_python_ch5.php)-[ Contenu sous licence CC BY-NC-SA 3.0 ](http://creativecommons.org/licenses/by-nc-sa/3.0/fr/)
