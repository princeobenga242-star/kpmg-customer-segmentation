# Segmentation de la clientèle par K-Means

Segmentation des clients du jeu de données **KPMG Customer Demography** avec l'algorithme K-Means (scikit-learn), en deux versions : sans et avec standardisation des variables numériques.

## Sommaire

- [Contexte](#contexte)
- [Données](#données)
- [Méthodologie](#méthodologie)
- [Résultats (notebook 01)](#résultats-notebook-01)
- [Version standardisée (notebook 02)](#version-standardisée-notebook-02)
- [Structure du dépôt](#structure-du-dépôt)
- [Installation](#installation)
- [Utilisation](#utilisation)
- [Limites](#limites)
- [Contribuer](#contribuer)
- [Licence](#licence)

## Contexte

Ce projet cherche à regrouper les clients d'une entreprise de vente de vélos en segments homogènes, afin de proposer à chaque segment des actions marketing adaptées (service premium, fidélisation, offres d'accueil, etc.).

## Données

Le jeu de données `KPMG_Customer_Demography.csv` contient des informations démographiques sur les clients (genre, âge, segment de richesse, ancienneté, valeur de la propriété, achats de vélos sur 3 ans, etc.).

**Le fichier CSV n'est pas versionné** : il contient des données clients (noms, adresses, dates de naissance). Il doit être placé dans le dossier `data/` avant d'exécuter les notebooks. Les colonnes attendues sont décrites dans [`data/README.md`](data/README.md).

## Méthodologie

Le dépôt contient deux notebooks :

1. **`01_segmentation_clients_kmeans.ipynb`** : analyse complète, sans standardisation des variables.
2. **`02_segmentation_clients_kmeans_standardise.ipynb`** : même préparation, mais les variables numériques sont standardisées avant le K-Means.

Étapes du notebook 01 :

1. Importation des données.
2. Nettoyage : suppression des colonnes d'identification et peu utiles (`customer_id`, `First_Name`, `Last_Name`, `DOB`, `Postcode`, `Country`, `deceased_indicator`, `Address`).
3. Statistiques descriptives et analyse univariée et bivariée.
4. Création de variables : `Age_group`, `Purchase_intensity`, `Wealth_score`, `Annual_purchase_frequency`, `Buyer_potential`.
5. Encodage *one-hot* des variables catégorielles (`pd.get_dummies`).
6. Choix du nombre de clusters (silhouette, méthode du coude), puis K-Means avec k = 3.
7. Visualisation par ACP et description des clusters (boxplots).

## Résultats (notebook 01)

Le K-Means sépare les clients en trois groupes, surtout selon leur score de richesse et leur fréquence d'achat, qui évoluent ensemble.

| Segment | Clients | Part | `Wealth_score` (moyenne) | Fréquence d'achat (moyenne) |
|---------|--------:|-----:|-------------------------:|----------------------------:|
| Premium | 854 | 22 % | 780 | 249 |
| Intermédiaire | 1 337 | 34 % | 420 | 170 |
| Faible valeur | 1 717 | 44 % | 125 | 79 |

L'âge est le même dans les trois groupes (médiane de 45 ans) et la valeur de la propriété discrimine peu : ces deux variables n'expliquent pas la segmentation.

## Version standardisée (notebook 02)

Dans le notebook 01, les variables qui ont les plus grandes valeurs (`Wealth_score`, `Annual_purchase_frequency`) pèsent le plus dans le calcul des distances. Le notebook 02 standardise les variables numériques (moyenne 0, écart-type 1) pour leur donner le même poids. Les colonnes 0/1 issues de l'encodage ne sont pas standardisées.

| Cluster | Profil | Clients | Part | Âge moyen | Ancienneté moyenne | `Wealth_score` (moyenne) | Fréquence d'achat (moyenne) |
|:-------:|--------|--------:|-----:|----------:|-------------------:|-------------------------:|----------------------------:|
| 0 | Jeunes clients récents | 586 | 15,0 % | 27,5 ans | 3,1 ans | 458 | 188 |
| 1 | Clients établis, achats faibles | 1 856 | 47,5 % | 47,0 ans | 11,5 ans | 165 | 74 |
| 2 | Clients établis, achats élevés | 1 466 | 37,5 % | 49,2 ans | 12,6 ans | 593 | 223 |

L'indice de Rand ajusté entre les deux segmentations vaut 0,35 : elles sont en partie différentes. Une fois les variables standardisées, l'âge et l'ancienneté deviennent déterminants. L'indice de silhouette reste faible (0,18 pour k = 3).

## Structure du dépôt

```
.
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   └── PULL_REQUEST_TEMPLATE.md
├── data/
│   └── README.md            # description du jeu de données attendu (CSV non versionné)
├── notebooks/
│   ├── 01_segmentation_clients_kmeans.ipynb
│   └── 02_segmentation_clients_kmeans_standardise.ipynb
├── .gitignore
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── LICENSE
├── README.md
└── requirements.txt
```

## Installation

Prérequis : Python 3.10 ou supérieur.

```bash
git clone https://github.com/princeobenga242-star/kpmg-customer-segmentation.git
cd kpmg-customer-segmentation

python -m venv .venv
source .venv/bin/activate        # Windows : .venv\Scripts\activate
pip install -r requirements.txt
```

Le fichier `requirements.txt` limite `scikit-learn` à une version inférieure à 1.8 : `yellowbrick` 1.5, utilisé dans le notebook 01 pour les graphiques de silhouette, ne fonctionne pas avec les versions plus récentes.

## Utilisation

1. Placer `KPMG_Customer_Demography.csv` dans le dossier `data/`.
2. Lancer Jupyter : `jupyter notebook`.
3. Ouvrir un notebook du dossier `notebooks/` et exécuter les cellules dans l'ordre.

Le notebook 01 lit le fichier depuis Google Drive (`drive.mount`), car il a été écrit sous Google Colab. Pour l'exécuter en local, remplacer le chemin par `../data/KPMG_Customer_Demography.csv` et supprimer les deux premières lignes de la cellule d'importation. Le notebook 02 lit déjà le fichier depuis `data/`.

Les sorties des notebooks sont vidées avant publication, car elles contiennent des données clients. Elles se régénèrent en exécutant les cellules.

## Limites

- L'indice de silhouette est faible dans les deux versions : les clusters sont des profils moyens, pas des catégories nettes.
- `Annual_purchase_frequency` est un multiple exact des achats sur 3 ans, et `Wealth_score` en dépend aussi : ces variables se répètent et comptent plusieurs fois dans les distances.
- Le nombre de clusters (k = 3) est fixé pour comparer les deux versions ; il n'est pas le meilleur au sens de la silhouette.
- Les résultats dépendent de la graine aléatoire et des versions des bibliothèques.

## Contribuer

Les contributions sont les bienvenues. Voir [`CONTRIBUTING.md`](CONTRIBUTING.md) et le [code de conduite](CODE_OF_CONDUCT.md).

## Licence

Ce projet est distribué sous licence MIT. Voir le fichier [`LICENSE`](LICENSE).
