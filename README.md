
---

#  Data Warehouse – Gestion des Ventes & Logistique

##  Présentation du Projet

Ce projet met en œuvre une **architecture complète d’intégration de données** visant à centraliser les données opérationnelles d’une entreprise de vente en ligne dans un **Data Warehouse décisionnel**.

L’objectif est de fournir aux services **Finance** et **Logistique** des indicateurs fiables et exploitables pour le pilotage stratégique.

---

##  Architecture du Système

###  Source (SIO – Système d’Information Opérationnel)

Base transactionnelle située à **Aurillac** contenant :

* Clients
* Commandes
* Détails de commandes
* Produits
* Catégories
* Employés
* Fournisseurs
* Messagers (transporteurs)

###  Cible (SID – Système d’Information Décisionnel)

Data Warehouse situé au siège à **Paris**, structuré selon une modélisation en **schémas en étoile**.

---

##  Stack Technique

*  **PostgreSQL** – Base source et Data Warehouse
*  **Talend Open Studio** – Orchestration des flux ETL
*  Excel – Restitution décisionnelle (Data Marts)

---

#  Modélisation Décisionnelle

##  Datamart 1 – Gestion des Ventes

###  Table de faits

`_faits_gestion_ventes`

###  Dimensions

* `_dim_client`
* `_dim_produit`
* `_dim_categorie`
* `_dim_commande`
* `_dim_employe`

###  Indicateurs produits

* Chiffre d'affaires par client
* CA par pays
* CA par ville
* CA par catégorie
* CA par employé

---

##  Datamart 2 – Gestion Logistique

###  Table de faits

`_faits_gestion_livraisons`

###  Dimensions

* `_dim2_messager`
* `_dim2_commande`
* `_dim2_produit`

###  Indicateurs produits

* Délai de livraison brut
* Délai moyen par transporteur
* Nombre de livraisons par pays
* Livraisons par messager
* Produits les plus livrés

---

#  Processus ETL

Les flux ETL assurent le transfert des données entre :

**Aurillac (SIO)** ➜ **Paris (Data Warehouse)**

Chaque flux Talend suit la logique :

1. **Extraction (tDBInput)** – Requêtes SQL avec jointures
2. **Transformation (tMap)** – Mapping & conversions
3. **Chargement (tDBOutput)** – Insertion dans le DW

---

#  Exemple de KPI (SQL)

###  CA par Client

```sql
CREATE VIEW public.CAbycli AS
SELECT codecli,
       SUM((prixunit * qte)::numeric * (1 - remise)) AS CAclient
FROM _faits_gestion_ventes
GROUP BY codecli;
```

###  Délai moyen par messager

```sql
CREATE VIEW delaimoyenlivraisonbymessager AS
SELECT nomess,
       CEIL(AVG(dateenv::date - datecom::date)) AS delaimoyen
FROM _faits_gestion_livraisons
GROUP BY nomess;
```

---

#  Objectifs Stratégiques

✔ Centralisation des données
✔ Fiabilisation des indicateurs
✔ Automatisation des flux
✔ Autonomie des services métiers
✔ Architecture évolutive

---

#  Valeur Ajoutée

Ce projet démontre :

* La conception d’un **Data Warehouse complet**
* La modélisation en **schéma en étoile**
* L’implémentation de flux ETL industrialisés
* La production de Data Marts orientés métier
* Une démarche Business Intelligence structurée

---

#  Auteurs

* Adebola Fonton
* Otiniel Aguida
* Hugues Fangnon

Encadrant : M. Bruno Lagarde
Année universitaire : 2025 – 2026

---

#  Perspectives d'Évolution

* Ajout d’autres sites géographiques
* Intégration d’outils BI (Power BI / Tableau)
* Mise en place d’un ordonnancement automatique
* Gestion de l’historisation (SCD)

---
