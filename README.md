# Détection de Nuages Martiens par Random Forest
## Projet de Référence (Baseline) pour la Classification de Profils Atmosphériques (MCS)

![Badge Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg)
![Badge Scikit-learn](https://img.shields.io/badge/Scikit--learn-1.x-orange.svg)
![Badge Statut](https://img.shields.io/badge/Statut-Baseline%20Complétée-brightgreen.svg)
Ce projet vise à construire un modèle de **baseline** (référence) pour la détection automatique des nuages de haute altitude sur Mars, en utilisant les données de l'instrument MCS (Mars Climate Sounder).

L'objectif n'est pas d'atteindre la perfection, mais de :
1.  Prouver qu'une détection automatique via le Machine Learning classique est possible.
2.  Établir un score de performance (ex: Précision, F1-Score) que des modèles plus complexes (Deep Learning) devront battre.
3.  Comprendre quelles caractéristiques physiques sont les plus importantes pour prédire un nuage.

---

### 1. L'Hypothèse Scientifique 🔭

Les nuages de haute altitude sur Mars (les "arches") ne sont pas aléatoires. Ils créent des **signatures statistiques identifiables** dans les profils verticaux de température, d'opacité de la poussière et d'opacité de la glace d'eau.

Ce projet postule qu'un modèle de **Random Forest** peut être entraîné à reconnaître ces signatures, à condition qu'on lui fournisse des caractéristiques (features) pertinentes.

### 2. La Tâche de Machine Learning 🤖

* **Type :** Classification Binaire Supervisée.
* **Input (X) :** Un vecteur de caractéristiques *créées manuellement* (Feature Engineering) décrivant chaque profil atmosphérique.
* **Output (Y) :** Une prédiction à 2 classes : `0` (Pas de Nuage) ou `1` (Nuage Présent).
* **Vérité Terrain :** Les classifications consensus issues du projet Zooniverse "Cloudspotting on Mars".

### 3. La Méthodologie 🧠

Le cœur de ce projet est le **Feature Engineering**. Un Random Forest ne peut pas interpréter une séquence brute ou "voir" une forme (une "arche"). Notre travail consiste à traduire la *physique* et la *morphologie* de chaque profil en un ensemble de nombres.

**1. Feature Engineering (Création de Caractéristiques)**
Le script `notebooks/02_feature_engineering.ipynb` transforme chaque profil vertical brut en une seule ligne d'un tableur, avec des colonnes descriptives.
Exemples de features créées :
* `temp_min_absolue` : La température la plus basse du profil.
* `altitude_de_temp_min` : L'altitude de cette température minimale.
* `pic_opacite_glace` : La valeur maximale d'opacité de la glace.
* `altitude_pic_glace` : L'altitude de ce pic de glace.
* `temp_moyenne_70_80km` : La température moyenne dans la "zone d'intérêt".
* `pente_temp_max` : La pente la plus raide (positive ou négative) de la courbe de température.
* ... et bien d'autres.

**2. Modélisation (Baseline)**
Le script `notebooks/03_model_baseline.ipynb` utilise le tableur de features pour entraîner le modèle.
* **Modèle :** `RandomForestClassifier` (Scikit-learn).
* **Pourquoi ?** Il est robuste, performant sur les données tabulaires et **interprétable** (il peut nous dire quelles features ont été les plus utiles).
* **Pipeline :** Une `Pipeline` Scikit-learn est utilisée pour inclure le `StandardScaler` (mise à l'échelle) et le modèle, garantissant la reproductibilité et évitant les fuites de données.

---

### 4. 📈 Résultats de la Baseline

*(Section à compléter par vous-même)*

Le modèle de référence (Random Forest) a été entraîné sur 80% des données et testé sur les 20% restants.

#### Métriques de Performance

| Métrique | Score (Test Set) |
| :--- | :--- |
| Précision (Accuracy) | **XX.X%** |
| F1-Score (Macro) | **XX.X%** |
| Rappel (Recall - Classe 1) | **XX.X%** |
| Précision (Precision - Classe 1) | **XX.X%** |

#### Matrice de Confusion

*(Insérez ici votre image de la matrice de confusion)*
`![Matrice de Confusion](reports/figures/confusion_matrix.png)`

#### Importance des Caractéristiques (Feature Importances)

Cette analyse est cruciale pour valider notre hypothèse. Elle montre quelles caractéristiques le modèle a le plus utilisées pour prendre ses décisions.

*(Insérez ici votre graphique des feature importances)*
`![Feature Importances](reports/figures/feature_importances.png)`

**Principales découvertes :**
1.  La caractéristique la plus importante était `[NOM_DE_LA_FEATURE_1]` (ex: `pic_opacite_glace`).
2.  L'altitude du pic (`[NOM_DE_LA_FEATURE_2]`) était également un prédicteur clé.
3.  ...

---

### 5. 🚀 Comment Reproduire le Projet

1.  **Cloner le dépôt :**
    ```bash
    git clone [https://github.com/](https://github.com/)[VOTRE_NOM]/projet-mars-clouds-rf.git
    cd projet-mars-clouds-rf
    ```

2.  **Créer un environnement virtuel et l'activer :**
    ```bash
    python -m venv venv
    source venv/bin/activate  # Sur Windows: venv\Scripts\activate
    ```

3.  **Installer les dépendances :**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Ajouter les données (Important !) :**
    Les données ne sont pas suivies par Git (voir `.gitignore`). Vous devez placer vos fichiers de données brutes (issus de Zooniverse) dans le dossier `data/raw/`.

5.  **Exécuter les Notebooks :**
    Lancez Jupyter Lab (`jupyter lab`) et suivez les notebooks dans l'ordre numérique dans le dossier `notebooks/`:
    * `01_data_exploration.ipynb` : Analyse exploratoire des profils bruts.
    * `02_feature_engineering.ipynb` : Génération du fichier `data/processed/features.csv`.
    * `03_model_baseline.ipynb` : Entraînement, évaluation du modèle et génération des graphiques de résultats.

### 6. 📂 Structure du Dépôt

```text
projet-mars-clouds-rf/
|
+-- .gitignore          # Fichiers et dossiers à ignorer par Git
+-- README.md           # Ce fichier
+-- requirements.txt    # Dépendances Python
|
+-- data/               # (Ignoré par Git) Données du projet
|   +-- raw/            # Données brutes (à placer ici)
|   \-- processed/      # Données nettoyées (ex: features.csv)
|
+-- notebooks/          # Carnets Jupyter pour l'analyse
|   +-- 01_data_exploration.ipynb
|   +-- 02_feature_engineering.ipynb
|   \-- 03_model_baseline.ipynb
|
+-- src/                # (Optionnel) Scripts ou modules Python
|   \-- feature_utils.py # Ex: Fonctions d'aide pour le feature engineering
|
+-- models/             # (Ignoré par Git) Modèles entraînés
|   \-- rf_baseline_pipeline.joblib
|
\-- reports/            # (Non ignoré) Figures et rapports
    \-- figures/
        +-- confusion_matrix.png     \-- feature_importances.png  ```

---

### 7. 🎯 Prochaines Étapes

* [ ] Améliorer le feature engineering avec des caractéristiques plus complexes (ex: largeurs de pic, intégrales).
* [ ] Tester d'autres modèles de ML classiques (ex: XGBoost, LightGBM) pour voir s'ils battent le RF.
* [ ] Utiliser cette baseline comme référence pour un modèle de **Deep Learning** (ex: CNN 1D) qui analysera directement les profils bruts.