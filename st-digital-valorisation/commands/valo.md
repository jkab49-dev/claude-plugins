---
description: Lance le coaching valorisation ST Digital — cadrage, hypothèses, fourchette défendable.
argument-hint: [contexte optionnel : "réunion PE", "levée fonds", "valorisation interne"]
---

# Commande /valo — Coaching Valorisation ST Digital

Déclenche le mode coach valorisation pour ST Digital.

## Instructions

Lance immédiatement le skill `coaching-valorisation-pdg` avec la question d'entrée appropriée selon le contexte fourni en argument.

**Si un argument est fourni** (ex: `/valo réunion PE dans 2 semaines`) :
- Identifier le contexte (urgence, type d'interlocuteur, délai)
- Démarrer directement à l'Étape 0 du skill coaching
- Poser la première question de cadrage sans introduction longue

**Si aucun argument** (`/valo` seul) :
- Poser la question d'entrée standard :
  > *"Pour qu'on travaille efficacement : est-ce que vous avez déjà une idée du prix que vaut ST Digital — même approximative — ou c'est justement la question centrale ?"*

**Dans tous les cas :**
- Activer les skills `risque-pays-cemac-uemoa`, `comps-cloud-africain`, `narrative-valeur-st-digital` et `dcf-st-digital` en arrière-plan
- Ne jamais livrer de fourchette sans avoir traversé les étapes de cadrage
- Adapter la langue à celle de l'utilisateur (français par défaut pour ST Digital)

## Exemples d'usage

```
/valo
/valo réunion fonds PE dans 2 semaines
/valo préparer session PDG 26 mai
/valo levée de fonds 5M USD
/valo valorisation interne CoMEX
```
