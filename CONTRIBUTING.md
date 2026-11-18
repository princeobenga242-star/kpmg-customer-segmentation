# Contribuer

Merci de votre intérêt pour ce projet.

## Signaler un problème ou proposer une amélioration

Ouvrir un ticket (*issue*) en utilisant l'un des modèles proposés (anomalie ou nouvelle fonctionnalité). Décrire le contexte, les étapes pour reproduire le problème et le résultat attendu.

## Proposer une modification

1. Créer une branche à partir de `main` : `git checkout -b feat/nom-de-la-modification`.
2. Faire les modifications, puis vérifier que les notebooks s'exécutent de bout en bout.
3. Ouvrir une *pull request* vers `main` en remplissant le modèle fourni.

## Messages de commit

Les messages suivent la convention [Conventional Commits](https://www.conventionalcommits.org/fr/) et sont rédigés en français, à la forme nominale.

```
type: description courte
```

Types usuels : `feat` (nouvelle fonctionnalité), `fix` (correction), `docs` (documentation), `refactor` (réorganisation du code), `chore` (maintenance).

Exemples :

- `feat: ajout de la standardisation des variables numériques`
- `fix: correction du chemin de lecture du fichier CSV`
- `docs: mise à jour de la section Installation`

## Règles pour les notebooks

- **Vider les sorties** des cellules avant de commiter : elles peuvent contenir des données clients.
- Ne jamais versionner le fichier CSV ni aucune donnée personnelle.
- Exécuter le notebook de haut en bas avant de commiter, pour vérifier qu'il ne dépend pas de l'ordre d'exécution.
- Fixer la graine aléatoire (`random_state`) pour que les résultats soient reproductibles.
