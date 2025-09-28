# Rapport Statistique Complet : Analyse des Données de VertiGo

## Introduction

Dans un contexte de concurrence accrue, VertiGo a entrepris une étude statistique pour mieux comprendre les préférences de ses clients et optimiser ses stratégies marketing. Cette recherche vise à analyser les données des clients, évaluer l'impact des campagnes marketing, et identifier les facteurs clés influençant les décisions d'achat.

**Objectifs :**

*   Identifier les préférences de voyage (destinations, types, saisons).
*   Évaluer l'efficacité des campagnes marketing.
*   Analyser les tendances d'achat en fonction de critères démographiques.

## Méthodologie de l'étude

Deux sources de données de 2023 ont été utilisées : une base de données client (profil, type de voyage, destination, etc.) et une base de données entreprise (revenus, dépenses publicitaires, etc.).

Le nettoyage des données a inclus la suppression des doublons, le traitement des outliers et l'imputation des valeurs manquantes (médiane pour le numérique, mode pour le catégoriel).

Les tests statistiques suivants ont été menés pour répondre aux questions de recherche.

## Analyses et Résultats

### 1. Analyse Quantitative

#### Test de Pearson : Dépenses Publicitaires vs. Nombre de Réservations

**Objectif :** Vérifier si l'augmentation des dépenses marketing est associée à une augmentation des réservations.

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

