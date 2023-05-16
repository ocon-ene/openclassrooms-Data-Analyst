
### 💡 **Compétences du projet**


| <div align="center">` Compétences acquises 💡 ` | ` Outils utilisées `
| :--- | :---: |
| Créer un tableau de bord (dashboard) permettant de visualiser et de mesurer les résultats obtenus en fonction d’indicateurs donnés. |![tableau](https://img.icons8.com/color/28/null/tableau-software.png) |
| Mettre en œuvre une méthode d’apprentissage non supervisée pour opérer des classifications automatiques et partitionner des données.  |![python](https://img.icons8.com/color/28/null/python--v1.png)<br><ul><li>*Regression lineaire*</li><li>*ACP*</li><li>*Clustering hiérarchique*</li><li>*K-means*</li></ul> |

<br>

# Synthèse des résultats de l'analyse.

- [Introduction](#introduction)
- [Cahier des charges](#cahier-des-charges)
- [Librairies utilisées et version](#librairies-utilisées-et-version)
- [Analyse](#analyse)
- [Conclusion](#conclusion)


## Introduction

![books](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/p8logo.PNG)

Dans un objectif de développent, La poule qui chante, une entreprise française d’agroalimentaire, souhaite étendre son marché à international.
Le but c'est de réaliser un étude de marché et selectionner les meilleurs pays pour ce developpement.

## Cahier des charges

1. Présentation sur le contexte du projet, la démarche et les résultats.

## Librairies utilisées et version

```python
numpy version is 1.21.5
pandas version is 1.4.2
seaborn version is 0.11.2
natsort version is 8.1.0
```
## Analyse 

La première partie de l'analyse consistait à sélectionner les bons indicateurs et les mettre sur la même échelle, nous avons sélectionnées les suivants :
1. Disponibilité alimentaire en quantité (kg/personne/an)*
2. Disponibilité intérieure*
3. Exportations - Quantité*
4. Importations - Quantité*
5. Production*
6. Valeur US $ par habitant (PIB par pays par habitant)
7. Le PIB par pays en 2017
8. La population par pays en 2017
*Du poulet


Après on réalise l'analyse en composants principaux (ACP) pour pouvoir représenter l'ensemble des variables dans un plan à deux dimensions.
Dans l'image suivant on peut voir le pourcentage de représentativité (ou de variance) de chaque variable sur les composants f# : 

![books](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/p8acp1.PNG)

Nous allons choisir f1 et f2 pour représenter les pays parce qu’ils expliquent bien la partie exportation et importations de poulet.

Ensuite nous faisons la classification ascendante hiérarchique.
Cette méthode nous permet de rassembler les pays avec des caractéristiques similaires dans le même groupe.

Voici l'image des premiers 3 groupes des pays :

![books](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/p8ach.PNG)

Une fois réunis par groupes, nous devons choisir la profondeur, la profondeur c'est la quantité des groupes que nous allons représenter dans le graphique
d'ACP. Pour sélectionner la quantité des groupes nous utilisons la méthode du silhouette score.

Nous avons donc choisi 4 groupes, et voici la représentation sur le plan à deux dimensions :

![books](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/p8acp.PNG)

Voici la représentation de chaque variable sur les 4 groupes :

![books](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/p8resultats.PNG)

## Conclusion

Trois données sont importantes, l'importation de poulet, la quantité d'argent du pays et la production.
Un pays qui ne produit pas beaucoup de poulet, qui importe beaucoup et où ses habitants sont capables de dépenser de l'argent c'est un pays intéressant pour développer l'entreprise de vente de poulet.

Selon les boxplots, le cluster qui a ses caractéristiques c'est le cluster c

Voici la liste des pays :

![books](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/p8resultatstable.PNG)

Link to tableau ![viz tableau](https://public.tableau.com/views/Clusteringpoulet/Dashboard2?:language=fr-FR&publish=yes&:display_count=n&:origin=viz_share_link)
		    

Pour accéder au code complet [click ici](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/PM4-%C3%A9tude%20de%20march%C3%A9/Ocon_2_clustering_visualisations_012023.ipynb) 

Pour accéder à la présentation ppt [click ici](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/PM4-%C3%A9tude%20de%20march%C3%A9/Ocon_3_presentations_012023.pdf)

