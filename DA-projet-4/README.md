
## Introduction

<details> 
<summary>
Click here for Introduction! 👋🏻
	
</summary>
	
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




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Zone</th>
      <th>Personnes en sous nutrition en milles</th>
      <th>Population en milles</th>
      <th>Pourcentage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>78</th>
      <td>Haïti</td>
      <td>5300.0</td>
      <td>10982.37</td>
      <td>48.26</td>
    </tr>
    <tr>
      <th>157</th>
      <td>République populaire démocratique de Corée</td>
      <td>12000.0</td>
      <td>25429.82</td>
      <td>47.19</td>
    </tr>
    <tr>
      <th>108</th>
      <td>Madagascar</td>
      <td>10500.0</td>
      <td>25570.51</td>
      <td>41.06</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Libéria</td>
      <td>1800.0</td>
      <td>4702.23</td>
      <td>38.28</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Lesotho</td>
      <td>800.0</td>
      <td>2091.53</td>
      <td>38.25</td>
    </tr>
  </tbody>
</table>
</div>



### 2eme point : Nombre théorique de personnes qui pourraient être nourries

#### Besoin alimentaire par pays
L'information se trouve dans https://www.fao.org/ dans ***Données de la sécurité alimentaire***. 

Il etais important d'avoir des chiffres officiels qui prennent en compte les differences culturelles, morphologiques et geografiques de chaque pays.

#### Importation des données


```python
besoin_alimentaires_2017 = pd.read_csv("C:/Users/ocon_/Sync/1_Data_analyst/P4_ocon_jorge/Données/besoin_energetiques_2017.csv")
```

#### Preparation des données des besoins alimentaires

Avant :


```python
besoin_alimentaires_2017.head(4)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Zone</th>
      <th>Produit</th>
      <th>Unité</th>
      <th>Valeur</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>Besoins énergétiques alimentaires minimaux (kc...</td>
      <td>Kcal/personne/jour</td>
      <td>1676</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afghanistan</td>
      <td>Besoins énergétiques alimentaires moyens (kcal...</td>
      <td>Kcal/personne/jour</td>
      <td>2134</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Afrique du Sud</td>
      <td>Besoins énergétiques alimentaires minimaux (kc...</td>
      <td>Kcal/personne/jour</td>
      <td>1859</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Afrique du Sud</td>
      <td>Besoins énergétiques alimentaires moyens (kcal...</td>
      <td>Kcal/personne/jour</td>
      <td>2406</td>
    </tr>
  </tbody>
</table>
</div>



Après :


```python
besoin_alimentaires_2017.drop(["Unité"], axis=1, inplace=True)
besoin_alimentaires_2017 = besoin_alimentaires_2017.set_index(["Zone", "Produit"], drop=True).unstack()["Valeur"].reset_index()
besoin_alimentaires_2017.head(4)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Produit</th>
      <th>Zone</th>
      <th>Besoins énergétiques alimentaires minimaux (kcal/personne/jour)</th>
      <th>Besoins énergétiques alimentaires moyens (kcal/personne/jour)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>1676</td>
      <td>2134</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afrique du Sud</td>
      <td>1859</td>
      <td>2406</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Albanie</td>
      <td>1911</td>
      <td>2490</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Algérie</td>
      <td>1781</td>
      <td>2289</td>
    </tr>
  </tbody>
</table>
</div>



#### Filtrage et jointure des données necessaires pour faire cette analyse 
- Données relatives aux besoin alimentaire;
- données relatives à la population et;
- données relatives à la disponibilité alimentaire


```python
dispo_alimentaire_prep_2 = dispo_alimentaire_prep[["Zone", "Origine", "Produit", "Disponibilité alimentaire (Kcal/personne/jour)"]].copy()
#dispo_alimentaire_prep = dispo_alimentaire_prep[dispo_alimentaire_prep["Produit"].str.contains("Boissons Alcooliques")==False]
dispo_alimentaire_prep_2 = dispo_alimentaire_prep_2[dispo_alimentaire_prep["Produit"] != "Boissons Alcooliques"]
dispo_alimentaire_prep_2 = dispo_alimentaire_prep_2.groupby(['Zone']).sum()
dispo_alimentaire_prep_2 = dispo_alimentaire_prep_2.reset_index()
```


