---
name: review-implementation
description: Relit le ticket d'une story et vérifie son implémentation critère par critère, puis pose des questions sur ce qui a pu être oublié. À utiliser quand l'utilisateur dit « review-implementation », « vérifie la story », ou avant d'ouvrir une PR. Ne modifie aucun fichier.
---

# Vérifier l'implémentation d'une story

Tu relis, tu ne corriges pas. Aucun fichier n'est modifié pendant ce skill.

## 1. Retrouver le ticket

- Demande le ticket de la story (numéro et texte) s'il n'est pas déjà dans la conversation.
- Ne reconstitue jamais un ticket de mémoire ou à partir du code.
- Recopie la liste numérotée de ses critères d'acceptation.

## 2. Lire l'implémentation

- Lance `git branch --show-current` puis `git diff development...HEAD` pour voir ce que la branche change.
- Si le diff est vide, demande quels fichiers relire.

## 3. Vérifier critère par critère

Pour chaque critère d'acceptation, donne :

| Critère | Statut | Où (fichier:ligne) | Test qui le couvre |
|---------|--------|--------------------|--------------------|

- Statut : ✅ fait · ⚠️ partiel · ❌ absent.
- « Test qui le couvre » : nom du test, ou « aucun ».
- Signale aussi le code ajouté qu'aucun critère ne demande.

## 4. Questionner ce qui a pu être oublié

Pose des questions, sans donner la réponse. Ne retiens que celles qui concernent cette story.

- **Sécurité** : quel rôle a le droit d'appeler cette action ? Un utilisateur peut-il lire ou modifier la ressource d'un autre ? Quelles entrées sont refusées ? Si le ticket ne le dit pas, demande le critère manquant : ne l'invente pas.
- **Cas d'erreur** : que se passe-t-il si la ressource n'existe pas, si l'entrée est vide, invalide ou en double ?
- **Données** : le schéma de la base a-t-il changé ? Le MCD/MLD dans `docs/` est-il à jour ?
- **Front** : chaque champ a-t-il un label ? Le parcours fonctionne-t-il au clavier ? Les messages d'erreur sont-ils liés au champ ?
- **Tests** : quel critère n'a aucun test ? Quel cas limite mériterait le sien ?

## 5. Conclure

- Une phrase de bilan : la story est-elle prête pour une PR ?
- La liste des questions qui attendent une réponse de l'utilisateur.
