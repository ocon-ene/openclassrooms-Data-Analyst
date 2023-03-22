
### 💡 **Competences du projet**


| <div align="center">` Competences adquises 💡 ` | ` outils utilsées `
| :--- | :---: |
| Pré-traiter des données stockées en les classifiant et en corrigeant les erreurs et les incohérences présentes. |![python](https://img.icons8.com/color/28/null/python--v1.png) |
| Manipulation des data frames |![python](https://img.icons8.com/color/28/null/python--v1.png) | 

<br>

## Resumé des résultats de la mission

- [Introduction](#introduction)
- [Resumé des demandes de l'équipe](#resumé-des-demandes-de-l-équipe)
- [Libreries utilisées et version](#libreries-utilisées-et-version)
- [Les pays avec des problemes de sous-nutrition](#les-pays-avec-des-problemes-de-sous-nutrition)
- [Nombre theroique des personnes qui peuvent être nourris](#nombre-theroique-des-personnes-qui-peuvent-être-nourris)
- [Conclusion](#conclusion)


## Introduction

![fao](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/FAO.PNG)

Vous avez récemment rejoint une équipe de chercheurs de la Food and Agriculture Organization of the United Nations (FAO), en tant que data analyst.

Les chercheurs sont au milieu d'une recherche sur le theme de la sous-nutrition et pour pouvoir mener à bein la recherche ils ont besoin de quelques informations suplementaires que seulement un data analyste peut fournir.

## Resumé des demandes de l équipe 

Pour l'année 2017:
- La proportion de personnes en état de sous-nutrition
- Le nombre théorique de personnes qui pourraient être nourries.
- Le nombre théorique de personnes qui pourraient être nourries avec des produits vegetaux.
- L’utilisation de la disponibilité intérieure, en particulier la part qui est attribuée à l’alimentation animale, celle qui est perdue et celle qui est concrètement utilisée pour l'alimentation humaine. 

Demandes suplementaires
- Les pays pour lesquels la proportion de personnes sous-alimentées est la plus forte en 2017
- Les pays qui ont le plus bénéficié d’aide depuis 2013
- Les pays ayant le plus/le moins de disponibilité alimentaire/habitant,

## Libreries utilisées et version

```python
numpy version is 1.21.5
pandas version is 1.4.2
seaborn version is 0.11.2
```
## Les pays avec des problemes de sous-nutrition

D'abord nous pouvons parler des pays qui ont le plus de personnes en sous nutrition en proportion à leur population.
Malhereusement Haiti est au top mundial du classement en 2017, suivi de la corée du nord. 
Dans cette table on peut observer une tendance, les pays qui ont beaucoup des problemes politiques ou qu'on suivie des guerres, sont les pays qui ont les plus de problemes a nourrir leur population.

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

## Nombre theroique des personnes qui peuvent être nourris

Ensuite nous allons analyser les nombre theorique des personnes qui pourraient etre nourris avec la production actuelle de norriture.

D'abord il faut savoir que les besoin energetiques diferent entre les pays, il y a des pays qui ont une structure morfologique plus petite, ou qui ont des depenses plus important du fait de leur clima. On a donc importé les fiches de données officielles sur les besoin energetiques par pays : 

```python
besoin_alimentaires_2017 = pd.read_csv("C:/Users/ocon_/Sync/1_Data_analyst/P4_ocon_jorge/Données/besoin_energetiques_2017.csv")
```
Quelle conclusion en tirer donc sur ce point ? Deux
1. La production actuelle permet de nourrir l'ensemble de la population mundial, et donc nous avons un probleme d'inegalité dans l'accès à la norriture et/ou un probleme de distribution
2. Une diete exclusivement vegetarienne pour tout la plannette serait difficiele avec les moyennes actuelles, mais atteigable si l'on destinée la norriture d'animaux pour nous

	<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>?l?ment</th>
      <th>Zone</th>
      <th>Aliments pour animaux</th>
      <th>Disponibilité intérieure</th>
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
	
Pour finir, nous ne pouvons pas fermer les yeux face a l'absurde quantité de norriture qui est jetée avec plus de 10%.

## Conclusion

Un gros pourcentage de la norriture est soit destinée a la consommation des animaux, soit jeté a la poubelle pendant que des pays pauvres sont en difficulté et n'arrivent pas a nourrir l'ensemble de ça population.

Pour acceder au code complet [click ici](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/PM1-%C3%A9tude%20de%20sant%C3%A9%20publique/Ocon_Jorge_1_P4_042022.ipynb) 
