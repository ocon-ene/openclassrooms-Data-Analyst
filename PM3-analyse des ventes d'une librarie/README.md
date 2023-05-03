### 💡 **Compétences du projet**


| <div align="center">` Compétences acquises 💡 ` | ` Outils utilisées `
| :--- | :---: |
| Créer les graphiques permettant de représenter les résultats obtenus. |![python](https://img.icons8.com/color/28/null/python--v1.png) |
| Réaliser une analyse multi-variée pour comprendre la corrélation entre des variables et les synthétiser.   |![python](https://img.icons8.com/color/28/null/python--v1.png)<br><ul><li>*CHI-2, R<sup>2</sup>, ANOVA*</li>|
| Analyser des séries temporelles pour mesurer un phénomène au cours du temps. |![python](https://img.icons8.com/color/28/null/python--v1.png) |
| Réaliser un test statistique afin de vérifier une prédiction.  |![python](https://img.icons8.com/color/28/null/python--v1.png)<br><ul><li>*Test de Man Whitneyu*</li><li>*Test de Bartlett*</li><li>*Test de shapiro*</li></ul> |

<br>

# Synthèse des résultats de l'analyse.

- [Introduction](#introduction)
- [Cahier des charges](#cahier-des-charges)
- [Librairies utilisées et version](#librairies-utilisées-et-version)
- [Analyse les indicateurs de vente](#analyse-les-indicateurs-de-vente)
- [Analyse sur les clients](#Analyse-sur-les-clients)
- [Conclusion](#conclusion)


## Introduction

![books](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/lapage.PNG)

Lapage était originellement une librairie physique avec plusieurs points de vente. Mais devant le succès de certains de ses produits et l’engouement de ses clients, elle a décidé depuis 2 ans d’ouvrir un site de vente en ligne. Vous intervenez car l’entreprise souhaite faire le point après deux ans d’exercice, et pouvoir analyser ses points forts, ses points faibles, les comportements clients, etc.
	
Trois tableaux ont été fournis :
customers : Avec l'information des clients
products : Avec l'informations des produits
transactions : Avec l'information des transactions

## Cahier des charges

Une analyse des différents indicateurs de vente :
- Graphiques autour du CA (chifre d'affaires)
- Regarder l’évolution dans le temps et mettre en place une décomposition en moyenne mobile
- Top et flops des ventes et répartition par catégorie
- Répartition du CA entre les clients

Une analyse ciblée sur les clients
- le lien entre le genre d’un client et les catégories des livres achetés
- le lien entre l’âge des clients et le montant total des achats, la fréquence d’achat, la taille du panier moyen et les catégories des livres achetés.
	
## Librairies utilisées et version

```python
numpy version is 1.21.5
pandas version is 1.4.2
seaborn version is 0.11.2
natsort version is 8.1.0
```
## Analyse les indicateurs de vente

Après avoir fait un nettoyage des trois tableaux, nous avons procedé à joindre les tableaux pour liér les inforations referentes aux produits, clients, et transactions.
	
A ce tableaux nous avons ajoutée quelques colonnes pour pouvoir faire un analyse plus pertinent. 
Les colonnes comme age, mois, année, tranche d'age, et Année de reference pour le CA ont été ajoutées.

	
```python
def age_tranche_maker(val):
    """
    Applies a formula that gives any given age a number between 1 and 3 
    """
    x = int((val-1)*3/90)+1
    if x == 1 :
        return '18-30'
    elif x == 2:
        return '31-60'
    else:
        return '60 ou plus'
    
customers_achats["date"] = customers_achats['date'].dt.round("30min")
customers_achats['age'] = customers_achats['date'].dt.year - customers_achats['birth']
customers_achats["jour_de_la_semaine"] = customers_achats['date'].dt.day_name()
customers_achats['mois_année'] = customers_achats['date'].dt.strftime('%Y-%m')
customers_achats["Jour_du_mois"] = customers_achats['date'].dt.day

customers_achats['tranche_dage'] = [ age_tranche_maker(age) for age in customers_achats["age"]]

date_time_str = '22/03/01 00:00:00'

date_time_obj = datetime.strptime(date_time_str, '%y/%m/%d %H:%M:%S')

customers_achats['Année_CA'] = np.where(customers_achats['date'] <= date_time_obj , '1', '2')
	
```

Voici le tableaux resultant :


```python
customers_achats
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>client_id</th>
      <th>sex</th>
      <th>birth</th>
      <th>id_prod</th>
      <th>price</th>
      <th>categ</th>
      <th>date</th>
      <th>session_id</th>
      <th>age</th>
      <th>jour_de_la_semaine</th>
      <th>mois_année</th>
      <th>Jour_du_mois</th>
      <th>tranche_dage</th>
      <th>Année_CA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>c_4410</td>
      <td>f</td>
      <td>1967</td>
      <td>1_385</td>
      <td>25.99</td>
      <td>1</td>
      <td>2021-03-22 01:30:00</td>
      <td>s_9707</td>
      <td>54</td>
      <td>Monday</td>
      <td>2021-03</td>
      <td>22</td>
      <td>31-60</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>c_4410</td>
      <td>f</td>
      <td>1967</td>
      <td>1_385</td>
      <td>25.99</td>
      <td>1</td>
      <td>2023-02-22 01:30:00</td>
      <td>s_345108</td>
      <td>56</td>
      <td>Wednesday</td>
      <td>2023-02</td>
      <td>22</td>
      <td>31-60</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>c_4410</td>
      <td>f</td>
      <td>1967</td>
      <td>0_1110</td>
      <td>4.71</td>
      <td>0</td>
      <td>2021-11-04 16:30:00</td>
      <td>s_114715</td>
      <td>54</td>
      <td>Thursday</td>
      <td>2021-11</td>
      <td>4</td>
      <td>31-60</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>c_4410</td>
      <td>f</td>
      <td>1967</td>
      <td>0_1111</td>
      <td>19.99</td>
      <td>0</td>
      <td>2023-02-22 01:30:00</td>
      <td>s_345108</td>
      <td>56</td>
      <td>Wednesday</td>
      <td>2023-02</td>
      <td>22</td>
      <td>31-60</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>c_4410</td>
      <td>f</td>
      <td>1967</td>
      <td>0_1111</td>
      <td>19.99</td>
      <td>0</td>
      <td>2021-03-22 01:30:00</td>
      <td>s_9707</td>
      <td>54</td>
      <td>Monday</td>
      <td>2021-03</td>
      <td>22</td>
      <td>31-60</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>679106</th>
      <td>c_84</td>
      <td>f</td>
      <td>1982</td>
      <td>0_1438</td>
      <td>9.31</td>
      <td>0</td>
      <td>2022-05-29 06:00:00</td>
      <td>s_215697</td>
      <td>40</td>
      <td>Sunday</td>
      <td>2022-05</td>
      <td>29</td>
      <td>31-60</td>
      <td>2</td>
    </tr>
    <tr>
      <th>679107</th>
      <td>c_84</td>
      <td>f</td>
      <td>1982</td>
      <td>0_1438</td>
      <td>9.31</td>
      <td>0</td>
      <td>2022-03-29 06:00:00</td>
      <td>s_186172</td>
      <td>40</td>
      <td>Tuesday</td>
      <td>2022-03</td>
      <td>29</td>
      <td>31-60</td>
      <td>2</td>
    </tr>
    <tr>
      <th>679108</th>
      <td>c_84</td>
      <td>f</td>
      <td>1982</td>
      <td>1_459</td>
      <td>15.99</td>
      <td>1</td>
      <td>2022-12-17 00:30:00</td>
      <td>s_313173</td>
      <td>40</td>
      <td>Saturday</td>
      <td>2022-12</td>
      <td>17</td>
      <td>31-60</td>
      <td>2</td>
    </tr>
    <tr>
      <th>679109</th>
      <td>c_84</td>
      <td>f</td>
      <td>1982</td>
      <td>1_459</td>
      <td>15.99</td>
      <td>1</td>
      <td>2022-02-17 00:30:00</td>
      <td>s_166337</td>
      <td>40</td>
      <td>Thursday</td>
      <td>2022-02</td>
      <td>17</td>
      <td>31-60</td>
      <td>1</td>
    </tr>
    <tr>
      <th>679110</th>
      <td>c_84</td>
      <td>f</td>
      <td>1982</td>
      <td>1_110</td>
      <td>54.90</td>
      <td>1</td>
      <td>2021-09-13 21:30:00</td>
      <td>s_89478</td>
      <td>39</td>
      <td>Monday</td>
      <td>2021-09</td>
      <td>13</td>
      <td>31-60</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>679111 rows × 14 columns</p>
</div>

Une fois le tableaux complet, nous n'avons qu'a créer les graphiques pour montrer la repartition des CA.


![images](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/p6_caanne.PNG)
	
	
![images](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/p6_cacateg.PNG)
	

![images](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/p6_cacategmois.PNG)
	

![images](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/p6_cl.PNG)
	

![images](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/p6_linealPNG.PNG)
	


	

## Analyse sur les clients
	
Pour faire l'analyse des clients nous avons étudier les possibles relations ou correlations entre le profil des clients et les produits achetés, pour faire ça nous noous sommes posés de differents questiones, qu'on va essayer de répondre au fur et au mesure :

1. Estce que le sex a une influence sur la quantité d'argent depensé par les clients ?


## Conclusion

Cette raprochement entre le fichier des ventes et le site en ligne va nous permetre d'analyser l'évolution de l'entreprise en terme de ventes.

Une donné qui à été très intéressant c'est que la plupart des chiffres d'affaires est obtenu à partir de la vente de quelques produits. Cela peut se montrer avec un coefficient de Gini de 0.805

Pour accèder au code complet [click ici](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/PM2-optimisation%20de%20la%20gestion%20des%20donn%C3%A9es%20d'une%20boutique/Ocon_Jorge_1_notebook_062022.ipynb) 

