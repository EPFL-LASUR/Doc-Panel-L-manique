# R

## Installation

R est un language informatique utilisé majoritairement pour faire de l'analyse de données et des statistiques. Il est particulièrement adapté pour analyser les données du Panel Lémanique. Pour installer la dernière version, voir le site du [CRAN](https://cran.r-project.org/).

Il existe une multitude de documentation sur R. Pour la trouver, vous pouvez vous rendre sur le site du [R Project](https://www.r-project.org/) ou lire cette [documentation](https://cran.r-project.org/doc/manuals/r-release/R-intro.pdf).

R peut être tourné sur une multitude de logiciels. Il est conseillé d'utiliser RStudio, que vous pouvez télécharger sur [Posit](https://posit.co/download/rstudio-desktop/). Vous y trouverez aussi un lien pour aller télécharger la dernière version de R. Il est également possible de tourner R sur Visual Studio Code ([Download](https://code.visualstudio.com/download)). N'oubliez pas de télécharger [l'extension R](https://code.visualstudio.com/docs/languages/r) ! Et si vous voulez faire tourner R sur un autre logiciel... well, dans ce cas, je pense que vous n'avez plus besoin de documentation de ma part.

## Méthodes R

Dans ce paragraphe, quelques méthodes d’analyse en R vont être présentées. Il s’agit bien sûr d’une liste non-exhaustive, et il existe toute une documentation sur d’autres méthodes de R ([Documentation R](https://www.r-project.org/other-docs.html)). Les outils présentés dans ce chapitre sont néanmoins des classiques pour l’analyse de données tel que celles du Panel Lémanique.

-   Sélectionner une ligne pour un·e certain·e participant·e: `df <- df[df$participant_code == "FR036"]`

-   Sélectionner une certaine colonne: `df <- df$name_column`

-   Ne sélectionner que les lignes avec une certaine réponse à une question: `df <- df[df$Q29 == 3, ]`

-   Faire un tableau croisé: `table(df$variable1, df$variable2)`

-   Faire un plot entre deux variables:

``` r
plot(x, y, type="b", pch=4, col="red", xlab="titre axe X", ylab="titre axe Y", main="titre graphique")
```
Pour des paramètres plus avancés, le package `ggplot2` peut être utilisé.


-   Faire une régression linéaire (si la variable dépendante Y est continue)

``` r
modele_lin <- lm(Y ~ X, data = df)
summary(modele_lin)
```

-   Faire une régression de Poisson (si Y discrète)

``` r
modele_poisson <- glm(Y ~ X, data = df, family = poisson)
summary(modele_poisson)
```

-   Faire une régression logistique multinomiale (si Y nominale)

``` r
library(nnet)
modele_multi <- multinom(Y ~ X, data = df)
summary(modele_multi)
```

-   Faire une régression logistique ordinale (si Y ordinale)

``` r
library(MASS)
df$Y <- ordered(df$Y, levels = c("faible", "moyen", "élevé"))
modele_ord <- polr(Y ~ X, data = df, Hess = TRUE)

# Calcul des p-values
coefs <- coef(summary(modele_ord))
pval <- pnorm(abs(coefs[, "t value"]), lower.tail = FALSE) * 2
cbind(coefs, "p value" = pval)
```

-   Faire une régression logistique binomiale (si Y booléen)

``` r
modele_bin <- glm(Y ~ X, data = df, family = binomial)
summary(modele_bin)
```

-   Les variables X et Y peuvent être attribuées avec `X <- df$Q36` et `Y <- df$Q42`.

    **Attention:** les types des variables doivent être bien gérés selon le type de régression. Utilisez `factor(X)` ou `as.numeric(X)` pour attribuer le bon type aux variables.
