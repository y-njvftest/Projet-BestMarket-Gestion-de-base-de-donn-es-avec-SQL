![Logo](https://github.com/y-njvftest/projet-Best-Market-gestion-de-base-de-donnees-SQL/blob/main/logo%20Best%20Market.png)

# Projet Best Market (gestion de requêtes SQL)

Le projet Best Market est un projet fictif portant sur l'exploitation d'une base de données et la création de requêtes SQL

Dans ce projet, la société Best Market nous solicite afin de pouvoir analyser les retours clients sur leurs différents magasins, que l'entreprise stock sans pouvoir en faire grand chose.

Ce projet est constitué de plusieurs phases.

La première phase permet de découvrir la base de données via le dictionnaire des données et son modèle relationnel.

![image](https://github.com/y-njvftest/projet-Best-Market-gestion-de-base-de-donnees-SQL/assets/166219135/3c885b12-d4ea-4867-a4cb-e04140f451e0)

À partir de cela nous pouvons entamer différentes requêtes SQL nous permettant de tirer de meilleurs analyses des données.

Nous nous concentrerons sur différenntes parties sur de la base de données, notamment la composition des avis clients de la base de données:

## Nombre de retours clients par catégorie de services 

```SQL
SELECT COUNT(cle_retour_client) AS nombre_de_retour_clients,libelle_categorie
FROM retour_client
Group BY libelle_categorie
```
<img width="972" alt="Capture d’écran 2024-07-05 à 14 04 12" src="https://github.com/y-njvftest/projet-Best-Market-gestion-de-base-de-donnees-SQL/assets/166219135/adb70984-73b8-40ff-b47b-75db88942e3a">

Nous analyserons aussi d'autre caractéristiques inhérentes au produit comme leurs typologies:

## Note moyenne pour chaque catégorie de produit, classée de la meilleure à la moins bonne ?

```SQL 
SELECT avg(note) as moyenne_notes,typologie_produit
FROM retour_client,produit
WHERE retour_client.cle_produit=produit.cle_produit
AND libelle_categorie='service après-vente'
GROUP BY typologie_produit
ORDER BY moyenne_notes desc
```
<img width="584" alt="Capture d’écran 2024-07-05 à 15 20 33" src="https://github.com/y-njvftest/projet-Best-Market-gestion-de-base-de-donnees-SQL/assets/166219135/23279fe6-e6a2-47d4-b1b7-747334521695">

Nous pourrons enfin créer des requêtes SQL basées sur un point de vue temporel, en effet tous les avis clients sont accompagnés de leur dates d'émission:

## Une analyse temporelle des avis clients

## les typologies de produits qui ont amélioré leur moyenne entre le premier et le deuxième trimestre 2021

```SQL
WITH

premier_trimestre AS
(SELECT round(avg(note),3) as note_moyenne1ER,typologie_produit
from retour_client,produit
where retour_client.cle_produit=produit.cle_produit
and date_achat between '2021-01-01'and '2021-03-31'
group by typologie_produit ),

second_trimestre  AS
(SELECT round(avg(note),3) as note_moyenne2eme,typologie_produit
from retour_client,produit
where retour_client.cle_produit=produit.cle_produit
and date_achat between '2021-04-01'and '2021-06-30'
group by typologie_produit )

SELECT note_moyenne1ER,note_moyenne2eme,second_trimestre.typologie_produit
FROM premier_trimestre,second_trimestre
WHERE premier_trimestre.typologie_produit=second_trimestre.typologie_produit

```

<img width="618" alt="Screenshot 2024-07-05 at 17 07 57" src="https://github.com/y-njvftest/projet-Best-Market-gestion-de-base-de-donnees-SQL/assets/166219135/fb3fb863-b385-47fb-aae2-d644bfc97ba4">



