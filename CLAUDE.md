# MoodGo

Projet de groupe, formation CDA GRETA Tours.

## Conventions

- Code en anglais. Commentaires en français, messages de commit en anglais.
- Branches `feat/<ticket>-<desc>` et `fix/<ticket>-<desc>`, créées depuis `development`.
- Messages de commit au format Conventional Commits (`feat:`, `fix:`, `refactor:`, `docs:`).
- Une PR par user story, avec au moins une relecture.

## Périmètre et Git

- Modifier uniquement les fichiers liés à la tâche demandée. Signaler les problèmes vus ailleurs sans les corriger.
- Ne jamais exécuter `git commit`, `push`, `rebase`, `reset` ou `merge`. Proposer les commandes sans les lancer.
- Ne force jamais un push (`git push --force`).
- Demander avant de modifier `application.yml`, `docker-compose.yml`, `.env*` ou `pom.xml`.
- Demander avant de modifier le schéma de la base (entités JPA, scripts SQL). Le MCD/MLD dans `docs/` fait référence.

## Méthode de travail

- Préfère des fonctions courtes.
- Avant une modification touchant plus de 3 fichiers, présenter un plan et attendre validation.
- Ne pas générer une feature complète (controller, service, repository, front) d'un coup : procéder couche par couche.
- Après une modification, indiquer la commande de test à lancer pour la vérifier.
- Tout membre de l'équipe doit pouvoir expliquer le code produit : préférer une solution simple et lisible à une solution compacte.
- Ne jamais supprimer ou désactiver un test pour le faire passer.

## Sécurité

- Avant de coder une story, relis ses critères d'acceptation ; s'il en manque un sur la sécurité, demande-le-moi au lieu de l'inventer.
- Aucun secret dans le code ni dans ce fichier : clé JWT, mots de passe et URL de base de données passent par des variables d'environnement.

## Front-end

- TypeScript strict : pas de `any`.

## Accessibilité

- HTML sémantique : `button` pour une action, `a` pour une navigation, `label` associé à chaque champ.
- Tout est utilisable au clavier, avec un focus visible.
- Textes alternatifs pertinents, contrastes conformes WCAG AA, messages d'erreur de formulaire liés au champ (`aria-describedby`).

## TypeScript

- Jamais de `any`, ni explicite ni par paresse. Si le type est inconnu, utiliser `unknown` puis le restreindre (type guard, `typeof`, `in`).
- Jamais de `as` pour forcer le type d'une réponse réseau (`fetch`, `res.json()`, `JSON.parse`, `HttpClient`). Valider la donnée avec un type guard avant de la typer. `as const` reste autorisé.
- `type` pour décrire des données (DTO, réponses d'API, unions). `interface` pour les contrats de méthodes qu'une classe implémente (`implements`).
