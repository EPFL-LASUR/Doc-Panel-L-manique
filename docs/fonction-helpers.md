# Package panlemhelpers

## Installation

Afin de permettre une meilleure visualisation des données de chaque vague, un package nommé `panlemhelpers` a été créé. Ce package contient des fonctions helpers permettant de séparer et trier les informations contenues dans les données brutes. Il peut être téléchargé depuis GitHub en utilisant le package `remotes`:

```r
install.packages("remotes")
remotes::install_github("EPFL-LASUR/panlemhelpers")
```

Pour chaque vague du Panel Lémanique, six fonctions ont été créées. Elles sont fondamentalement similaires à travers les vagues, néanmoins, dû à la structure des données brutes, il était compliqué d'implémenter une seule fonction permettant d'obtenir le même résultat pour toutes les vagues (pour les informaticien·nes: oui, oui, je sais, c'est absolument ignoble... but time and money baby). Afin de distinguer ces fonctions similaires, un suffixe a été rajouté dans leur nom afin d'indiquer sur quelle vague elles peuvent être utilisées (e.g. `get_participants_wave1()` pour obtenir les informations sur les participant·es dans la vague 1).

On distingue six différentes fonctions helpers qui sont présentées ci-dessous.

## `get_participants`

Cette fonction permet d'obtenir les informations sur les participant·es des différentes vagues. On obtient ainsi l'ID, le pays de résidence selon l'échantillon d'origine, le pays de résidence lors de la vague actuelle, le genre, la date de naissance, le lieu de résidence, etc... pour chaque participant·e.

La fonction rend deux tableaux. Le premier, nommé `participants`, contient les informations citées ci-dessus, tandis que le deuxième, nommé `labels`, est un dictionnaire des variables listées dans le premier. Voici comment y accéder en prenant exemple sur la première vague du panel:

``` r
wave1_data <- opal.table_get(o, "Panel Lémanique", "wave1")
fichier <- get_participants_wave1(wave1_data)
fichier$participants
fichier$labels
```

## `get_questions`

Cette fonction permet de lister les questions de chaque vague, en indiquant le nom de la variable (e.g. Q42), le texte de la question (e.g. *"Possédez-vous le permis de conduire?"*) ainsi que la section du questionnaire à laquelle la question appartient (e.g. *Portfolio personnel*). En addition, afin de faciliter le suivi entre les vagues, le tableau pour la vague 3 possède une colonne nommée `lien_W1`, indiquant si la question reprend une question de la première vague.

Comme pour la fonction `get_participants`, la fonction renvoie deux tableaux, le premier contenant les informations nommées ci-dessus, le deuxième le dictionnaire des variables. Ils sont accessibles avec `fichier$questions` et `fichier$labels`.

## `get_section`

Cette fonction renvoie un tableau listant les sections du questionnaire de la vague concernée.

## `get_documentation`

Cette fonction est très similaire à la fonction `opal.table_dictionary_get()`. Elle permet d'obtenir un dictionnaire de toutes les variables de la vague concernée.

## `get_survey_completion`

À l'aide de cette fonction, il est possible de filter les métadonnées des vagues. Elle fournit ainsi des informations sur l'heure à laquelle le questionnaire a été commencé, l'heure à laquelle il a été terminé, et le temps qui fut nécessaire à sa complétion. Selon les vagues, elle permet également de connaître le système d'exploitation des participant·es (Windows, MacOS, Android, iOS, Linux,...) et le browser qui fut utilisé (Edge, Safari, Chrome,...). Enfin, et très intéressant, elle permet d'accéder à la variable `count_miss`, qui indique le nombre de questions qui n'ont pas été répondue.

Selon les vagues, la fonction livre également un dictionnaire des variables, accessible sous `fichier$labels`.

## `get_answers`

Cette dernière fonction permet d'obtenir les réponses des participant·es aux questions. Sur chaque ligne est donné la réponse à une question par un·e particpant·e. Il s'agit donc d'un tableau avec un nombre de ligne important, pouvant dépasser la limite de lignes pour Microsoft Excel ou LibreOffice Calc.