data_entreprise = pd.read_csv('Données+entreprise+VertiGo+nettoyées.csv')
sns.regplot(x=data_entreprise['Depenses_Publicitaires'], y=data_entreprise['Nombre_de_Reservations'], line_kws=dict(color="r"))
plt.title('Dépenses publicitaires vs. Nombre de réservations')
plt.show()
```

**Résultats :**
*   **Coefficient de corrélation de Pearson (r) :** 0,926
*   **Valeur p :** 0,0

**Interprétation :** Le coefficient indique une **forte corrélation positive** et statistiquement significative. L'augmentation des dépenses publicitaires est fortement associée à une augmentation du nombre de réservations.

---

#### Test de Pearson : Revenu des Ventes vs. Nombre de Clients

**Objectif :** Confirmer la relation entre le nombre de clients et le revenu généré.

```python
sns.regplot(x=data_entreprise['Revenu_des_Ventes'], y=data_entreprise['Nombre_de_Clients'], line_kws=dict(color="r"))
plt.title('Revenu des ventes vs. Nombre de clients')
plt.show()
```

**Résultats :**
*   **Coefficient de corrélation de Pearson (r) :** 0,980
*   **Valeur p :** 0,0

**Interprétation :** La corrélation est **extrêmement forte et positive**. Cela confirme logiquement que plus le nombre de clients augmente, plus le revenu des ventes est élevé.

---

#### Test de Spearman : Durée de Voyage vs. Prix Total

**Objectif :** Identifier si des séjours plus longs sont associés à des dépenses plus élevées.

```python
data_clients = pd.read_csv('vertigo_nettoyées.csv')
sns.regplot(data=data_clients, x='durée de voyage (en jours)', y='prix total', line_kws=dict(color="r"))
plt.title('Durée de voyage vs. Prix total')
plt.show()
```

**Résultats :**
*   **Coefficient de corrélation de Spearman (ρ) :** 0,994
*   **Valeur p :** 0,0

**Interprétation :** La corrélation est **très forte et positive**. La durée du voyage est un déterminant majeur du prix total.
### 2. Analyse Qualitative

#### Test de Fisher : Genre vs. Type de Voyage

**Objectif :** Comprendre si le genre des clients influence leur choix de type de voyage (aventure vs. culturel).

```python
data_clients_fisher = data_clients[data_clients['genre'] != 'Autre']
contingency_table_fm = pd.crosstab(data_clients_fisher['genre'], data_clients_fisher['type de voyage'])
sns.heatmap(contingency_table_fm, annot=True, fmt='d')
plt.title('Heatmap Genre vs. Type de voyage')
plt.show()
```

**Résultats :**
*   **Odds ratio :** 1,108
*   **p-valeur :** 0,726

**Interprétation :** Aucune association statistiquement significative n'a été observée. Le genre n'influence pas la préférence pour un type de voyage plutôt qu'un autre.

---

#### Test du Chi-2 : Saison de Voyage vs. Type de Voyage

**Objectif :** Déterminer si les choix de voyage varient en fonction des saisons.

```python
contingency_table_saison = pd.crosstab(data_clients['saison de voyage'], data_clients['type de voyage'])
sns.heatmap(contingency_table_saison, annot=True, fmt='d')
plt.title('Heatmap Saison vs. Type de voyage')
plt.show()
```

**Résultats :**
*   **Statistique Chi-2 :** 5,361
*   **Valeur p :** 0,498

**Interprétation :** Aucun lien de dépendance significatif. Le type de voyage préféré ne dépend pas de la saison.

---

#### Test du Chi-2 : Saison de Voyage vs. Destination

**Objectif :** Analyser si le choix de la destination dépend de la saison.

```python
contingency_table_dest = pd.crosstab(data_clients['saison de voyage'], data_clients['destination'])
plt.figure(figsize=(12, 8))
sns.heatmap(contingency_table_dest, annot=True, fmt='d')
plt.title('Heatmap Saison vs. Destination')
plt.show()
```

**Résultats :**
*   **Statistique Chi-2 :** 34,06
*   **Valeur p :** 0,416

**Interprétation :** La valeur p étant très supérieure à 0,05, il n'y a pas de relation statistiquement significative entre la saison et le choix de la destination.

---

#### Test T : Évaluation des séjours Été vs. Hiver

**Objectif :** Déterminer si la satisfaction client varie entre l'été et l'hiver.

```python
sns.boxplot(x='saison de voyage', y='évaluation sur 5', data=data_clients)
plt.title('Évaluations des séjours par saison')
plt.show()
```

**Résultats :**
*   **Statistique T :** -1,277
*   **Valeur p :** 0,202

**Interprétation :** La différence d'évaluation entre l'été et l'hiver n'est pas statistiquement significative.

---

#### Test d'ANOVA : Satisfaction vs. Type de Voyage

**Objectif :** Identifier si un type de voyage génère plus de satisfaction.

```python
sns.boxplot(x='type de voyage', y='évaluation sur 5', data=data_clients)
plt.title('Niveaux de satisfaction par type de voyage')
plt.show()
```

**Résultats :**
*   **Statistique F :** 0,703
*   **Valeur p :** 0,495

**Interprétation :** Aucune différence significative de satisfaction n'a été observée entre les types de voyages.

---

#### Test d'ANOVA : Âge des Clients vs. Type de Voyage

**Objectif :** Déterminer si l'âge moyen des clients diffère selon le type de voyage choisi.

```python
sns.boxplot(x='type de voyage', y='âge', data=data_clients)
plt.title('Distribution de l\'âge par type de voyage')
plt.show()
```

**Résultats :**
*   **Statistique F :** 1,428
*   **Valeur p :** 0,240

**Interprétation :** Il n'y a pas de différence statistiquement significative dans l'âge moyen des clients entre les différents types de voyage.

## Limites de l'étude

Les données sont limitées à l'année 2023 et ne permettent pas d'analyser les tendances à long terme. De plus, des facteurs externes (conjoncture économique, etc.) non capturés pourraient influencer les résultats. Ces conclusions doivent donc être interprétées avec prudence.

## Conclusion

### Récapitulatif des découvertes

1.  **Marketing Efficace :** L'investissement publicitaire est fortement corrélé aux réservations (r=0.926).
2.  **Logique Commerciale Confirmée :** Le revenu est très fortement corrélé au nombre de clients (r=0.980).
3.  **Prix et Durée :** Le coût d'un voyage est quasi parfaitement corrélé à sa durée (ρ=0.994).
4.  **Stabilité des Préférences :** Les préférences de voyage ne varient pas de manière significative ni selon le **genre**, ni selon la **saison**.
5.  **Satisfaction Uniforme :** Le niveau de satisfaction des clients est constant, quel que soit le type de voyage ou la saison.
6.  **Clientèle d'âge homogène :** L'âge moyen des clients ne diffère pas significativement d'un type de voyage à l'autre.

### Recommandations pratiques

1.  **Augmenter les investissements en publicité :** La forte corrélation justifie une augmentation du budget marketing pour stimuler les réservations.
2.  **Développer des offres basées sur la durée :** Créer des forfaits pour séjours prolongés afin d'optimiser les revenus, puisque la durée est le principal facteur de coût.
3.  **Uniformiser les offres :** Simplifier le marketing avec des offres cohérentes tout au long de l'année et pour tous les genres, puisque les préférences ne varient pas sur ces critères.
4.  **Poursuivre la recherche :** Mener des études futures avec des données sur plusieurs années pour affiner les stratégies et détecter des tendances à long terme.
