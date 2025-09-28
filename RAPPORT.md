# Rapport Statistique : Analyse des Données de VertiGo

## Introduction

Dans un contexte de concurrence accrue et de changement des comportements des consommateurs, VertiGo, une entreprise spécialisée dans les voyages insolites, a entrepris une étude statistique approfondie pour mieux comprendre les préférences de ses clients et optimiser ses stratégies de marketing. Cette recherche vise à analyser les données des clients, évaluer l'impact des campagnes marketing, et identifier les facteurs clés influençant les décisions d'achat. Les résultats obtenus permettront de mieux aligner les offres de l'entreprise sur les attentes des clients, contribuant ainsi à une amélioration de la performance commerciale de VertiGo. Les objectifs principaux de cette étude sont :

*   Identifier les préférences de voyage les plus populaires parmi les clients;
*   Évaluer l'efficacité des différentes campagnes marketing sur les ventes;
*   Analyser des tendances d'achat en fonction de critères démographiques comme l'âge et le revenu des clients.

## Méthodologie de l'étude

Pour cette étude, nous avons utilisé les données clients collectées par VertiGo à partir de 2023. Les données sont disponibles dans deux bases de données distinctes. La première inclut des informations sur le profil des clients (âge, genre), le type de voyage réservé, les destinations choisies, etc. La seconde base de données inclut des informations liées à l'entreprise comme le nombre de clients, les revenus de ventes, les dépenses publicitaires et le nombre des réservations.

Ces données ont été nettoyées pour supprimer les doublons, traiter les outliers et imputer les valeurs manquantes en utilisant la médiane pour les données numériques et le mode pour les données catégorielles.

Plusieurs tests statistiques ont été menés pour répondre aux questions de recherche, notamment les tests de Pearson, Spearman, Fisher, Chi-2, T-test et ANOVA.

## Analyses et Résultats

### Le test de Pearson

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

Le coefficient de corrélation de 0,926 indique une **forte corrélation positive** entre les dépenses publicitaires et le nombre de réservations. La valeur p de 0,0 (inférieure à 0,05) confirme que cette corrélation est **statistiquement significative**.

### Le test de Spearman

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

Le coefficient de 0,994 indique une **très forte corrélation positive** entre la durée et le prix total des voyages. La corrélation est **statistiquement significative**, suggérant que la durée est un déterminant majeur du prix.

### Le test de Fisher

**Objectif :** Comprendre si le genre des clients influence leur choix de type de voyage.

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

Aucune association statistiquement significative n'a été observée. L'odds ratio proche de 1 et la valeur p élevée (0,726) confirment qu'il n'y a pas de différence de préférences entre hommes et femmes pour les voyages d'aventure ou culturels.

### Le test du Chi-2

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

Le test n'a révélé aucun lien de dépendance significatif. La valeur p de 0,498 indique que la saison de voyage est indépendante du type de voyage préféré.

### Le test T

**Objectif :** Déterminer si la satisfaction des clients varie de manière significative entre l'été et l'hiver.

```python
sns.boxplot(x='saison de voyage', y='évaluation sur 5', data=data_clients)
plt.title('Évaluations des séjours par saison')
plt.show()
```

**Résultats :**
*   **Statistique de test T :** -1,277
*   **Valeur p :** 0,202

La différence d'évaluation entre l'été et l'hiver n'est pas statistiquement significative, comme l'indique la valeur p de 0,202.

### Le test d'ANOVA

**Objectif :** Identifier si un type de voyage (aventure, détente, culturel) génère plus de satisfaction que les autres.

```python
sns.boxplot(x='type de voyage', y='évaluation sur 5', data=data_clients)
plt.title('Niveaux de satisfaction par type de voyage')
plt.show()
```

**Résultats :**
*   **Statistique de test F :** 0,703
*   **Valeur p :** 0,495

Aucune différence significative de satisfaction n'a été observée entre les types de voyages. La valeur p élevée (0,495) le confirme.

## Limites de l'étude

Les données de cette étude sont limitées à l'année 2023, ce qui ne permet pas d'analyser les tendances à long terme. De plus, des facteurs externes (conjoncture économique, événements spéciaux) non inclus dans les données pourraient influencer les résultats. Les conclusions doivent donc être interprétées avec prudence.

## Conclusion

### Récapitulatif des découvertes

1.  **Dépenses publicitaires et réservations :** Une forte corrélation positive (r = 0.926) suggère que le marketing est efficace.
2.  **Durée et coût des séjours :** Une très forte corrélation (ρ = 0.994) montre que les séjours plus longs sont plus chers.
3.  **Préférences par genre :** Aucune différence significative n'a été trouvée entre les genres.
4.  **Préférences par saison :** Le type de voyage préféré ne varie pas significativement avec les saisons.

### Recommandations pratiques

1.  **Augmenter les investissements en publicité :** La forte corrélation justifie une augmentation du budget marketing pour stimuler les réservations.
2.  **Développer des offres basées sur la durée :** Créer des forfaits pour séjours prolongés afin d'optimiser les revenus.
3.  **Uniformiser les offres saisonnières :** Simplifier le marketing avec des offres cohérentes tout au long de l'année, puisque les préférences ne varient pas selon la saison.
4.  **Poursuivre la recherche :** Mener des études futures avec des données sur plusieurs années pour affiner les stratégies.
