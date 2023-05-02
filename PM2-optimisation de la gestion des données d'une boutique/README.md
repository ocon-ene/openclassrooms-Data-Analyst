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

Après avoir nettoyé les trois tableaux, nous faison leur rapprochement à l'aide du méthode df.merge :

```python
join_erp = erp.merge(liaison, on="product_id")
```
Ce méthode nous permet de faire le merge des tableaux en "innier join", ce qui nous donne un tableaux qui partage les information des deux tableaux, parce qu'on veut faire le lien entre les objets vendus et les objets mises en vente.

Voici l'example du code qui permet de lier les trois tables :

```python
liaison.shape
```




    (825, 2)




```python
erp.shape
```




    (825, 5)




```python
web2.shape
```




    (714, 28)




```python
join_erp = erp.merge(liaison, on="product_id")
join_erp.head()
```




<div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>product_id</th>
      <th>onsale_web</th>
      <th>price</th>
      <th>stock_quantity</th>
      <th>stock_status</th>
      <th>id_web</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3847</td>
      <td>1</td>
      <td>24.2</td>
      <td>0</td>
      <td>outofstock</td>
      <td>15298</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3849</td>
      <td>1</td>
      <td>34.3</td>
      <td>0</td>
      <td>outofstock</td>
      <td>15296</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3850</td>
      <td>1</td>
      <td>20.8</td>
      <td>0</td>
      <td>outofstock</td>
      <td>15300</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4032</td>
      <td>1</td>
      <td>14.1</td>
      <td>0</td>
      <td>outofstock</td>
      <td>19814</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4039</td>
      <td>1</td>
      <td>46.0</td>
      <td>0</td>
      <td>outofstock</td>
      <td>19815</td>
    </tr>
  </tbody>
</table>
</div>




```python
erp_web = join_erp.merge(web2, on="id_web")
```


```python
erp_web = erp_web.drop(['post_name', 'guid'] , axis=1)
```


