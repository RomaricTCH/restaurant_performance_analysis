# Restaurant Analytics : analyse commerciale et prévision du chiffre d'affaires

Pourquoi certaines journées sont-elles plus performantes que d'autres dans un établissement de restauration ? Peut-on anticiper le chiffre d'affaires du lendemain ?

Le projet étudie trois points de vente, de l'audit des données à la prévision du CA à J+1, en comparant des modèles de Machine Learning à des méthodes de référence simples.

## Données
*Anonymous Transactional Dataset* (v2), Engel, Lumban Gaol, Kurniawan, Budiharto, Mendeley Data, 2026. [DOI 10.17632/kcgf45y24m.2](https://doi.org/10.17632/kcgf45y24m.2), licence CC BY 4.0.

- 53 820 tickets réels d'une PME de restauration indonésienne : 3 points de vente, 96 produits
- Du 2 janvier au 30 septembre 2025, soit 272 jours par établissement
- Montants en roupies indonésiennes (IDR), sans données de coûts : l'étude porte sur le CA, pas sur la rentabilité

## Démarche

| Notebook | Contenu |
|---|---|
| `01_data_audit` | Qualité des données, transactions à montant nul |
| `02_data_preparation` | Table quotidienne : 816 lignes (date × établissement) |
| `03_performance_analysis` | CA, transactions, panier moyen, calendrier, composition des tickets |
| `04_revenue-forecasting` | Baselines, régression linéaire, Ridge, Random Forest |

Pour prédire le CA du jour J, seules les informations disponibles à la clôture de J−1 sont utilisées. Les modèles sont comparés par validation chronologique (mai, juin, juillet). Le test final porte sur août-septembre, réservé à part.

## Résultats

**Analyse commerciale**
- SHOP001 a le CA le plus élevé et le plus régulier (coefficient de variation de 15 %), contre 26 % pour SHOP003 et 31 % pour SHOP002.
- Le CA varie surtout avec le **nombre de transactions** (corrélation de 0,96 à 0,97). Le panier moyen reste stable.
- Le jour de la semaine a peu d'effet. Les écarts sont surtout mensuels : avril pour SHOP003, juin pour les trois établissements.
- Les tickets de deux articles représentent 15 % des transactions mais 27 % du CA.

**Prévision du CA du lendemain** (183 prévisions, août-septembre 2025)

| Méthode | MAE |
|---|---|
| Moyenne mobile 7 jours (baseline) | 178 267 IDR |
| **Régression linéaire** | **172 830 IDR** |

La régression réduit l'erreur de 3 % sur cette période, avec des gains hétérogènes : +7 % pour SHOP002, +4 % pour SHOP001 et −2 % pour SHOP003. L'écart n'a pas été testé statistiquement. La moyenne mobile reste une référence simple et compétitive.

## Pistes pour un gérant
- Prioriser le suivi et l'analyse de la **fréquentation** plutôt que sur le panier moyen.
- Encourager les tickets à deux articles (offres associées), qui pèsent près du double de leur part dans le CA.
- Analyser les baisses mensuelles, comme celle d'avril, avec des données externes.

## Limites
- 9 mois d'historique, sans saisonnalité annuelle complète ni facteurs externes (météo, événements, promotions) : les **causes** des variations ne peuvent pas être établies.
- Les 1 733 transactions à montant nul, liées à l'utilisation de la carte de fidélité, sont incluses dans les indicateurs.
- Les configurations des modèles ont été en partie choisies sur la période de validation. Seul le test final est une évaluation entièrement indépendante.

## Lancer le projet
    pip install -r requirements.txt
Exécuter ensuite les notebooks du dossier `notebooks/`, dans l'ordre de 01 à 04. Le notebook 02 génère `data/processed/performance_quotidienne.csv`.

**Auteur** : Romaric TCHAKOUNTE HANDOU · [LinkedIn](https://www.linkedin.com/in/romaric-armel-tchakounte-handou-a39552121/)