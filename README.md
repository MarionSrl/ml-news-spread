# Prédiction des variations du spread des obligations souveraines à partir de l'actualité financière

## 1. Description du projet

Ce projet étudie si le contenu de l'actualité financière peut contribuer à prédire l'évolution du spread de l'obligation souveraine française à 10 ans (OAT) par rapport au Bund allemand.

À l'aide de la technique de Natural Language Processing (NLP), nous construisons des indices d'actualité macroéconomique et testons leur capacité prédictive sur la dynamique du spread entre avril 2018 et juin 2020 (période des news Reuters).

L'objectif est d'évaluer si le sentiment macro-financier extrait des titres de la presse contient des informations exploitables pour les marchés obligataires.


## 2. Sources de données

### Data sur les obligations souveraines
- Source : FRED (Données économiques de la Réserve fédérale)
- Bund allemand à 10 ans : IRLTLT01DEM156N
- Rendement des obligations françaises à 10 ans : IRLTLT01FRM156N
- Fréquence : Mensuelle
- Période : Avril 2018 – Juin 2020

### Data d’actualité financière
- Source : Base de données Reuters Headlines (Kaggle)
- Contenu : Titres et brèves descriptions
- Période : Mars 2018 – Juillet 2020


## 3. Méthodologie

### 3.1 Traitement du langage naturel (NLP)

Une approche basée sur un dictionnaire est mise en œuvre pour construire des indices de sentiment macro-financier.

Les mots-clés sont regroupés en quatre catégories économiques :
- Inflation / Politique monétaire
- Risque / Crise
- Croissance
- Risque géopolitique

Des indices mensuels sont construits en comptabilisant les occurrences des mots-clés et en les agrégeant à une fréquence mensuelle.

### 3.2 Ingénierie des caractéristiques

- Spread souverain : OAT France 10 ans – Bund de l'Allemagne 10 ans
- Variation mensuelle du spread (delta)
- Classification directionnelle (1 = élargissement, 0 = resserrement)
- Volatilité glissante sur 6 mois

Les indices d'actualité sont fusionnés avec les données de spread à une fréquence mensuelle.

### 3.3 Sélection du modèle

Deux modèles sont mis en œuvre :

- Random Forest (modèle non linéaire)
- Régression logistique (modèle linéaire de référence - baseline)

Une division chronologique des données train/test est utilisée pour éviter les biais liés à l'anticipation.

Une validation croisée de séries temporelles (rolling splits) est également mise en œuvre.

### 3.4 Cadre d'évaluation

Métriques de performance :
- Précision
- Courbe ROC
- AUC
- Comparaison avec une méthode de référence naïve (prédiction systématique de 0)
- Analyse de l'importance des variables

Un backtest hors échantillon glissant est réalisé pour simuler une prévision en temps réel.


## 4. Résultats

Le modèle Random Forest surpasse le modèle de baseline naïf et la régression logistique en termes de précision prédictive.

L'analyse ROC-AUC suggère que le modèle possède un pouvoir prédictif modéré, supérieur à celui du hasard.

L'analyse de l'importance des variables indique que les indices de risque macroéconomique et géopolitiques contribuent significativement à la prédiction de la direction de la propagation.

Le backtesting sur échantillon glissant démontre que les signaux issus de l'actualité peuvent fournir une capacité de prévision économiquement pertinente.


## 5. Limitations
- Période d'échantillonnage courte (2018-2020)
- Approche NLP basée sur un dictionnaire (sans plongements lexicaux avancés)
- Absence d'optimisation des hyperparamètres
- Absence de modélisation des coûts de transaction lors des tests rétrospectifs
- Granularité limitée à une fréquence mensuelle

Perspectives d'avenir :
- Modèles NLP basés sur Transformer
- Données à plus haute fréquence
- Ensemble de données historiques étendu
- Analyse d'interprétabilité SHAP