```python
erp_web.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>product_id</th>
      <th>onsale_web</th>
      <th>price</th>
      <th>stock_quantity</th>
      <th>stock_status</th>
      <th>id_web</th>
      <th>virtual</th>
      <th>downloadable</th>
      <th>rating_count</th>
      <th>average_rating</th>
      <th>...</th>
      <th>ping_status</th>
      <th>post_password</th>
      <th>post_modified</th>
      <th>post_modified_gmt</th>
      <th>post_content_filtered</th>
      <th>post_parent</th>
      <th>menu_order</th>
      <th>post_type</th>
      <th>post_mime_type</th>
      <th>comment_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3847</td>
      <td>1</td>
      <td>24.2</td>
      <td>0</td>
      <td>outofstock</td>
      <td>15298</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.0</td>
      <td>...</td>
      <td>closed</td>
      <td>NaN</td>
      <td>2019-12-30 09:30:29</td>
      <td>2019-12-30 08:30:29</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>product</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3849</td>
      <td>1</td>
      <td>34.3</td>
      <td>0</td>
      <td>outofstock</td>
      <td>15296</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.0</td>
      <td>...</td>
      <td>closed</td>
      <td>NaN</td>
      <td>2019-12-21 09:00:17</td>
      <td>2019-12-21 08:00:17</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>product</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3850</td>
      <td>1</td>
      <td>20.8</td>
      <td>0</td>
      <td>outofstock</td>
      <td>15300</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.0</td>
      <td>...</td>
      <td>closed</td>
      <td>NaN</td>
      <td>2020-06-26 18:15:03</td>
      <td>2020-06-26 16:15:03</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>product</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4032</td>
      <td>1</td>
      <td>14.1</td>
      <td>0</td>
      <td>outofstock</td>
      <td>19814</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.0</td>
      <td>...</td>
      <td>closed</td>
      <td>NaN</td>
      <td>2020-01-04 16:36:01</td>
      <td>2020-01-04 15:36:01</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>product</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4039</td>
      <td>1</td>
      <td>46.0</td>
      <td>0</td>
      <td>outofstock</td>
      <td>19815</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0.0</td>
      <td>...</td>
      <td>closed</td>
      <td>NaN</td>
      <td>2020-01-04 16:36:10</td>
      <td>2020-01-04 15:36:10</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>product</td>
      <td>NaN</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 31 columns</p>
</div>




```python
erp_web.shape
```




    (714, 31)

## Deuxième mission

On ajoute la colonne "chiffre d'affaires" au tableau pour pouvoir calculer le CA par produit et le CA total. 


```python
CA_produit["chiffre_daffaires"] = CA_produit["total_sales"] * CA_produit["price"]
```


```python
CA_produit = CA_produit.sort_values(by=['chiffre_daffaires'], ascending=False)
```


```python
CA_produit['cum_sum'] = CA_produit['chiffre_daffaires'].cumsum()
CA_produit['cum_perc'] = 100*CA_produit['cum_sum']/CA_produit['chiffre_daffaires'].sum()
```


```python
CA_produit.head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>product_id</th>
      <th>id_web</th>
      <th>price</th>
      <th>total_sales</th>
      <th>chiffre_daffaires</th>
      <th>cum_sum</th>
      <th>cum_perc</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>194</th>
      <td>4334</td>
      <td>7818</td>
      <td>49.0</td>
      <td>96.0</td>
      <td>4704.0</td>
      <td>4704.0</td>
      <td>6.665854</td>
    </tr>
    <tr>
      <th>71</th>
      <td>4144</td>
      <td>1662</td>
      <td>49.0</td>
      <td>87.0</td>
      <td>4263.0</td>
      <td>8967.0</td>
      <td>12.706785</td>
    </tr>
    <tr>
      <th>218</th>
      <td>4402</td>
      <td>3510</td>
      <td>176.0</td>
      <td>13.0</td>
      <td>2288.0</td>
      <td>11255.0</td>
      <td>15.949020</td>
    </tr>
    <tr>
      <th>70</th>
      <td>4142</td>
      <td>11641</td>
      <td>53.0</td>
      <td>30.0</td>
      <td>1590.0</td>
      <td>12845.0</td>
      <td>18.202147</td>
    </tr>
    <tr>
      <th>69</th>
      <td>4141</td>
      <td>304</td>
      <td>39.0</td>
      <td>40.0</td>
      <td>1560.0</td>
      <td>14405.0</td>
      <td>20.412761</td>
    </tr>
    <tr>
      <th>201</th>
      <td>4355</td>
      <td>12589</td>
      <td>126.5</td>
      <td>11.0</td>
      <td>1391.5</td>
      <td>15796.5</td>
      <td>22.384602</td>
    </tr>
    <tr>
      <th>199</th>
      <td>4352</td>
      <td>15940</td>
      <td>225.0</td>
      <td>5.0</td>
      <td>1125.0</td>
      <td>16921.5</td>
      <td>23.978795</td>
    </tr>
    <tr>
      <th>79</th>
      <td>4153</td>
      <td>16237</td>
      <td>29.0</td>
      <td>36.0</td>
      <td>1044.0</td>
      <td>17965.5</td>
      <td>25.458207</td>
    </tr>
    <tr>
      <th>651</th>
      <td>6206</td>
      <td>16580</td>
      <td>25.2</td>
      <td>41.0</td>
      <td>1033.2</td>
      <td>18998.7</td>
      <td>26.922314</td>
    </tr>
    <tr>
      <th>30</th>
      <td>4068</td>
      <td>16416</td>
      <td>16.6</td>
      <td>62.0</td>
      <td>1029.2</td>
      <td>20027.9</td>
      <td>28.380753</td>
    </tr>
  </tbody>
</table>
</div>

Le CA total c'est :


```python
CA_produit["chiffre_daffaires"].sum()
```




    70568.6

## Troisième mission

On peut analyser la variable price avec une boxplot.

![boxplot](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/images/boxplot_p5.PNG)

Nous avons aussi identifié les outliers avec la méthode IQR et la méthode du Z-score:

```python
#(q3-q1)*1.5 + q3
q1 = np.percentile(CA_produit["price"],25)
q3 = np.percentile(CA_produit["price"],75)
```


```python
B = (q3-q1)*1.5 + q3
```


```python
B
```

    84.28750000000001




```python
outliers = CA_produit.loc[CA_produit["price"] > B]
```


```python
outliers.shape
```




    (32, 7)


## Conclusion

Cette raprochement entre le fichier des ventes et le site en ligne va nous permetre d'analyser l'évolution de l'entreprise en terme de ventes.

Une donné qui à été très intéressant c'est que la plupart des chiffres d'affaires est obtenu à partir de la vente de quelques produits. Cela peut se montrer avec un coefficient de Gini de 0.805

Pour accèder au code complet [click ici](https://github.com/ocon-ene/openclassrooms-Data-Analyst/blob/main/PM2-optimisation%20de%20la%20gestion%20des%20donn%C3%A9es%20d'une%20boutique/Ocon_Jorge_1_notebook_062022.ipynb) 
