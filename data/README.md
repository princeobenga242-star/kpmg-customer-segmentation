# Données

Ce dossier accueille le fichier `KPMG_Customer_Demography.csv`. Il n'est **pas versionné** (voir `.gitignore`) : il contient des données clients (noms, adresses, dates de naissance).

Placer le fichier ici avant d'exécuter les notebooks : `data/KPMG_Customer_Demography.csv`.

## Colonnes attendues

| Colonne | Description | Utilisée dans les notebooks |
|---------|-------------|:---------------------------:|
| `customer_id` | Identifiant du client | Supprimée |
| `First_Name` | Prénom | Supprimée |
| `Last_Name` | Nom | Supprimée |
| `gender` | Genre | Oui |
| `DOB` | Date de naissance | Supprimée |
| `AGE` | Âge en années | Oui |
| `past_3_years_bike_related_purchases` | Nombre d'achats de vélos sur 3 ans | Oui |
| `job_industry_category` | Secteur d'activité | Oui |
| `wealth_segment` | Segment de richesse (Mass Customer, Affluent Customer, High Net Worth) | Oui |
| `deceased_indicator` | Client décédé (Y/N) | Supprimée |
| `tenure` | Ancienneté, en années | Oui |
| `Owns_Car` | Possède une voiture (Yes/No) | Oui |
| `Address` | Adresse | Supprimée |
| `Postcode` | Code postal | Supprimée |
| `State` | État | Oui |
| `Country` | Pays | Supprimée |
| `property_valuation` | Valeur de la propriété (échelle de 1 à 12) | Oui |
