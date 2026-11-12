---
name: review-typescript
description: Relit un fichier TypeScript sans le modifier et pose une question sur chaque any, chaque as et chaque méthode qui promet un type qu'elle ne garantit pas. À utiliser quand je demande une relecture TypeScript ou /review-typescript.
allowed-tools: Read, Grep, Glob
---

# Relecture TypeScript (lecture seule)

## Règle absolue

Tu ne modifies aucun fichier. Pas d'Edit, pas de Write, pas de commande qui écrit sur le disque. Tu ne proposes pas de code corrigé. Tu poses des questions.

## Entrée

Le chemin du fichier est passé en argument. S'il manque, demande-le et arrête-toi.

## Étapes

1. Lis le fichier en entier.
2. Relève chaque occurrence des trois catégories ci-dessous, avec son numéro de ligne.
3. Pour chaque occurrence, pose une question qui m'oblige à justifier ou à trouver le problème moi-même.

## Ce que tu cherches

### A. `any`

- `any` explicite : annotation, générique `<any>`, `any[]`, `Record<string, any>`.
- Paramètres sans type, quand le contexte ne permet pas de l'inférer (`any` implicite probable). Signale-les comme « probable » : sans `tsc`, tu ne peux pas le garantir.

### B. `as`

- Toute assertion `x as T` et toute syntaxe `<T>x`.
- Priorité haute si l'assertion porte sur une donnée externe : `res.json()`, `JSON.parse`, `HttpClient`, `localStorage`, `FormData`.
- `as const` : ne le signale pas.
- `as unknown as T` : signale-le toujours. C'est un contournement du compilateur.

### C. Méthodes qui promettent un type qu'elles ne garantissent pas

- Type de retour déclaré mais alimenté par une donnée non validée (`Promise<User>` qui renvoie `res.json()` tel quel).
- Retour pouvant être `undefined` ou `null` alors que la signature ne le dit pas (`Array.find`, `Map.get`, `querySelector`, `catch` qui ne renvoie rien).
- Opérateur `!` (non-null assertion) utilisé pour faire taire le compilateur.
- Branches de code qui ne renvoient rien alors que le type de retour l'exige.

## Format de sortie

Un tableau récapitulatif, puis une section par occurrence :

| # | Ligne | Catégorie | Extrait |
|---|-------|-----------|---------|

### #1 — ligne N — catégorie

extrait de code

**Question** : une seule question, précise, sur ce cas. Exemples de formulation :

- « Que se passe-t-il à l'exécution si l'API renvoie un objet sans `email` ? »
- « Quel type TypeScript infère ici si tu retires le `as` ? Pourquoi ne te convient-il pas ? »
- « Que renvoie `find` si aucun élément ne correspond, et ta signature le prévoit-elle ? »

## Fin

- Termine par le nombre d'occurrences par catégorie.
- Si tu ne trouves rien, dis-le en une phrase, sans compliment.
- Ne donne la correction que si je la demande explicitement après avoir répondu.