```python
nb_personnes_nourries = pd.merge(population_prep[["Zone", "Population en milles"]], dispo_alimentaire_prep_2[["Zone","Disponibilité alimentaire (Kcal/personne/jour)"]],
                                 on="Zone")
nb_personnes_nourries = pd.merge(nb_personnes_nourries[["Zone", "Population en milles","Disponibilité alimentaire (Kcal/personne/jour)"]],
                                              besoin_alimentaires_2017[["Zone", "Besoins énergétiques alimentaires minimaux (kcal/personne/jour)", "Besoins énergétiques alimentaires moyens (kcal/personne/jour)"]], on="Zone")
```

#### Analyse statistique 
Population * dipo alimenaire / besoin min = Nombre theorique des personnes qui pourraient etre alimentées avec une ingestion de norriture minimal
Population * dipo alimenaire / besoin moy = Nombre theorique des personnes qui pourraient etre alimentées avec une ingestion de norriture moyenne


```python
nb_personnes_nourries["Disponibilité alimentaire par pays"] = nb_personnes_nourries["Population en milles"]*nb_personnes_nourries["Disponibilité alimentaire (Kcal/personne/jour)"]
nb_personnes_nourries["personnes nourries avec le min des calories par pays en milles"] = round(nb_personnes_nourries["Disponibilité alimentaire par pays"]/nb_personnes_nourries["Besoins énergétiques alimentaires minimaux (kcal/personne/jour)"],3)
nb_personnes_nourries["personnes nourries par pays en milles"] = round(nb_personnes_nourries["Disponibilité alimentaire par pays"]/nb_personnes_nourries["Besoins énergétiques alimentaires moyens (kcal/personne/jour)"],3)
nb_personnes_nourries.loc['Total']= nb_personnes_nourries.sum(numeric_only=True)

nb_personnes_nourries.head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Zone</th>
      <th>Population en milles</th>
      <th>Disponibilité alimentaire (Kcal/personne/jour)</th>
      <th>Besoins énergétiques alimentaires minimaux (kcal/personne/jour)</th>
      <th>Besoins énergétiques alimentaires moyens (kcal/personne/jour)</th>
      <th>Disponibilité alimentaire par pays</th>
      <th>personnes nourries avec le min des calories par pays en milles</th>
      <th>personnes nourries par pays en milles</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>36296.113</td>
      <td>2087.0</td>
      <td>1676.0</td>
      <td>2134.0</td>
      <td>7.574999e+07</td>
      <td>45196.890</td>
      <td>35496.714</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afrique du Sud</td>
      <td>57009.756</td>
      <td>2968.0</td>
      <td>1859.0</td>
      <td>2406.0</td>
      <td>1.692050e+08</td>
      <td>91019.341</td>
      <td>70326.249</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Albanie</td>
      <td>2884.169</td>
      <td>3167.0</td>
      <td>1911.0</td>
      <td>2490.0</td>
      <td>9.134163e+06</td>
      <td>4779.782</td>
      <td>3668.339</td>
    </tr>
  </tbody>
</table>
</div>




```python
nb_personnes_nourries.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Zone</th>
      <th>Population en milles</th>
      <th>Disponibilité alimentaire (Kcal/personne/jour)</th>
      <th>Besoins énergétiques alimentaires minimaux (kcal/personne/jour)</th>
      <th>Besoins énergétiques alimentaires moyens (kcal/personne/jour)</th>
      <th>Disponibilité alimentaire par pays</th>
      <th>personnes nourries avec le min des calories par pays en milles</th>
      <th>personnes nourries par pays en milles</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>168</th>
      <td>Viet Nam</td>
      <td>94600.648</td>
      <td>2723.0</td>
      <td>1785.0</td>
      <td>2299.0</td>
      <td>2.575976e+08</td>
      <td>1.443124e+05</td>
      <td>112047.657</td>
    </tr>
    <tr>
      <th>169</th>
      <td>Yémen</td>
      <td>27834.819</td>
      <td>2217.0</td>
      <td>1703.0</td>
      <td>2174.0</td>
      <td>6.170979e+07</td>
      <td>3.623593e+04</td>
      <td>28385.370</td>
    </tr>
    <tr>
      <th>170</th>
      <td>Zambie</td>
      <td>16853.599</td>
      <td>1924.0</td>
      <td>1696.0</td>
      <td>2159.0</td>
      <td>3.242632e+07</td>
      <td>1.911929e+04</td>
      <td>15019.141</td>
    </tr>
    <tr>
      <th>171</th>
      <td>Zimbabwe</td>
      <td>14236.595</td>
      <td>2071.0</td>
      <td>1734.0</td>
      <td>2216.0</td>
      <td>2.948399e+07</td>
      <td>1.700345e+04</td>
      <td>13305.049</td>
    </tr>
    <tr>
      <th>Total</th>
      <td>NaN</td>
      <td>7291900.830</td>
      <td>484454.0</td>
      <td>314785.0</td>
      <td>406521.0</td>
      <td>2.074540e+10</td>
      <td>1.131859e+07</td>
      <td>8762488.212</td>
    </tr>
  </tbody>
