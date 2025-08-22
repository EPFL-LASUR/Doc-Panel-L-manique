# OPAL

## Login

## Accès aux données

### Valeurs

Les données OPAL sont stockées sous forme de tables/vues. Celles-ci sont organisées en projets. Pour lister les projets: `opal.projects(o)`. Plus sur la gestion de projets sous [Projets OPAL](https://www.obiba.org/opalr/articles/opal-projects.html).

Les données du Panel Lémanique sont organisées par vagues, chaque vague étant stockées dans une table. La commande pour lister les tables disponibles dans un projet est `opal.tables(o, "project")`. Une table peut-être crée avec `opal.table_create()` et supprimée avec `opal.table_delete()`. Plus sur les tables sous [Tables OPAL](https://www.obiba.org/opalr/articles/opal-projects.html).

Il existe deux manières de retirer les données d'une vague. Le tableau avec l'ensemble des données d'une vague peut être obtenu avec `opal.table_get(o, "project", "table")`. Pour retirer uniquement les valeurs pour une personne, la commande `opal.valueset(o, "project", "table", identifier = "CH007")` peut être utilisée. Pour obtenir les réponses à une seule question, les données doivent d'abord être retirées avec `df <- opal.table_get()` puis les valeurs sont obtenus avec `df$Q42`.

### Tables et Vues

OPAL fait une distinction entre les tables et les vues. Les tables sont des tableaux statiques et contiennent les données brutes du projet. Elles sont crées en téléchargeant des fichiers externes sur OPAL et en les transformant en tables. Plus sur les fichiers sous [Files OPAL](https://www.obiba.org/opalr/articles/opal-files.html).

Les vues sont des tableaux dynamiques. Elles sont crées à partir de tables et permettent des modifications à l'aide de la commande `opal.execute(o, "script")`, avec un script en R. Il est également possible de télécharger ce script sur OPAL et de l'exécuter ensuite avec `opal.execute.source(o, "path/to/script")`. Plus d'informations sous [OPAL R Session](https://www.obiba.org/opalr/articles/opal-rsession.html).

### Dictionnaires

L'un des principaux avantages d'OPAL est que les dictionnaires des variables peuvent être retirés très facilement. La commande pour cela est `dico <- opal.table_dictionary_get(o, "project", "table")`. On obtient ainsi deux choses: tout d'abord, `dico$variable`, listant les variables (donc les en-tête des colonnes), et `dico$categories`, qui donne les différents labels pour une variable (e.g. 1: Homme, 2: Femme, 3: Non-binaire, 4: Ne souhaite pas répondre).

Les dictionnaires peuvent ensuite être téléchargés en format CSV. Malheureusement, les tableaux `variables` et `categories` doivent être téléchargés séparément, donnant donc deux fichiers CSV. La commande pour cela est `write.csv(dico$variable, "path/to/file")` (de même mais avec `dico$categories` pour le tableau `categories`). Il est également possible de télécharger un fichier XLSX avec deux feuilles, une pour les variables et une pour les catégories. Pour cela, le package `openxlsx` est votre ami. Plus sur les dictionnaires OPAL sous [Dictionnaires OPAL](https://www.obiba.org/opalr/articles/opal-projects.html).
