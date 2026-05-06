---
name: risque-pays-cemac-uemoa
description: >
  Encode la méthodologie Damodaran de Country Risk Premium (CRP) pour les
  7 pays d'opération de ST Digital (CEMAC + UEMOA) et la méthode de
  pondération multi-pays pour dériver un WACC consolidé défendable face à
  un investisseur international. Active ce skill dès qu'on parle de WACC,
  coût du capital, taux d'actualisation, risque pays, beta, prime de
  liquidité ou fourchette de valorisation.
tags: [valorisation, wacc, risk, africa, cemac, uemoa, damodaran]
---

# Skill — Risque Pays CEMAC / UEMOA & WACC Ajusté ST Digital

## Quand utiliser ce skill

Active-toi automatiquement dès que la conversation aborde :
- Le calcul ou la justification du WACC de ST Digital
- Le Country Risk Premium (CRP) d'un ou plusieurs pays d'opération
- La prime de risque actions, le coût des fonds propres (Ke) ou le coût de la dette (Kd)
- La comparaison de taux d'actualisation avec des benchmarks sectoriels
- La sensibilité de la valorisation au WACC (tableaux de sensibilité)
- La réponse à un investisseur qui conteste le taux d'actualisation retenu

**Ne pas utiliser** pour des questions générales sur la finance d'entreprise non liées à ST Digital.

---

## 1. Architecture du WACC — Méthode officielle

Le WACC de ST Digital suit la méthode Damodaran pour marchés émergents. C'est la méthode la plus défendable face à un investisseur institutionnel africain ou international.

```
WACC = (E/(D+E)) × Ke  +  (D/(D+E)) × Kd × (1 − t)

Ke  = Rf  +  βL × (ERP_mature + CRP_pondéré)
Kd  = taux de la dette pré-impôt (marché crédit CEMAC/UEMOA)
t   = taux d'imposition effectif ST Digital
```

**Convention monétaire :** modélisation en USD. Rf = US 10Y Treasury Note (taux USD). Si modélisation en XAF/XOF, remplacer Rf par bons du Trésor BEAC/BCEAO + ajustement inflation différentielle XAF-USD (~2%).

---

## 2. Paramètres fixes — Mai 2026

### 2.1 Taux sans risque (Rf)

| Source | Valeur | Note |
|--------|--------|------|
| US 10Y T-Note (base USD) | **4,5 %** | À rafraîchir sur FRED / Bloomberg. Dernière observation : mai 2026. |
| Alternative XAF : bon Trésor CMR 5 ans | ~7,5 % | Si modélisation en FCFA — ajoute un différentiel d'inflation implicite. |
| Alternative UEMOA : bon Trésor CIV 5 ans | ~7,0 % | Idem. |

> **Règle de justification :** toujours utiliser le Rf USD si le modèle est en USD. Ne jamais mixer Rf XAF et cash flows USD — c'est l'erreur la plus courante que conteste un investisseur sophistiqué.

### 2.2 Prime de risque marché mature (ERP)

| Source | Valeur | Note |
|--------|--------|------|
| ERP implied US (Damodaran) | **4,6 %** | Damodaran.com, mis à jour mensuellement. Socle global avant ajout CRP. |
| ERP historique US (alternative conservative) | 5,5 % | Si l'investisseur préfère l'ERP historique (rare pour marchés émergents). |

### 2.3 Beta du secteur

| Profil | Beta non-levé (βU) | Source |
|--------|-------------------|--------|
| Software (Internet) — Damodaran secteur | 1,05 | Sector betas, Damodaran 2025 |
| Cloud infrastructure / Datacenter | 0,95–1,10 | Ajustement manuel : ST Digital est capex-heavy → haut de fourchette |
| **Retenu pour ST Digital** | **1,05** (βU) | Reflète le mix cloud opérationnel + infra datacenter |

**Relivrage du beta :**
```
βL = βU × (1 + (1-t) × D/E)
   = 1,05 × (1 + 0,70 × 0,25)    [structure cible 80/20, t=30%]
   = 1,05 × 1,175
   ≈ 1,23
```

> **Note pédagogique :** le beta levé de 1,23 est légèrement supérieur au 1,10 utilisé dans le test du 6 mai (qui avait utilisé un βL direct sans relier). La méthode complète est plus défendable en due diligence.

---

## 3. Country Risk Premium (CRP) — 7 pays ST Digital

### 3.1 Méthode Damodaran de dérivation du CRP

```
CRP = Spread_souverain_défaut × (σ_actions_local / σ_obligations_local)

Proxy pratique (Damodaran) :
CRP ≈ Spread_souverain × 1,5
```

Le spread souverain est lu sur les CDS 5 ans ou estimé depuis la notation Moody's / S&P via la table de correspondance Damodaran (Country Default Spreads and Risk Premiums, mise à jour janvier de chaque année).

### 3.2 CRP par pays — Référence mai 2026