</table>
</div>



### 3eme point : Nombre théorique de personnes qui pourraient être nourries avec des produits vegetaux

#### Filtrage et jointure des données necessaires pour faire cette analyse 


```python
dispo_alimentaire_prep_vegetal = dispo_alimentaire_prep[["Zone", "Origine", "Produit", "Disponibilité alimentaire (Kcal/personne/jour)"]].copy()
#dispo_alimentaire_vegetal = dispo_alimentaire_tonnes_Kcal[dispo_alimentaire_tonnes_Kcal["Origine"].str.contains("animale")==False]
dispo_alimentaire_prep_vegetal = dispo_alimentaire_prep_vegetal[dispo_alimentaire_prep_vegetal["Origine"] != "animale"]
dispo_alimentaire_prep_vegetal = dispo_alimentaire_prep_vegetal[dispo_alimentaire_prep_vegetal["Produit"] != "Boissons Alcooliques"]
dispo_alimentaire_prep_vegetal = dispo_alimentaire_prep_vegetal.groupby(['Zone']).sum()
dispo_alimentaire_prep_vegetal = dispo_alimentaire_prep_vegetal.reset_index()
```


```python
Table_nombre_des_personnes_nourries_VEG = pd.merge(population_prep[["Zone", "Population en milles"]], dispo_alimentaire_prep_vegetal[["Zone","Disponibilité alimentaire (Kcal/personne/jour)"]],on="Zone")
Table_nombre_des_personnes_nourries_VEG1 = pd.merge(Table_nombre_des_personnes_nourries_VEG[["Zone", "Population en milles","Disponibilité alimentaire (Kcal/personne/jour)"]],
                                              besoin_alimentaires_2017[["Zone", "Besoins énergétiques alimentaires minimaux (kcal/personne/jour)", "Besoins énergétiques alimentaires moyens (kcal/personne/jour)"]], on="Zone")
```

#### Analyse statistique 

Le meme analyse que pour le 2eme point


```python
Table_nombre_des_personnes_nourries_VEG1["Disponibilité alimentaire par pays"] = Table_nombre_des_personnes_nourries_VEG1["Population en milles"]*Table_nombre_des_personnes_nourries_VEG1["Disponibilité alimentaire (Kcal/personne/jour)"]
Table_nombre_des_personnes_nourries_VEG1["Nombre theorique MIN des personnes nourries par pays"] = round(Table_nombre_des_personnes_nourries_VEG1["Disponibilité alimentaire par pays"]/Table_nombre_des_personnes_nourries_VEG1["Besoins énergétiques alimentaires minimaux (kcal/personne/jour)"],3)
Table_nombre_des_personnes_nourries_VEG1["Nombre theorique MOY des personnes nourries par pays"] = round(Table_nombre_des_personnes_nourries_VEG1["Disponibilité alimentaire par pays"]/Table_nombre_des_personnes_nourries_VEG1["Besoins énergétiques alimentaires moyens (kcal/personne/jour)"],3)
Table_nombre_des_personnes_nourries_VEG1.loc['Total']= Table_nombre_des_personnes_nourries_VEG1.sum(numeric_only=True)
```


