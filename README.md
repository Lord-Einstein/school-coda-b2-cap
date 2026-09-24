# Module CAP — Concevoir une API REST

Coda Dijon, B2.

Ce dépôt regroupe les ateliers du module. Chaque atelier vit dans son propre dossier,
avec ses ressources et son `README.md` : ouvrez celui de l'atelier en cours, il vous
dit tout ce dont vous avez besoin.

## Les ateliers

| Dossier | Atelier |
|---|---|
| [`atelier-curl-quests/`](atelier-curl-quests/) | Les fondamentaux HTTP avec curl |
| [`atelier-bruno/`](atelier-bruno/) | Prendre en main une API existante avec Bruno |
| [`atelier-analyse/`](atelier-analyse/) | Inventorier les points d'entrée du projet |
| [`atelier-openapi/`](atelier-openapi/) | Concevoir le contrat OpenAPI du projet |

## Le linter

Le ruleset **Spectral** est fixé à la racine dans `.spectral.yaml` — il vaut pour tout
le dépôt, et l'extension VSCode le trouve en remontant depuis n'importe quel dossier.
Il est le même pour tout le monde : ce qui passe chez vous passe à la correction.

## Notes évaluation OpenAPI - Déclaration d'utilisation de l'IA

Conformément aux consignes du module, j'ai utilisé une IA générative (Gemini) comme assistant de développement pour :

- **Compréhension des conventions OpenAPI :** Clarification sur la déclaration des paramètres dans l'URL (`in: path` pour les identifiants `{id}`) et des filtres de requêtes (`in: query` sur les opérations `GET`).
- **Modélisation et syntaxe des corps de requêtes :** Aide à la structuration des schémas JSON pour les corps de requêtes (`requestBody`), notamment pour les opérations d'ajout au panier et de paiement.
- **Tests et débogage :** Génération d'exemples de `body` JSON pour tester les routes dans Bruno et valider le comportement du mock Prism.

Le contrôle de la syntaxe et la validation de la conformité du contrat ont été assurés via `spectral lint` et les tests d'intégration sur Bruno.