# Memoire_Maintenance_Predictive

# Maintenance Prédictive sur Microturbines à Gaz

## Description du Projet

Ce projet implémente un système de maintenance prédictive pour des microturbines à gaz en combinant deux approches complémentaires :

1. **Détection de défauts** (Classification binaire) : Identifier la présence d'anomalies
2. **Estimation de la durée de vie résiduelle (RUL)** (Régression) : Prédire le temps restant avant défaillance

## Structure du Projet

predictive_maintenance/
├── Detection_Defaut/
│   ├── Fusion_Fichier_Sans_Defaut_0.ipynb
│   ├── Verification_Arrets_0.ipynb
│   ├── Etiquetage_Fichiers_1.ipynb
│   ├── Recalage_Temporel_2.ipynb
│   ├── Creation_Dataset_3.ipynb
│   ├── Nettoyage_Colonnes_Inexistantes_3bis.ipynb
│   └── Detection_Defaut_Final_4.ipynb
│
└── Estimation_RUL/
├── Etiquetage_RUL_1.ipynb
├── Preparation_RUL_ML_2.ipynb
├── RUL_Entrainement_Specialise_3.ipynb
└── RUL_Entrainement_Generaliste_3.ipynb

## Module 1 : Détection de Défauts

### Notebooks (dans l'ordre d'exécution)

1. **Fusion_Fichier_Sans_Defaut_0.ipynb**
   - Consolidation des fichiers de simulation sans défaut
   - Déplacement et organisation des fichiers dans un dossier unique
   - Renommage cohérent avec numérotation séquentielle
   - Détection et suppression des doublons

2. **Verification_Arrets_0.ipynb**
   - Vérification des temps de simulation et critères d'arrêt
   - Analyse des valeurs Beta_Comp, Beta_Turb, T_rec_h_in
   - Visualisation des critères d'arrêt par dossier
   - Répartition des fichiers Defaut_Tuyaux en 4 sous-dossiers

3. **Etiquetage_Fichiers_1.ipynb**
   - Ajout de la colonne "Defaut" (0/1) selon le timestamp du défaut
   - Extraction automatique du début du défaut depuis les noms de fichiers
   - Traitement de 13 types de défauts différents
   - Sauvegarde dans fichiers_etiquettes_defaut/

4. **Recalage_Temporel_2.ipynb**
   - Alignement temporel avec t=0 au moment du défaut
   - Recalage aléatoire pour les fichiers sans défaut
   - Reconstruction du temps avec pas uniforme de 0.1s
   - Sauvegarde dans fichiers_temps_recale/

5. **Creation_Dataset_3.ipynb**
   - Split train/test 80/20 au niveau des unités
   - Échantillonnage aléatoire pour le train
   - 21 datasets de test à pourcentages fixes (0-100%)
   - Dataset pré-défaut pour analyse des fausses alarmes

6. **Nettoyage_Colonnes_Inexistantes_3bis.ipynb**
   - Harmonisation des colonnes entre tous les fichiers
   - Conservation de 41 colonnes essentielles
   - Suppression des colonnes Power_*, Eff_*, Beta_*, etc.
   - Vérification de cohérence et rapport de nettoyage

7. **Detection_Defaut_Final_4.ipynb**
   - Pipeline complet d'analyse ML
   - Cross-validation de 90 combinaisons (scalers × features × modèles)
   - Sélection de features par 13 méthodes différentes
   - Optimisation bayésienne avec Optuna
   - Analyse SHAP pour l'explicabilité
   - Optimisation du seuil de décision

## Module 2 : Estimation RUL

### Notebooks (dans l'ordre d'exécution)

1. **Etiquetage_RUL_1.ipynb**
   - Parcours des 13 dossiers de défauts
   - Extraction du début du défaut depuis les noms de fichiers
   - Calcul de la RUL comme différence entre temps final et temps courant
   - Conservation des colonnes communes uniquement
   - Export vers all_scenarios_with_RUL.csv

2. **Preparation_RUL_ML_2.ipynb**
   - Chargement du fichier global all_scenarios_with_RUL.csv
   - Normalisation du temps par unité (remise à t=0)
   - Filtrage des colonnes pour garder 46 features + RUL + unit_id
   - Sauvegarde dans donneeML.txt
   - Affichage des statistiques générales

3. **RUL_Entrainement_Specialise_3.ipynb**
   - Feature engineering avec agrégations (last, mean, std, slope, change_rate)
   - Justification empirique du feature engineering
   - Sélection de features par corrélation et méthodes avancées
   - Optimisation du scaler et des modèles
   - Analyse multi-pourcentage (1% à 90%)
   - Analyse de l'importance des features et SHAP
   - Modèles d'ensemble (Voting, Stacking)

4. **RUL_Entrainement_Generaliste_3.ipynb**
   - Installation automatique des dépendances (optuna, shap, catboost)
   - Feature engineering avec 5 types d'agrégations temporelles
   - 11 méthodes de sélection de variables comparées
   - Optimisation bayésienne des hyperparamètres
   - Analyse multi-pourcentage (1% à 90% du cycle de vie)
   - Modèles d'ensemble (Voting & Stacking)
   - Génération automatique de 15+ visualisations haute résolution

## Types de Défauts Traités

- Defaut_CC_T
- Defaut_Compresseur
- Defaut_Fuite_Fuel
- Defaut_Noise_TOT
- Defaut_Rec_T
- Defaut_Recuperateur
- Defaut_Turbine
- FiltreAir
- Defaut_Mix_Final
- Defaut_Tuyaux (4 types : cc_t, comp_rec, rec_cc, turb_rec)

## Capteurs Utilisés

36 colonnes de capteurs incluant :
- Températures : T_amb, T_c_in/out, T_rec_*, T_cc_*, T_t_*
- Pressions : p_amb, p_c_in/out, p_rec_*, p_cc_*, p_t_*
- Débits : m_amb, m_c_in/out, m_rec_*, m_cc_*, m_t_*, m_fuel
- Vitesses : N, Nref

## Technologies Utilisées

- Python 3.x
- Pandas, NumPy, Scipy
- Scikit-learn
- XGBoost, LightGBM, CatBoost
- Optuna (optimisation bayésienne)
- SHAP (explicabilité)
- Matplotlib, Seaborn, Plotly

## Installation

```bash
pip install pandas numpy scipy matplotlib seaborn plotly
pip install scikit-learn xgboost lightgbm catboost
pip install optuna shap tqdm joblib

Utilisation

Exécuter les notebooks du dossier Detection_Defaut dans l'ordre numérique
Exécuter les notebooks du dossier Estimation_RUL dans l'ordre numérique
Les résultats et modèles sont sauvegardés dans les dossiers appropriés

Auteur
Justin Pareit - Master 2 Ingénieur de gestion