```python
Table_nombre_des_personnes_nourries_VEG1.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Zone</th>
      <th>Population en milles</th>
      <th>Disponibilité alimentaire (Kcal/personne/jour)</th>
      <th>Besoins énergétiques alimentaires minimaux (kcal/personne/jour)</th>
      <th>Besoins énergétiques alimentaires moyens (kcal/personne/jour)</th>
      <th>Disponibilité alimentaire par pays</th>
      <th>Nombre theorique MIN des personnes nourries par pays</th>
      <th>Nombre theorique MOY des personnes nourries par pays</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>36296.113</td>
      <td>1871.0</td>
      <td>1676.0</td>
      <td>2134.0</td>
      <td>6.791003e+07</td>
      <td>40519.109</td>
      <td>31822.881</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afrique du Sud</td>
      <td>57009.756</td>
      <td>2481.0</td>
      <td>1859.0</td>
      <td>2406.0</td>
      <td>1.414412e+08</td>
      <td>76084.564</td>
      <td>58786.868</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Albanie</td>
      <td>2884.169</td>
      <td>2182.0</td>
      <td>1911.0</td>
      <td>2490.0</td>
      <td>6.293257e+06</td>
      <td>3293.175</td>
      <td>2527.412</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Algérie</td>
      <td>41389.189</td>
      <td>2914.0</td>
      <td>1781.0</td>
      <td>2289.0</td>
      <td>1.206081e+08</td>
      <td>67719.313</td>
      <td>52690.300</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Allemagne</td>
      <td>82658.409</td>
      <td>2375.0</td>
      <td>1948.0</td>
      <td>2545.0</td>
      <td>1.963137e+08</td>
      <td>100777.064</td>
      <td>77137.022</td>
    </tr>
  </tbody>
</table>
</div>




```python
Table_nombre_des_personnes_nourries_VEG1.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Zone</th>
      <th>Population en milles</th>
      <th>Disponibilité alimentaire (Kcal/personne/jour)</th>
      <th>Besoins énergétiques alimentaires minimaux (kcal/personne/jour)</th>
      <th>Besoins énergétiques alimentaires moyens (kcal/personne/jour)</th>
      <th>Disponibilité alimentaire par pays</th>
      <th>Nombre theorique MIN des personnes nourries par pays</th>
      <th>Nombre theorique MOY des personnes nourries par pays</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>168</th>
      <td>Viet Nam</td>
      <td>94600.648</td>
      <td>2148.0</td>
      <td>1785.0</td>
      <td>2299.0</td>
      <td>2.032022e+08</td>
      <td>113838.763</td>
      <td>88387.208</td>
    </tr>
    <tr>
      <th>169</th>
      <td>Yémen</td>
      <td>27834.819</td>
      <td>2028.0</td>
      <td>1703.0</td>
      <td>2174.0</td>
      <td>5.644901e+07</td>
      <td>33146.807</td>
      <td>25965.507</td>
    </tr>
    <tr>
      <th>170</th>
      <td>Zambie</td>
      <td>16853.599</td>
      <td>1818.0</td>
      <td>1696.0</td>
      <td>2159.0</td>
      <td>3.063984e+07</td>
      <td>18065.945</td>
      <td>14191.683</td>
    </tr>
    <tr>
      <th>171</th>
      <td>Zimbabwe</td>
      <td>14236.595</td>
      <td>1893.0</td>
      <td>1734.0</td>
      <td>2216.0</td>
      <td>2.694987e+07</td>
      <td>15542.027</td>
      <td>12161.496</td>
    </tr>
    <tr>
      <th>Total</th>
      <td>NaN</td>
      <td>7291900.830</td>
      <td>389613.0</td>
      <td>314785.0</td>
      <td>406521.0</td>
      <td>1.708718e+10</td>
      <td>9353202.959</td>
      <td>7244902.056</td>
    </tr>
  </tbody>
</table>
</div>



### 4eme point : l’utilisation de la disponibilité intérieure

En particulier la part qui est attribuée à l’alimentation animale, celle qui est perdue et celle qui est concrètement utilisée pour l'alimentation humaine


