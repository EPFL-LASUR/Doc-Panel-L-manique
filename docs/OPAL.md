# OPAL

## Introduction

Les données du Panel Lémanique sont stockées sur OPAL, une application mise à disposition par OBiBa. OPAL sert à créer des référentiels de données centraux pour les études, qui intègrent sous une interface uniforme les données collectées à partir de multiples sources. Ainsi, grâce à OPAL, les études peuvent importer, valider, dériver, interroger, rapporter, analyser et exporter des données. ([Plus sur OBiBA](https://www.obiba.org/)).

Afin d'accéder aux données sur OPAL depuis une interface R, il est nécessaire d'installer le package `opalr`:

```r
# install from CRAN
install.packages("opalr")

# or install latest development version
remotes::install_github("obiba/opalr")

library(opalr)
```

Pour plus d'informations, la [documentation OPAL](https://www.obiba.org/opalr/) est la [documentation R](https://www.r-project.org/) sont à disposition.

## Login
Une fois votre mot de passe reçu, la première connexion doit se faire sur l'interface graphique d'OPAL sous [cette adresse](https://panel-lemanique-data.epfl.ch/). Afin de pouvoir y accéder en R, il vous faudra générer un *Personal Access Token* ([voir documentation OPAL](https://opaldoc.obiba.org/en/latest/web-user-guide/my-profile.html#two-factor-authentication)). Vous pouvez en créer un en allant dans *My Profile* en haut à gauche dans OPAL. N'oubliez pas de le sauvegarder quelque part, vous ne le verrez qu'une seule fois!

Une fois le Personal Access Token créé, vous pouvez ouvrir une nouvelle session dans RStudio, intaller le package `opalr` et vous connecter avec:
```r
o <- opal.login(token="votre_personal_access_token", url = "https://panel-lemanique-data.epfl.ch/")
```
Une bonne pratique est également de changer votre mot de passe pour vous créer le votre. Vous pouvez le faire dans *My Profile*, avec le bouton *Update password*.

Une fois que vous avez fini de travailler dans OPAL, n'oubliez pas de vous déconnecter avec:
```r
opal.logout(o)
```

## Accès aux données

### Valeurs

Les données OPAL sont stockées sous forme de tables/vues. Celles-ci sont organisées en projets. Les données du Panel Lémanique sont dans le projet *Panel_Lemanique*. Pour lister les projets: `opal.projects(o)`. Plus sur la gestion de projets sous [Projets OPAL](https://www.obiba.org/opalr/articles/opal-projects.html).

Les données du Panel Lémanique sont organisées par vagues, chaque vague étant stockées dans une table. La commande pour lister les tables disponibles dans un projet est `opal.tables(o, "project")`. Une table peut-être crée avec `opal.table_create()` et supprimée avec `opal.table_delete()`. Plus sur les tables sous [Tables OPAL](https://www.obiba.org/opalr/articles/opal-projects.html).

Il existe deux manières de retirer les données d'une vague. Le tableau avec l'ensemble des données d'une vague peut être obtenu avec `opal.table_get(o, "project", "table")`. Pour retirer uniquement les valeurs pour une personne, la commande `opal.valueset(o, "project", "table", identifier = "CH007")` peut être utilisée. Pour obtenir les réponses à une seule question, les données doivent d'abord être retirées avec `df <- opal.table_get()` puis les valeurs sont obtenus avec `df$Q42`.

### Tables et Vues

OPAL fait une distinction entre les tables et les vues. Les tables sont des tableaux statiques et contiennent les données brutes du projet. Elles sont crées en téléchargeant des fichiers externes sur OPAL et en les transformant en tables. Plus sur les fichiers sous [Files OPAL](https://www.obiba.org/opalr/articles/opal-files.html).

Les vues sont des tableaux dynamiques. Elles sont crées à partir de tables et permettent des modifications à l'aide de la commande `opal.execute(o, "script")`, avec un script en R. Il est également possible de télécharger ce script sur OPAL et de l'exécuter ensuite avec `opal.execute.source(o, "path/to/script")`. Plus d'informations sous [OPAL R Session](https://www.obiba.org/opalr/articles/opal-rsession.html).

### Dictionnaires

L'un des principaux avantages d'OPAL est que les dictionnaires des variables peuvent être retirés très facilement. La commande pour cela est `dico <- opal.table_dictionary_get(o, "project", "table")`. On obtient ainsi deux choses: tout d'abord, `dico$variable`, listant les variables (donc les en-tête des colonnes), et `dico$categories`, qui donne les différents labels pour une variable (e.g. 1: Homme, 2: Femme, 3: Non-binaire, 4: Ne souhaite pas répondre).

Les dictionnaires peuvent ensuite être téléchargés en format CSV. Malheureusement, les tableaux `variables` et `categories` doivent être téléchargés séparément, donnant donc deux fichiers CSV. La commande pour cela est `write.csv(dico$variable, "path/to/file")` (de même mais avec `dico$categories` pour le tableau `categories`). Il est également possible de télécharger un fichier XLSX avec deux feuilles, une pour les variables et une pour les catégories. Pour cela, le package `openxlsx` est votre ami. Plus sur les dictionnaires OPAL sous [Dictionnaires OPAL](https://www.obiba.org/opalr/articles/opal-projects.html).
