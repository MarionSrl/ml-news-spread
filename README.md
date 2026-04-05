# Prédiction des variations du spread des obligations souveraines à partir de l'actualité financière

## 1. Description du projet

Ce projet vise à étudier si le contenu de l’actualité financière permet de prédire l’évolution du spread entre les obligations souveraines françaises (OAT 10 ans) et allemandes (Bund 10 ans).

L’objectif est d’évaluer si des signaux textuels issus de la presse économique contiennent une information exploitable pour anticiper les mouvements des marchés obligataires.

Nous combinons des techniques de Natural Language Processing (NLP) et de Machine Learning pour construire des indices macro-financiers et tester leur pouvoir prédictif.


## 2. Sources de données

### 2.1 Données de taux souverains
- Sources : Banque de France (France 10Y) et Bundesbank (Allemagne 10Y)
- Fréquence : quotidienne
- Période : 2018 – 2026
- Construction :
Spread = France 10Y – Allemagne 10Y
Variation (delta)
Direction (classification binaire)
Volatilité glissante

Ces données constituent la variable cible du projet.

### 2.2 Data d’actualité financière

Nous avons exploré plusieurs sources :

- Reuters (Kaggle)
  - Headlines et descriptions
  - Période : 2018 – 2020

- GDELT
  - Base de news internationales
  - Extraction quotidienne
  - Filtrage Europe

- Tests exploratoires
  - Les Échos (web scraping limité)
  - Le Monde (RSS → trop peu de données)

→ GDELT s’est révélé être la source la plus exploitable pour le projet.


## 3. Méthodologie

### 3.1 Construction des features (NLP)

Une approche basée sur un dictionnaire est mise en œuvre pour construire des indices de sentiment macro-financier.

Les mots-clés sont regroupés en quatre catégories économiques :
- Inflation / Politique monétaire
- Risque / Crise
- Croissance
- Risque géopolitique

Pour GDELT :
- Filtrage des news liées à l’Europe
- Agrégation quotidienne
- Lissage (rolling mean)

### 3.2 Construction de la cible

Deux approches :

- Prédiction directionnelle
    - y=1 si le spread augmente à t+1
    - y=0 sinon
    - Décalage des variables → pas de look-ahead bias

- Prédiction de volatilité
    - Volatilité glissante du spread
    - Modélisation en régression

### 3.3 Sélection du modèle

Trois modèles de classification sont mis en œuvre :

- Random Forest (modèle non linéaire)
- Régression logistique (modèle linéaire de référence - baseline)
- Réseau de neurones (MLPClassifier)

Comme modèle de regression nous avons utilisé Ridge (volatilité).

### 3.4 Validation

- Split temporel (train/test)
- Cross-validation adaptée aux séries temporelles
- Rolling out-of-sample backtest

### 3.5 Métriques

- Accuracy
- ROC Curve
- AUC
- Baseline naïf
- Importance des variables
- Backtest cumulatif


## 4. Résultats

- Prédiction du spread
    - Performance proche du hasard (accuracy ≈ 50–57%)
    - AUC faible (~0.5 → 0.55)
    - Random Forest et Logistic Regression peu performants
    - Neural Network légèrement meilleur (≈ 56%)

→ Les modèles capturent très peu de signal exploitable

- Prédiction de la volatilité
    - R² faible
    - Erreurs importantes
    - Faible capacité explicative

- Analyse
    - Les variables news ont peu d’impact
    - Le modèle repose surtout sur la volatilité passée
    - Signal noyé dans le bruit de marché


## 5. Interprétation économique

Initialement, l’hypothèse était :

Des news négatives → fuite vers les actifs sûrs (Bund) → augmentation du spread

Cette intuition est économiquement valide mais :

- Les news utilisées ne sont pas assez ciblées
- L’information est déjà intégrée dans les prix
- Les marchés obligataires sont très efficients


## 6. Limites

- Dataset limité (Reuters)
- Approche NLP simple (keywords)
- Pas de sentiment analysis avancé
- Données journalières uniquement
- Pas de coûts de transaction dans le backtest
- Modèles peu optimisés


## 7. Améliorations possibles

- NLP avancé (BERT, embeddings)
- Analyse de sentiment fine
- Données intraday
- Enrichissement des variables macro
- Modèles plus complexes (XGBoost, LSTM)
- SHAP / interprétabilité


## 8. Conclusion

Ce projet montre que :

- La relation entre news et marchés obligataires est difficile à exploiter
- Le signal informationnel est faible face au bruit
- Les marchés intègrent rapidement l’information


Malgré des résultats limités, le projet permet de :

- Mettre en œuvre des méthodes de Machine Learning
- Explorer différentes sources de données
- Comprendre les limites des approches quantitatives


## Auteurs
Marion Sirol & Maxence Martini