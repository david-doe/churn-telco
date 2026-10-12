# Prédire et limiter le churn d'un opérateur télécom

Roman Delbare et David Doe · Master SIAD, Université de Lille · cours de programmation R, 2025-2026

Un opérateur télécom perd chaque année une partie de ses clients. Peut-on repérer à l'avance ceux qui vont partir, et savoir sur quoi agir pour les garder ? Le projet construit un score de churn par régression logistique sur 7 043 clients, puis le traduit en règles simples pour les équipes métier.

## Résultats

| | |
|---|---|
| Données | 7 043 clients, 21 variables, 26,5 % de départs (jeu public *Telco Customer Churn*, IBM) |
| Modèle | Régression logistique, 17 variables, découpage 70 % apprentissage / 30 % test |
| Règle de décision | Un client est signalé à risque quand sa probabilité de départ dépasse 30 % |
| Sur l'échantillon test | 78 % des clients partis repérés (rappel), 53 % de précision, 76 % de bonnes prédictions |

**Ce qui pèse le plus sur le départ** (odds ratios, toutes choses égales par ailleurs) :

| Facteur | Odds ratio | Effet sur les chances de départ |
|---|---|---|
| Contrat de deux ans (vs mensuel) | 0,24 | −76 % |
| Pas d'internet (vs fibre) | 0,26 | −74 % |
| Contrat d'un an (vs mensuel) | 0,53 | −47 % |
| Internet DSL (vs fibre) | 0,55 | −45 % |
| Paiement par carte bancaire | 0,63 | −37 % |
| Sécurité en ligne | 0,64 | −36 % |
| Assistance technique | 0,66 | −34 % |
| Facturation électronique | 1,45 | +45 % |
| Streaming TV | 1,37 | +37 % |
| Chaque mois d'ancienneté | 0,95 | −5 % |

Être senior (p = 0,07) et avoir des personnes à charge (p = 0,05) ne sont pas significatifs au seuil de 5 %.

**Recommandations** : pousser les contrats longs, développer les options de sécurité et d'assistance, et suivre de près la première année, surtout pour les clients en fibre, en contrat mensuel et en facturation électronique.

## Contenu du dépôt

| Fichier | Rôle |
|---|---|
| `churn_telco.Rmd` | Tout le code R commenté : nettoyage, recodage, statistiques descriptives, modèles, choix du seuil, odds ratios. |
| `donnees/telco_customer_churn.csv` | Le jeu de données d'origine. |
| `rapport/rapport_metier.pdf` | Le rapport métier, écrit pour un lecteur sans formation en statistique. |

## Relancer l'analyse

Il faut R (4.3 ou plus) avec les packages `rmarkdown`, `dplyr`, `caret`, `ggplot2` et `questionr`.

```r
install.packages(c("rmarkdown", "dplyr", "caret", "ggplot2", "questionr"))
rmarkdown::render("churn_telco.Rmd")
```

Le découpage apprentissage / test utilise `set.seed(42)` : on retrouve les mêmes chiffres à chaque exécution.

## Limites

- Un odds ratio décrit un lien, pas une cause : proposer un contrat plus long ne garantit pas qu'un client restera.
- Avec 53 % de précision, environ un client signalé sur deux serait resté. Le score sert à prioriser une action peu coûteuse (une offre, un appel), pas à décider client par client.
- L'ancienneté et le montant total facturé sont très corrélés (0,82) et restent tous les deux dans le modèle : leurs coefficients sont à lire avec prudence.
- Les données sont une photo à un instant donné, sans suivi dans le temps.
- Le rapport métier a été rédigé avant la vérification sur l'échantillon test : il annonce 77 % de rappel et 55 % de précision, mesurés sur l'apprentissage. Les chiffres sur le test (78 % et 53 %) sont dans le Rmd, section 6.3.1.4.