| Pays | Notation (Moody's / S&P) | Spread souverain estimé | **CRP retenu** | Intervalle de confiance |
|------|--------------------------|------------------------|----------------|------------------------|
| **Côte d'Ivoire** | Ba3 / B+ | ~4,5 % | **6,5 %** | 6,0–7,5 % |
| **Gabon** | Caa1 / unrated | ~5,5 % | **8,0 %** | 7,0–9,5 % |
| **Bénin** | B1 / B+ | ~5,5 % | **8,0 %** | 7,0–9,0 % |
| **Togo** | non noté formellement | ~6,0 % (proxy Bénin+) | **9,0 %** | 8,0–10,5 % |
| **Cameroun** | B2 / B | ~6,5 % | **9,5 %** | 8,5–11,0 % |
| **Congo-Brazzaville** | Caa2 / CCC+ | ~8,0 % | **12,0 %** | 10,5–14,0 % |
| **RDC** | Caa2 / CCC | ~9,5 % | **14,0 %** | 12,0–16,5 % |

> **Source de référence :** Damodaran, "Country Default Spreads and Risk Premiums", janvier 2026 (damodaran.com). À rafraîchir chaque trimestre. Les valeurs ci-dessus sont les meilleures estimations disponibles en mai 2026 — à valider avec le tableau Damodaran du mois courant avant toute présentation formelle.

> **Alerte Congo & RDC :** CRP > 12 % implique un WACC > 22 % pour ces juridictions prises isolément. Si leur poids dans le revenu consolidé ST Digital est > 15 %, la valorisation consolidée se dégrade significativement. Recommandation : toujours calculer le WACC avec et sans ces deux pays pour mesurer leur impact.

### 3.3 CRP pondéré — Méthode de calcul

Le CRP consolidé du Groupe ST Digital est la moyenne pondérée des CRP pays par la part de chaque pays dans le chiffre d'affaires total.

```
CRP_consolidé = Σ (part_revenu_pays_i × CRP_pays_i)
```

**Template de calcul (à alimenter avec les données DAF) :**

| Pays | CRP | Part CA (%) | Contribution |
|------|-----|-------------|-------------|
| Côte d'Ivoire | 6,5 % | ___ % | ___ |
| Gabon | 8,0 % | ___ % | ___ |
| Bénin | 8,0 % | ___ % | ___ |
| Togo | 9,0 % | ___ % | ___ |
| Cameroun | 9,5 % | ___ % | ___ |
| Congo | 12,0 % | ___ % | ___ |
| RDC | 14,0 % | ___ % | ___ |
| **TOTAL** | | **100 %** | **= CRP_consolidé** |

**Simulation indicative (structure revenue hypothétique) :**

Si Cameroun 45% + CIV 25% + Gabon 15% + Togo 8% + Bénin 5% + Congo 2% :
```
CRP_consolidé = 0,45×9,5 + 0,25×6,5 + 0,15×8,0 + 0,08×9,0 + 0,05×8,0 + 0,02×12,0
             = 4,275 + 1,625 + 1,200 + 0,720 + 0,400 + 0,240
             = 8,46 %
```
→ Nettement inférieur au CRP Cameroun seul (9,5 %) : la diversification géographique vers CIV et Gabon **réduit le risque consolidé**. C'est un argument de pitch à valoriser.

---

## 4. Coût des fonds propres (Ke) — Dérivation complète

```
Ke = Rf  +  βL × (ERP_mature + CRP_consolidé)
   = 4,5 % + 1,23 × (4,6 % + 8,46 %)
   = 4,5 % + 1,23 × 13,06 %
   = 4,5 % + 16,06 %
   = 20,56 %
```

> **Comparaison avec le test du 6 mai (Ke = 20,0 %) :** légèrement supérieur car beta levé plus précis (1,23 vs 1,10) et CRP pondéré vs CRP Cameroun seul. En pratique la différence est < 1 point de WACC — l'ordre de grandeur est confirmé.

---

## 5. Coût de la dette (Kd)

| Composante | Valeur | Source |
|-----------|--------|--------|
| Taux crédit corporate XAF zone CEMAC | 10–13 % | BEAC, observé sur Afriland, Société Générale Cameroun, BGFI 2025 |
| Taux crédit corporate XOF zone UEMOA | 9–12 % | BCEAO, Coris Bank, Ecobank 2025 |
| Financement DFI en USD (IFC, Proparco) | 7–9 % | Taux préférentiels DFI + spread crédit, disponible pour acteurs certifiés |
| **Retenu (blended) :** | **11 %** | Pondéré entre financement local CEMAC et éventuel DFI |

> **Levier DFI :** si ST Digital sécurise un financement IFC ou Proparco (possible vu le profil souveraineté + Anthropic Partner), le Kd peut descendre vers 8–9 %, réduisant le WACC de ~0,5 point. C'est un argument à valoriser en négociation.

---

## 6. Structure de capital cible

| Paramètre | Valeur retenue | Justification |
|-----------|---------------|---------------|
| Pondération Equity (E/(D+E)) | **80 %** | Tech growth africaine, levée prévue avant dette substantielle. Comparables : Raxio, MainOne en phase growth ≈ 75–85 % equity. |
| Pondération Dette (D/(D+E)) | **20 %** | Cohérent avec bilan actuel ; peut évoluer vers 30–40 % post-levée si refinancement DFI. |
| Taux d'imposition effectif (t) | **30 %** | Composite : IS Cameroun 33 %, CIV 25 %, Gabon 30 %, Togo 27 %, Bénin 30 %, Congo 30 %, RDC 30 %. Effectif après crédits et régimes préférentiels ≈ 28–32 %. Retenu 30 % pour la modélisation. |

---

## 7. WACC consolidé ST Digital — Synthèse

```
WACC = 0,80 × 20,56 % + 0,20 × 11,0 % × (1 − 0,30)
     = 0,80 × 20,56 % + 0,20 × 7,70 %
     = 16,45 % + 1,54 %
     = 17,99 %

Arrondi opérationnel : 18 %
```

**Fourchette de sensibilité selon la structure revenue pays :**

| Scénario | Description | CRP pondéré | WACC |
|----------|-------------|-------------|------|
| Optimiste | Forte part CIV + Gabon (pays mieux notés) | ~7,8 % | ~17,0 % |
| **Base** | **Distribution actuelle estimée** | **~8,5 %** | **~18,0 %** |
| Pessimiste | Croissance accélérée Congo + RDC | ~9,5 % | ~19,5 % |
| Stress | Congo + RDC > 20 % du CA | ~10,5 % | ~21,0 % |

> **Règle de communication investisseur :** présenter le WACC comme une fourchette (17–19 %), pas un point unique. Cela montre la rigueur analytique et anticipe les objections sur les hypothèses pays.

---

## 8. Primes additionnelles — Non-cotation & taille

Pour un acteur non coté de taille mid-cap africaine, deux primes s'ajoutent selon certains investisseurs. Elles sont **facultatives mais défendables** en DCF — les inclure ou non selon l'audience :

| Prime | Valeur indicative | Justification |
|-------|-------------------|---------------|
| **Prime de taille (Size premium)** | +1,5 à +3,0 % | Duff & Phelps / Kroll CRSP decile 10 (micro-cap) : +3 %. Acteur africain non coté de 10–30 M USD CA = micro/small cap. |
| **Prime d'illiquidité** | +1,0 à +2,0 % | Compense l'absence de liquidité de la participation. Standard pour PE africain. |
| **WACC avec primes additionnelles** | ~21–23 % | Fourchette haute — utilisée par les fonds PE prudents. |

> **Stratégie de négociation :** présenter d'abord le WACC sans primes additionnelles (18 %) comme cas de base, et mentionner que vous avez modélisé le cas avec primes (21–23 %) en sensibilité. Cela montre que vous avez anticipé l'argument de l'investisseur sans vous l'imposer d'emblée.

---

## 9. Scripts de réponse face à un investisseur

**Objection typique :** *"Votre WACC à 18 % est trop bas pour l'Afrique sub-saharienne."*

**Réponse structurée :**
> "Notre WACC de 18 % est dérivé de la méthode Damodaran (janvier 2026) : Rf USD 4,5 %, ERP mature 4,6 %, CRP pondéré 8,5 % calculé sur les 7 juridictions d'opération pondérées par le chiffre d'affaires, beta sectoriel relevé à 1,23. Le WACC Cameroun seul serait de 19,5 %. La diversification géographique, en particulier la part significative de la Côte d'Ivoire (mieux notée, Ba3), réduit le CRP consolidé. Nous avons également modélisé le cas avec prime de taille et prime d'illiquidité (WACC ~21 %), disponible en sensibilité."

**Objection typique :** *"Pourquoi ne pas avoir utilisé le taux sans risque local ?"*

**Réponse structurée :**
> "Le modèle est en USD pour cohérence avec les comparables et les éventuels investisseurs internationaux. Si la modélisation en FCFA était requise, nous substituerions le Rf par le taux des bons du Trésor BEAC à 5 ans (~7,5 %) et ajusterions les cash flows en conséquence — le résultat final converge si la prime d'inflation différentielle est correctement modélisée."

---

## 10. Checklist avant toute présentation du WACC

- [ ] Rf : date de l'observation vérifiée (ne pas utiliser un taux > 3 mois)
- [ ] CRP : tableau Damodaran de l'année en cours chargé et référencé
- [ ] Pondération pays : basée sur la répartition revenu réelle (données DAF)
- [ ] Beta : source Damodaran sector betas (année en cours) citée
- [ ] Structure de capital : cohérente avec le bilan réel ou la structure cible post-levée
- [ ] Taux d'imposition : effectif (pas le taux statutaire)
- [ ] Sensibilité : matrice WACC × g terminal produite et jointe
- [ ] Primes additionnelles : documentées séparément, disponibles en annexe
