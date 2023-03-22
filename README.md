
![OC](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/OC%20logo.PNG)

## A propos de Openclassrooms

OpenClassrooms rend l’éducation accessible à tous en révolutionnant la pédagogie grâce à une approche professionnalisante unique centrée sur les compétences numériques et du mentorat individuel. OpenClassrooms est la plateforme leader en Europe d'éducation en ligne et la seule au monde à délivrer des diplômes de niveau reconnu par l’Etat (de niveau bac+2 à +5) complètement en ligne. 

## Objectifs et contexte de la certification :

Face à l’explosion du volume d’informations et à la multiplication des sources disponibles, les entreprises sont amenées à s'appuyer sur le Big Data pour anticiper le comportement de leurs clients, optimiser leur modèle opérationnel, et informer les décisions stratégiques. 

L’analyse de données massives s’impose aujourd’hui comme une véritable nécessité pour rester compétitif dans la quasi-totalité des secteurs professionnels, y compris le marketing, la publicité, la commerce, l’assurance ou encore la médecine. Le volume considérable de données, aux formats divers, nécessite toutefois de recruter de nouveaux profils qui sachent les “faire parler” ou, en d’autres termes, qui aient les compétences techniques et mathématiques pour agréger, interpréter et communiquer des connaissances extraites d’un jeu de données. Ces compétences correspondent au profil du data analyst.  

## Niveau de qualification atteint 

Niveau 6 - équivalent Master 1
Code(s) NSF :

    114 : Mathématiques
    326 : Informatique, traitement de l'information, réseaux de transmission

Formacode(s) :

    31025 : Data analytics

## Pédagogie

La formation s’est basée sur la pratique, avec des projets professionnalisants et l’aide d’un tuteur expert dans le métier qui partageait ses expériences avec nous.

![REP](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/REPARTITION.png)

# Partenariats

Nous avons eu la chance d'avoir des cours theoriques de qualité (comme la [statistiques inférentielle](https://openclassrooms.com/fr/course-certificates/9349576468) ou l’algèbre relationnelle(lien)) qui ont été faites en partenariat avec l’ENSAE.

![ENSAE](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/ENSAE.png)

**👩🏻‍💼 THE SITUATION** 

You’ve just been hired as an eCommerce Database Analyst for Maven Fuzzy Factory, an online retailer which has just launched their first product.

**📈 THE BRIEF**

As a member of the startup team, you will work with the CEO, the Head of Marketing, and the Website Manager to help steer the business. 

You will analyze and optimize marketing channels, measure and test website conversion performance, and use data to understand the impact of new product launches.

**✏️ THE OBJECTIVE**

Use SQL to:
- Access and explore the Maven Fuzzy Factory database
- Become the data expert for the company, and the go-to person for mission critical analyses
- Analyze and optimize the business’ marketing channels, website, and product portfolio

</details> 
	
***

## Table of Contents