```python
dispo_alimentaire_prep.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Zone</th>
      <th>Produit</th>
      <th>Origine</th>
      <th>Aliments pour animaux</th>
      <th>Autres Utilisations</th>
      <th>Disponibilité alimentaire (Kcal/personne/jour)</th>
      <th>Disponibilité alimentaire en quantité (kg/personne/an)</th>
      <th>Disponibilité de matière grasse en quantité (g/personne/jour)</th>
      <th>Disponibilité de protéines en quantité (g/personne/jour)</th>
      <th>Disponibilité intérieure</th>
      <th>Exportations - Quantité</th>
      <th>Importations - Quantité</th>
      <th>Nourriture</th>
      <th>Pertes</th>
      <th>Production</th>
      <th>Semences</th>
      <th>Traitement</th>
      <th>Variation de stock</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>Abats Comestible</td>
      <td>animale</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5.0</td>
      <td>1.72</td>
      <td>0.20</td>
      <td>0.77</td>
      <td>53000.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>53000.0</td>
      <td>NaN</td>
      <td>53000.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Afghanistan</td>
      <td>Agrumes, Autres</td>
      <td>vegetale</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.29</td>
      <td>0.01</td>
      <td>0.02</td>
      <td>41000.0</td>
      <td>2000.0</td>
      <td>40000.0</td>
      <td>39000.0</td>
      <td>2000.0</td>
      <td>3000.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Afghanistan</td>
      <td>Aliments pour enfants</td>
      <td>vegetale</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>0.06</td>
      <td>0.01</td>
      <td>0.03</td>
      <td>2000.0</td>
      <td>NaN</td>
      <td>2000.0</td>
      <td>2000.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Afghanistan</td>
      <td>Ananas</td>
      <td>vegetale</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Afghanistan</td>
      <td>Bananes</td>
      <td>vegetale</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.0</td>
      <td>2.70</td>
      <td>0.02</td>
      <td>0.05</td>
      <td>82000.0</td>
      <td>NaN</td>
      <td>82000.0</td>
      <td>82000.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



Tri des colonnes qu'on va utiliser et elimination des boisons alcoholiques comme source des calories


```python
#dispo_alimentaire_prep_4 = dispo_alimentaire_prep.drop(["Disponibilité alimentaire (Kcal/personne/jour)", "Disponibilité alimentaire en quantité (kg/personne/an)", 
                                                           #"Disponibilité de matière grasse en quantité (g/personne/jour)", "Disponibilité de protéines en quantité (g/personne/jour)"], axis=1)
dispo_alimentaire_prep_4 = dispo_alimentaire_prep[dispo_alimentaire_prep["Produit"].str.contains("Boissons Alcooliques")==False].copy()
utilisation_dispo_alimentaire = dispo_alimentaire_prep[["Zone", "Disponibilité intérieure", "Aliments pour animaux",
                                                              "Nourriture", "Pertes"]]
utilisation_dispo_alimentaire = utilisation_dispo_alimentaire.groupby(["Zone"]).sum()
utilisation_dispo_alimentaire.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Disponibilité intérieure</th>
      <th>Aliments pour animaux</th>
      <th>Nourriture</th>
      <th>Pertes</th>
    </tr>
    <tr>
      <th>Zone</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Afghanistan</th>
      <td>13515000.0</td>
      <td>768000.0</td>
      <td>10735000.0</td>
      <td>1135000.0</td>
    </tr>
    <tr>
      <th>Afrique du Sud</th>
      <td>61256000.0</td>
      <td>5309000.0</td>
      <td>29812000.0</td>
      <td>2193000.0</td>
    </tr>
    <tr>
      <th>Albanie</th>
      <td>4758000.0</td>
      <td>660000.0</td>
      <td>3476000.0</td>
      <td>276000.0</td>
    </tr>
    <tr>
      <th>Algérie</th>
      <td>42630000.0</td>
      <td>4352000.0</td>
      <td>31729000.0</td>
      <td>3753000.0</td>
    </tr>
    <tr>
      <th>Allemagne</th>
      <td>162275000.0</td>
      <td>30209000.0</td>
      <td>79238000.0</td>
      <td>3781000.0</td>
    </tr>
  </tbody>
</table>
</div>



#### Analyse statistique

On ajoute le pourcentage des aliments qui ne sont pas consommées par les humaines par rapport a la disponibilité interieur du pays


