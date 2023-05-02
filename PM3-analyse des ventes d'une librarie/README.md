### 💡 **Compétences du projet**


| <div align="center">` Compétences acquises 💡 ` | ` Outils utilisées `
| :--- | :---: |
| Mettre à jour un catalogue de données pour rendre accessible la base de données à ses utilisateurs. |![python](https://img.icons8.com/color/28/null/python--v1.png) |
| Réaliser une analyse uni-variée de données pré-traitées afin de les décrire et de détecter les incohérences. |![python](https://img.icons8.com/color/28/null/python--v1.png) | 

<br>

# Synthèse des résultats de l'analyse.

- [Introduction](#introduction)
- [Cahier des charges](#cahier-des-charges)
- [Librairies utilisées et version](#librairies-utilisées-et-version)
- [Premier mission](#premier-mission)
- [Deuxième mission](#deuxième-mission)
- [Troisième mission](#troisième-mission)
- [Conclusion](#conclusion)


## Introduction

![fao](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/bottleneck.PNG)

Vous êtes une data analyst free lance et vous avez reussi a decrocher vôtre premier travail chez Bottleneck, un marchand de vin très prestigieux.

Laurent, le manager de cette mission, vous parle des problemes de l'entreprise : 

>Actuellement, pour gérer nos ressources, nos clients, etc., on utilise un ERP (Entreprise Resource Planning) qui n’est absolument pas relié à notre site de vente en ligne. Pour être tout à fait honnête, les outils en place sont vraiment artisanaux et dans ces conditions, la gestion des stocks est vraiment complexe et notre visibilité en termes d’analyse des ventes sur le Net est vraiment réduite, car très peu de personnes ont accès au back-office. En attendant une solution plus centralisée, un rapprochement entre les 2 bases, même manuel, pourrait être très utile…

## Cahier des charges

Premier mission:
- Rapprocher deux exports: un export de l’ERP contenant les références produit, leur prix de vente et leur état de stock, et un export d’une table de l’outil de CMS (Content Management System) contenant les informations des produits commercialisés en ligne (nom, description, nombre de ventes...)

Deuxième mission:
- Une fois le rapprochement effectué, calculer le chiffre d’affaires par produit, ainsi que le total du chiffre d’affaires réalisé en ligne.
	
Troisième mission:
- Analyser la variable "prix" afin de détecter d’éventuelles valeurs aberrantes, de les lister et d’en faire une représentation graphique pour plus de lisibilité.

## Librairies utilisées et version

```python
numpy version is 1.21.5
pandas version is 1.4.2
seaborn version is 0.11.2
natsort version is 8.1.0
```
##  Premier mission


## Deuxième mission


## Troisième mission


## Conclusion

Cette raprochement entre le fichier des ventes et le site en ligne va nous permetre d'analyser l'évolution de l'entreprise en terme de ventes.

Une donné qui à été très intéressant c'est que la plupart des chiffres d'affaires est obtenu à partir de la vente de quelques produits. Cela peut se montrer avec un coefficient de Gini de 0.805

Pour accèder au code complet [click ici](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/PM2-optimisation%20de%20la%20gestion%20des%20donn%C3%A9es%20d'une%20boutique/Ocon_Jorge_1_notebook_062022.ipynb) 