- [Preparation des données](#preparation-des-données)
- [1er mission](#1er-mission)
- [Analyzing Traffic Sources](#analyzing-traffic-sources)
- [Analyzing Website Performances](#analyzing-website-performances)
- [Mid-Course Project](#mid-course-project)
- [Analysis for Channel Portfolio Management](#analysis-for-channel-portfolio-management)
- [Analyzing Business Patterns and Seasonality](#analyzing-business-patterns-and-seasonality)
- [Product Analysis](#product-analysis)
- [User Analysis](#user-analysis)
- [Final Project](#final-project)


# Master-DA-projets
Les projets plus significatives du Master en DA

<details>
<summary>
  
## *Preparation des données*

</summary>

***
  
### Importation des librairies 


```python
import numpy as np
import pandas as pd
import seaborn as sns
```
## Analyzing Traffic Sources


### Importation des fichiers csv



```python
aide_alimentaire = pd.read_csv("C:/Users/ocon_/Sync/1_Data_analyst/P4_ocon_jorge/Données/aide_alimentaire.csv")
dispo_alimentaire = pd.read_csv("C:/Users/ocon_/Sync/1_Data_analyst/P4_ocon_jorge/Données/dispo_alimentaire.csv")
population = pd.read_csv("C:/Users/ocon_/Sync/1_Data_analyst/P4_ocon_jorge/Données//population.csv")
sous_nutrition = pd.read_csv("C:/Users/ocon_/Sync/1_Data_analyst/P4_ocon_jorge/Données/sous_nutrition.csv")
```

### Copy des donées 
Pour n'est pas alteré les données d'origine


```python
aide_alimentaire_prep = aide_alimentaire.copy()
dispo_alimentaire_prep = dispo_alimentaire.copy()
population_prep = population.copy() 
sous_nutrition_prep = sous_nutrition.copy()
```

### Correction des data types
D'un autre cote, le data type pour certains quantité n'etais pas correcte, par example la colonne qui exprime la quantite des personnes en sous nutrition etais classé comme etant du texte alors que il s'agissait d'une quantité.



```python
sous_nutrition_prep["Valeur"] = pd.to_numeric(sous_nutrition_prep["Valeur"], errors='coerce')
population_prep["Année"] = population_prep["Année"].astype(str)
aide_alimentaire_prep["Année"] = aide_alimentaire_prep["Année"].astype(str)
```

### Homogeanisation des unitées 
Les unitées n'etaient pas egaux pour le meme type de donnée, il y a une population exprimé en millions et une autre exprimée en milles, une quantité de norriture exprimé en tonnes et une autre exprimé en milliers des tonnes. 


```python
dispo_alimentaire_prep = dispo_alimentaire_prep * [1, 1, 1, 1000, 1000, 1, 1, 1, 1, 1000, 1000, 1000, 1000, 1000, 1000, 1000, 1000, 1000]
sous_nutrition_prep["Valeur"] *= 1000
```

L'homogenisation des unitées inclus le changement de noms des colonnes pour les rendre plus clairs.


```python
sous_nutrition_prep.rename(columns={"Valeur": "Personnes en sous nutrition en milles"}, inplace=True)
population_prep.rename(columns={"Valeur": "Population en milles"}, inplace=True)
aide_alimentaire_prep.rename(columns={"Valeur": "Tonnes"}, inplace=True)
``` 
</details>
  
## *1er mission*



### 4 points principaux

Informations pour l’année 2017 :

- la proportion de personnes en état de sous-nutrition ;

- le nombre théorique de personnes qui pourraient être nourries. Tu devrais pouvoir calculer ça à partir de la disponibilité alimentaire mondiale ;

- idem pour la disponibilité alimentaire des produits végétaux ;

- l’utilisation de la disponibilité intérieure, en particulier la part qui est attribuée à l’alimentation animale, celle qui est perdue et celle qui est concrètement utilisée pour l'alimentation humaine. Je crois que Julien avait trouvé un moyen de facilement calculer ces proportions



#### Tri pour avoir les données relatives à l'année 2017


```python
sous_nutrition_prep = sous_nutrition_prep[sous_nutrition_prep['Année'] == '2016-2018'] 
population_prep = population_prep[population_prep['Année'] == '2017']
```

### 1er point : Creation de table qui montre la proportion des personnes en etat de sous nutrition en 2017


```python
prop_sous_nutrition_2017 = pd.merge(sous_nutrition_prep[["Zone", "Personnes en sous nutrition en milles"]], population_prep[["Zone","Population en milles"]],on="Zone")
prop_sous_nutrition_2017['Pourcentage'] = prop_sous_nutrition_2017['Personnes en sous nutrition en milles']/prop_sous_nutrition_2017['Population en milles']*100
prop_sous_nutrition_2017 = prop_sous_nutrition_2017.sort_values(by=["Pourcentage"], inplace=False, ascending=False).round(2)
```


```python
prop_sous_nutrition_2017.head()
```