```python
utilisation_dispo_alimentaire["Aliments pour animaux %"] = round(utilisation_dispo_alimentaire["Aliments pour animaux"]/utilisation_dispo_alimentaire["Disponibilité intérieure"]*100,2)
utilisation_dispo_alimentaire["Norriture %"] = round(utilisation_dispo_alimentaire["Nourriture"]/utilisation_dispo_alimentaire["Disponibilité intérieure"]*100,2)
utilisation_dispo_alimentaire["Pertes %"] = round(utilisation_dispo_alimentaire["Pertes"]/utilisation_dispo_alimentaire["Disponibilité intérieure"]*100,2)
utilisation_dispo_alimentaire["Autre %"] = round(100 - utilisation_dispo_alimentaire["Pertes %"] - utilisation_dispo_alimentaire["Norriture %"] - utilisation_dispo_alimentaire["Aliments pour animaux %"],2)

utilisation_dispo_alimentaire.head() 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Disponibilité intérieure</th>
      <th>Aliments pour animaux</th>
      <th>Nourriture</th>
      <th>Pertes</th>
      <th>Aliments pour animaux %</th>
      <th>Norriture %</th>
      <th>Pertes %</th>
      <th>Autre %</th>
    </tr>
    <tr>
      <th>Zone</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Afghanistan</th>
      <td>13515000.0</td>
      <td>768000.0</td>
      <td>10735000.0</td>
      <td>1135000.0</td>
      <td>5.68</td>
      <td>79.43</td>
      <td>8.40</td>
      <td>6.49</td>
    </tr>
    <tr>
      <th>Afrique du Sud</th>
      <td>61256000.0</td>
      <td>5309000.0</td>
      <td>29812000.0</td>
      <td>2193000.0</td>
      <td>8.67</td>
      <td>48.67</td>
      <td>3.58</td>
      <td>39.08</td>
    </tr>
    <tr>
      <th>Albanie</th>
      <td>4758000.0</td>
      <td>660000.0</td>
      <td>3476000.0</td>
      <td>276000.0</td>
      <td>13.87</td>
      <td>73.06</td>
      <td>5.80</td>
      <td>7.27</td>
    </tr>
    <tr>
      <th>Algérie</th>
      <td>42630000.0</td>
      <td>4352000.0</td>
      <td>31729000.0</td>
      <td>3753000.0</td>
      <td>10.21</td>
      <td>74.43</td>
      <td>8.80</td>
      <td>6.56</td>
    </tr>
    <tr>
      <th>Allemagne</th>
      <td>162275000.0</td>
      <td>30209000.0</td>
      <td>79238000.0</td>
      <td>3781000.0</td>
      <td>18.62</td>
      <td>48.83</td>
      <td>2.33</td>
      <td>30.22</td>
    </tr>
  </tbody>
</table>
</div>



## *2eme mission : Mélanie*

#### Trois interrogations
 - les pays pour lesquels la proportion de personnes sous-alimentées est la plus forte en 2017;
 - ceux qui ont le plus bénéficié d’aide depuis 2013;
 - ceux ayant le plus/le moins de disponibilité/habitant

#### 1er :  Les pays pour lesquels la proportion de personnes sous-alimentées est la plus forte en 2017;



```python
prop_sous_nutrition_2017.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Zone</th>
      <th>Personnes en sous nutrition en milles</th>
      <th>Population en milles</th>
      <th>Pourcentage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>78</th>
      <td>Haïti</td>
      <td>5300.0</td>
      <td>10982.37</td>
      <td>48.26</td>
    </tr>
    <tr>
      <th>157</th>
      <td>République populaire démocratique de Corée</td>
      <td>12000.0</td>
      <td>25429.82</td>
      <td>47.19</td>
    </tr>
    <tr>
      <th>108</th>
      <td>Madagascar</td>
      <td>10500.0</td>
      <td>25570.51</td>
      <td>41.06</td>
    </tr>
    <tr>
      <th>103</th>
      <td>Libéria</td>
      <td>1800.0</td>
      <td>4702.23</td>
      <td>38.28</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Lesotho</td>
      <td>800.0</td>
      <td>2091.53</td>
      <td>38.25</td>
    </tr>
  </tbody>
</table>
</div>



#### 2eme : Les pays qui ont le plus bénéficié d'aide depuis 2013


```python
les_plus_beneficies_aide_alimentaire = aide_alimentaire_prep.copy()
les_plus_beneficies_aide_alimentaire["Année"] = pd.to_numeric(les_plus_beneficies_aide_alimentaire["Année"], errors='coerce')
```


