# 📊 Projet Data Analysis — SQL & Looker Studio Dashboard

## 🧭 Contexte du projet

Ce projet a pour objectif d’analyser une base de données commerciale à l’aide de **requêtes SQL** et de présenter les résultats à travers un **dashboard interactif** créé sur **Looker Studio**.

L’analyse couvre les clients, les produits, les ventes et les employés afin de mettre en lumière les indicateurs clés de performance et les tendances business.

---

## 🗂️ Données et outils

- **Source de données** : tables SQL (`customers`, `order_items`, `products`, `categories`, `stores`, `staffs`).
- **Outils utilisés** :
    - 🧮 **SQL** : extraction, agrégation et exploration de données.
    - 📈 **Looker Studio** : conception du tableau de bord interactif.
- **Type d’analyse** : descriptive et exploratoire (EDA).

# 2️⃣ Modèle de données & architecture

Le modèle est une architecture **en flocon** (snowflake schema) centrée sur la table de faits `orders`.

```
                🔵 [orders]
                /    |     \\
🧑‍🤝‍🧑 [customers] 🏪 [stores] 👨‍💼 [staffs]
                      |
                   📦 [stocks]
                      |
           +----------+----------+
           |                     |
      🟩 [products]          📂 [categories]
           |
      🏷️ [brands]

```

### 🔍 Points clés :

- Relations **One-to-Many** sur l'ensemble du modèle (ex. : un client peut passer plusieurs commandes).
- **Normalisation** des données produits via les tables `brands` et `categories`.
- Architecture optimisée pour les **analyses détaillées** et la **scalabilité**.

---

## 3️⃣ Requêtes SQL et analyses

```sql
🏙️ **Top 10 des villes avec le plus de clients** :
SELECT c.city, COUNT(c.customer_id) AS q
FROM customers AS c
GROUP BY c.city
ORDER BY q desc
LIMIT 10;

👥  Nombre total de clients
SELECT COUNT(DISTINCT customers.customer_id)
FROM customers; 

📦 **Quantité totale de produits vendus**
SELECT SUM(order_items.quantity)
FROM order_items;

🧾 **Montant total après remise pour les 5 plus grosses commandes**
SELECT order_id, SUM(quantity) AS tot
FROM order_items
GROUP BY order_id
ORDER BY tot DESC
LIMIT 5;

SELECT order_id,
SUM(quantity * list_price * (1 - discount)) AS tt
from order_items
WHERE order_id IN (813, 1577, 944, 518, 100)
GROUP BY order_id;

**-💸 Écart de prix entre l’article le plus cher et le moins cher**
SELECT MAX(list_price), min(list_price)
FROM order_items;

**🗂️ Lister les produits avec leur catégorie**
SELECT p.product_name, c.category_name
FROM products AS p
JOIN categories AS c
ON p.category_id = c.category_id;

🧑‍💼 **Lister les employés et les magasins associés**
SELECT sf.first_name, sf.last_name, s.store_name
FROM staffs AS sf
LEFT JOIN stores AS s
ON sf.store_id = s.store_id;

```

# 4️⃣ Résultats & Insights

## 📊 Dashboard Looker Studio : *Bike Dashboard*

📍 **Lien du dashboard** :

👉 [Voir le tableau de bord interactif sur Looker Studio](https://lookerstudio.google.com/reporting/5aea3cb4-5de9-4de9-b00c-96922ba18a99)

### 🖼️ **Aperçu du contenu**

Le dashboard met en avant les principaux indicateurs commerciaux de la base de données à travers une interface claire et interactive.

### 🔹 Indicateurs clés (KPIs)

| Indicateur | Valeur |
| --- | --- |
| 💰 **Revenu total** | **7.69M** |
| 📦 **Total produits vendus** | **3.9K** |
| 💵 **Valeur moyenne de commande** | **1.63K** |
| 💲 **Prix le plus élevé** | **12.0K** |

### 🔹 Top 10 des villes avec le plus de clients

Carte des États-Unis illustrant la répartition géographique des clients.

Les villes majeures incluent :

- Mount Vernon
- Ballston Spa
- Scarsdale
- Canandaigua
- Ossining
- Floral Park
- Longview
- Merrick
- Sunnyside
- San Angelo

➡️ Ces zones concentrent la majorité de la clientèle.

### 🔹 Répartition par marque (% du chiffre d’affaires)

- **Trek** – 17.8%
- **Surly** – 13.3%
- **Sun Bicycles** – 15.6%
- **Strider** – 11.1%
- **Ritchey** – 8.9%
- **Pure Cycles** – 6.7%
- **Heller**, **Haro**, **Electra** – parts plus faibles

➡️ Les marques *Trek*, *Surly* et *Sun Bicycles* dominent le marché.

---

## 📈 Résultats et insights clés

- Les **10 villes** listées concentrent la majorité des ventes et clients.
- Le **revenu total de 7.69M** démontre un volume commercial conséquent.
- Une **valeur moyenne de commande de 1.63K**, signe d’un panier moyen élevé.
- La **marque Trek** est la plus performante, suivie de près par *Sun Bicycles* et *Surly*.
- Le **produit le plus cher atteint 12K**, traduisant une gamme premium.

---

## 🧠 Compétences mobilisées

- SQL (agrégations, jointures, filtres, calculs)
- Conception de tableaux de bord interactifs sous Looker Studio
- Analyse et interprétation des KPI
- Data storytelling et communication visuelle
