# 🦠 Analyse des Tendances Épidémiques en France 2020-2023

![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-blue?logo=postgresql)
![Tableau](https://img.shields.io/badge/Tableau-Public-orange?logo=tableau)
![Status](https://img.shields.io/badge/Status-Completed-green)

> Analyse épidémiologique complète des principales maladies en France : COVID-19, Grippe saisonnière, Diabète et Cancer. Pipeline de données end-to-end depuis la collecte jusqu'au dashboard interactif.

## 📊 Dashboard Interactif

🔗 **[Voir le Dashboard Tableau Public](https://public.tableau.com/app/profile/ikram.benachour/viz/AnalyseCOVID-19enFrance2020-2023)**

[![Dashboard Preview](covid_vs_grippe_final.png)](https://public.tableau.com/app/profile/ikram.benachour/viz/AnalyseCOVID-19enFrance2020-2023)

---

## 🎯 Objectifs du Projet

- Analyser l'évolution des épidémies en France sur la période 2020-2023
- Identifier les corrélations entre COVID-19 et grippe saisonnière
- Visualiser les disparités régionales du diabète et de l'impact du cancer
- Construire un pipeline de données complet et reproductible

---

## 🔍 Insights Clés Découverts

| Insight | Donnée |
|---------|--------|
| 🔵 **Grippe quasi absente en 2021** | -90% d'incidence grâce aux gestes barrières COVID |
| 🔴 **Pic historique grippe jan 2025** | 522 cas / 100 000 habitants — record absolu |
| 🏥 **140 000 décès COVID** | 5 vagues identifiées entre 2020 et 2023 |
| 🗺️ **Bouches-du-Rhône & Paris** | Départements les plus touchés par le COVID |
| 📈 **Diabète +10%** | Progression de 5.96% à 6.58% entre 2020 et 2024 |
| 🎗️ **Impact COVID sur dépistage cancer** | Baisse de 274K à 277K cas en 2020-2021, rattrapage en 2022 |

---

## 🛠️ Stack Technique

| Technologie | Usage |
|-------------|-------|
| **Python 3.12** | Collecte, nettoyage, analyse, visualisation |
| **Pandas / NumPy** | Manipulation des données |
| **Matplotlib / Seaborn** | Graphiques statiques |
| **PostgreSQL 15** | Stockage et requêtes SQL avancées |
| **SQLAlchemy** | Connexion Python ↔ PostgreSQL |
| **Tableau Public** | Dashboard interactif |
| **Jupyter Lab** | Environnement d'analyse |

---

## 📁 Structure du Projet

```
analyse-epidemique-france/
│
├── 📓 notebooks/
│   └── analyse_epidemique_france.ipynb   # Notebook principal
│
├── 📊 data/
│   ├── covid_france_clean.csv            # Données COVID nettoyées
│   ├── grippe_france_clean.csv           # Données grippe (Réseau Sentinelles)
│   ├── diabete_france_clean.csv          # Données diabète par région
│   └── cancer_france_clean.csv           # Données cancer par type
│
├── 📈 visualisations/
│   ├── covid_france.png                  # Évolution COVID 2020-2023
│   ├── covid_vs_grippe_final.png         # COVID vs Grippe superposés
│   ├── heatmap_grippe_saisonnalite.png   # Heatmap saisonnalité grippe
│   ├── diabete_france.png                # Taux diabète par région
│   └── cancer_france.png                 # Impact COVID sur dépistage
│
└── README.md
```

---

## 🔄 Pipeline de Données

```
Sources de données
    │
    ├── data.gouv.fr (COVID-19)
    ├── sentiweb.fr (Grippe — Réseau Sentinelles INSERM)
    ├── SPF / INCa (Diabète, Cancer)
    │
    ▼
Python (Pandas) — Collecte & Nettoyage
    │
    ▼
PostgreSQL — Stockage (4 tables)
    │
    ├── clean_covid_france
    ├── clean_grippe_france
    ├── clean_diabete_france
    └── clean_cancer_france
    │
    ▼
Analyse SQL avancée (Window Functions, LAG, CASE)
    │
    ▼
Visualisations Python (Matplotlib / Seaborn)
    │
    ▼
Dashboard Tableau Public (interactif)
```

---

## 📊 Visualisations

### 1. Évolution COVID-19 en France 2020-2023
5 vagues clairement identifiées avec pics d'hospitalisations et réanimations.

### 2. COVID-19 vs Grippe Saisonnière
Corrélation inverse : quand le COVID était fort, la grippe disparaissait presque.

### 3. Heatmap Saisonnalité Grippe 2020-2026
![Heatmap](heatmap_grippe_saisonnalite.png)

### 4. Diabète par Région & Évolution Nationale
Hauts-de-France et Grand Est : régions les plus touchées (>7%).

### 5. Cancer — Impact COVID sur le Dépistage
Baisse visible du dépistage en 2020-2021, fort rattrapage ensuite.

---

## 🗃️ Requêtes SQL Avancées

```sql
-- Exemple : Évolution cancer avec taux de mortalité et variation annuelle
SELECT 
    annee,
    type_cancer,
    nouveaux_cas,
    ROUND(100.0 * deces / NULLIF(nouveaux_cas, 0), 1) AS taux_mortalite_pct,
    ROUND(100.0 * (nouveaux_cas - LAG(nouveaux_cas) 
        OVER (PARTITION BY type_cancer ORDER BY annee)) 
        / NULLIF(LAG(nouveaux_cas) OVER (PARTITION BY type_cancer ORDER BY annee), 0), 1) AS evolution_pct
FROM clean_cancer_france
ORDER BY type_cancer, annee;
```

---

## 🚀 Lancer le Projet

```bash
# 1. Cloner le repo
git clone https://github.com/ikram424/analyse-epidemique-france
cd analyse-epidemique-france

# 2. Créer l'environnement virtuel
python -m venv data-env
source data-env/bin/activate

# 3. Installer les dépendances
pip install pandas numpy matplotlib seaborn sqlalchemy psycopg2-binary jupyter

# 4. Lancer Jupyter
jupyter lab
```

---

## 📚 Sources de Données

| Source | Données | URL |
|--------|---------|-----|
| data.gouv.fr | COVID-19 hospitalisations | [Lien](https://www.data.gouv.fr/fr/datasets/donnees-hospitalieres-relatives-a-lepidemie-de-covid-19) |
| Réseau Sentinelles INSERM | Grippe saisonnière | [Lien](https://www.sentiweb.fr) |
| SPF / INCa | Cancer France | [Lien](https://www.e-cancer.fr) |
| AMELI / SPF | Diabète | [Lien](https://www.santepubliquefrance.fr) |

---

## 👩‍💻 Auteure

**Ikram Benachour** — Data Analyste junior 

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Ikram_Benachour-blue?logo=linkedin)](https://linkedin.com/in/ikram-benachour)
[![Tableau](https://img.shields.io/badge/Tableau-Public-orange?logo=tableau)](https://public.tableau.com/app/profile/ikram.benachour)
[![GitHub](https://img.shields.io/badge/GitHub-ikram424-black?logo=github)](https://github.com/ikram424)

---