```python
les_plus_beneficies_aide_alimentaire = les_plus_beneficies_aide_alimentaire.query("Année >= 2013").groupby("Pays bénéficiaire").sum().drop(["Année"], axis=1).reset_index()
les_plus_beneficies_aide_alimentaire.sort_values(by=["Tonnes"], inplace=True, ascending=False)
les_plus_beneficies_aide_alimentaire.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Pays bénéficiaire</th>
      <th>Tonnes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>50</th>
      <td>République arabe syrienne</td>
      <td>1858943</td>
    </tr>
    <tr>
      <th>75</th>
      <td>Éthiopie</td>
      <td>1381294</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Yémen</td>
      <td>1206484</td>
    </tr>
    <tr>
      <th>61</th>
      <td>Soudan du Sud</td>
      <td>695248</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Soudan</td>
      <td>669784</td>
    </tr>
  </tbody>
</table>
</div>



#### 3eme : Les pays ayant le plus/le moins de disponibilité/habitant


```python
dispo_alimentaire_prep_le_moins = dispo_alimentaire_prep[["Zone", "Disponibilité alimentaire (Kcal/personne/jour)"]]
dispo_alimentaire_prep_le_moins = dispo_alimentaire_prep_le_moins.groupby("Zone").sum()
dispo_alimentaire_prep_le_moins.sort_values(by=["Disponibilité alimentaire (Kcal/personne/jour)"], ascending=True)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Disponibilité alimentaire (Kcal/personne/jour)</th>
    </tr>
    <tr>
      <th>Zone</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>République centrafricaine</th>
      <td>1879.0</td>
    </tr>
    <tr>
      <th>Zambie</th>
      <td>1924.0</td>
    </tr>
    <tr>
      <th>Madagascar</th>
      <td>2056.0</td>
    </tr>
    <tr>
      <th>Afghanistan</th>
      <td>2087.0</td>
    </tr>
    <tr>
      <th>Haïti</th>
      <td>2089.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>Israël</th>
      <td>3610.0</td>
    </tr>
    <tr>
      <th>États-Unis d'Amérique</th>
      <td>3682.0</td>
    </tr>
    <tr>
      <th>Turquie</th>
      <td>3708.0</td>
    </tr>
    <tr>
      <th>Belgique</th>
      <td>3737.0</td>
    </tr>
    <tr>
      <th>Autriche</th>
      <td>3770.0</td>
    </tr>
  </tbody>
</table>
<p>174 rows × 1 columns</p>
</div>



#### Julien


```python
cereales_2017 = pd.read_csv("C:/Users/ocon_/Sync/1_Data_analyst/P4_ocon_jorge/Données/cereales_total_2017.csv")
```


```python
cereales_2017 = cereales_2017.set_index(["Zone", "?l?ment"], drop=True).unstack()["Valeur"].reset_index()
cereales_2017['Proportion aliments pour animaux %'] = round(cereales_2017['Aliments pour animaux']/cereales_2017['Disponibilit? int?rieure']*100,2)
cereales_2017['Proportion aliments pour norriture humaine %'] = round(cereales_2017['Nourriture']/cereales_2017['Disponibilit? int?rieure']*100,2)
cereales_2017 = cereales_2017.sort_values(by=["Proportion aliments pour animaux %"], inplace=False, ascending=False)
cereales_2017.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>?l?ment</th>
      <th>Zone</th>
      <th>Aliments pour animaux</th>
      <th>Disponibilit? int?rieure</th>
      <th>Nourriture</th>
      <th>Proportion aliments pour animaux %</th>
      <th>Proportion aliments pour norriture humaine %</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>51</th>
      <td>Danemark</td>
      <td>7171</td>
      <td>8591</td>
      <td>697</td>
      <td>83.47</td>
      <td>8.11</td>
    </tr>
    <tr>
      <th>79</th>
      <td>Irlande</td>
      <td>3253</td>
      <td>4035</td>
      <td>592</td>
      <td>80.62</td>
      <td>14.67</td>
    </tr>
    <tr>
      <th>129</th>
      <td>Pays-Bas</td>
      <td>9909</td>
      <td>13604</td>
      <td>1626</td>
      <td>72.84</td>
      <td>11.95</td>
    </tr>
    <tr>
      <th>55</th>
      <td>Espagne</td>
      <td>22618</td>
      <td>31235</td>
      <td>5227</td>
      <td>72.41</td>
      <td>16.73</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Chypre</td>
      <td>432</td>
      <td>626</td>
      <td>170</td>
      <td>69.01</td>
      <td>27.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = prop_sous_nutrition_2017.loc[prop_sous_nutrition_2017['Zone'] == 'france']
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Zone</th>
      <th>Personnes en sous nutrition en milles</th>
      <th>Population en milles</th>
      <th>Pourcentage</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>




```python

```

