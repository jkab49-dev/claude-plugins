---
name: dcf-st-digital
description: >
  Modèle DCF pré-configuré pour ST Digital. Encode les paramètres
  financiers de référence (WACC, structure de capital, taux IS,
  conventions de modélisation) pour que chaque session de valorisation
  reparte des bons paramètres ST Digital sans re-saisie. Active ce
  skill dès qu'on demande de construire, mettre à jour ou discuter
  le modèle financier DCF de ST Digital.
tags: [dcf, valorisation, modele, financier, wacc, fcf, excel]
---

# Skill — DCF ST Digital (Paramètres Pré-Configurés)

## Principe

Ce skill évite de partir de zéro à chaque session. Il encode les conventions, paramètres et choix méthodologiques spécifiques à ST Digital pour que le modèle DCF soit cohérent d'une session à l'autre. Tous les paramètres peuvent être mis à jour dès que les vraies données DAF sont disponibles — les valeurs encodées ici sont des références de départ défendables.

---

## 1. Paramètres WACC — Référence ST Digital

Ces paramètres découlent du skill `risque-pays-cemac-uemoa`. Les reproduire ici pour accès rapide.

```
WACC ST DIGITAL — RÉFÉRENCE MAI 2026

Rf (US 10Y T-Note)              : 4,5 %
ERP mature (Damodaran implied)  : 4,6 %
CRP pondéré 7 pays (base)       : 8,5 %   ← à recalibrer avec % CA réels DAF
Beta non-levé (sector cloud)    : 1,05
Beta levé (structure 80/20)     : 1,23
Ke (coût fonds propres)         : 20,6 %
Kd pré-impôt (CEMAC)            : 11,0 %
Taux IS effectif                : 30,0 %
Kd après impôt                  : 7,7 %
Pondération equity              : 80 %
Pondération dette               : 20 %

WACC CONSOLIDÉ                  : 18,0 %  (arrondi opérationnel)
Fourchette défendable           : 17 – 19 %
```

> **Mise à jour obligatoire :** dès que le DAF confirme la répartition CA par pays, recalculer le CRP pondéré dans le skill `risque-pays-cemac-uemoa` et mettre à jour le WACC ici.

---

## 2. Structure du modèle — Conventions ST Digital

### 2.1 Devise et unités
- **Devise :** USD millions (USD M)
- **Arrondi :** 3 décimales dans les calculs, 1 décimale dans les présentations
- **Taux de change XAF/USD :** 655,96 (fixe BEAC) — si modélisation en FCFA nécessaire, appliquer ce taux sur les données Odoo avant import

### 2.2 Horizon et convention temporelle
- **Horizon explicite :** 6 ans (2026 – 2031) — standard pour un acteur cloud capex-heavy en phase de croissance
- **Convention :** end-of-year discounting (FCFF 2026 actualisé à t=1)
- **Année de référence :** 2025A (actuel) — à alimenter avec les vraies données DAF
- **Valeur terminale :** Gordon-Shapiro + cross-check multiple de sortie

### 2.3 Croissance terminale
- **Taux retenu :** 5,0 % (cas de base)
- **Fourchette sensibilité :** 3,0 % – 8,0 %
- **Règle de prudence :** g terminal ≠ dernière croissance explicite. Le 8% de croissance en 2031 est la dernière année de la phase explicite — le g perpétuel est distinct et doit être inférieur au WACC d'au moins 5 points.

---

## 3. Hypothèses de croissance — Scénarios ST Digital

### 3.1 Trajectoire de croissance du chiffre d'affaires

```
                2026    2027    2028    2029    2030    2031
Bear           12%     12%     10%      8%      7%      5%
Base           25%     25%     20%     15%     10%      8%
Bull           35%     35%     30%     20%     14%      8%
```

**Justifications :**
- **Bear (12% plateau) :** concurrence accrue Orange/MTN cloud local, ralentissement économique CEMAC, retard expansion CIV/RDC.
- **Base (25% puis dégressif) :** trajectoire historique ST Digital confirmée, expansion géographique maîtrisée, montée en charge CloudStore.
- **Bull (35% initial) :** accélération post-partenariat Anthropic, contrats IA souveraine gouvernementaux, accélération CIV.

### 3.2 Marge EBITDA — Trajectoire cible

```
                2025A   2026    2027    2028    2029    2030    2031
Bear            20%     18%     18%     16%     16%     15%     14%
Base            20%     20%     21%     22%     24%     26%     28%
Bull            20%     22%     24%     26%     28%     30%     32%
```

**Note Bear :** guerre des prix sur le segment cloud infrastructure, coûts énergétiques en hausse, pression salariale sur les profils tech locaux.

**Note Bull :** effet d'échelle CloudStore (revenu marginal quasi-nul), mix favorable vers conseil IA (marge > 50%), tarification premium souveraineté.

### 3.3 CapEx — Intensité par phase

```
                2025A   2026    2027    2028    2029    2030    2031
Bear            18%     18%     17%     16%     16%     15%     14%
Base            18%     16%     14%     12%     11%     10%     10%
Bull            16%     12%     10%      9%      8%      8%      8%
```

**Logique :** CapEx décroissant en % du CA reflète l'amortissement progressif de l'infrastructure existante. En cas d'expansion RDC ou nouveau pays, le CapEx repart ponctuellement à la hausse — modéliser comme choc exogène, pas comme tendance.

### 3.4 Autres hypothèses fixes

```
D&A (% CA)              : 10 – 11 % (reflet amortissement datacenter 10 ans)
BFR / CA                : 8 % (clientèle B2B institutionnelle, DSO ~90 jours)
Taux IS effectif        : 30 %
```

---

## 4. Construction du Free Cash Flow to Firm (FCFF)

```
FCFF = EBITDA
     − D&A
     = EBIT
     − Impôt sur EBIT (= EBIT × 30%)
     = NOPAT
     + D&A (réintégration)
     − CapEx
     − Variation BFR (= Δ CA × BFR%)
     = FCFF
```

**Points d'attention spécifiques à ST Digital :**
- La variation BFR est importante chez ST Digital car la clientèle B2B institutionnelle a des délais de paiement longs (administrations, banques). En phase de forte croissance, la variation BFR est un drain de cash significatif.
- Le CapEx inclut UNIQUEMENT les investissements en infrastructures propres (serveurs, équipements réseau, datacenter). Les coûts de licences éditeurs (Microsoft, Fortinet) sont des OPEX, pas des CAPEX.
- La R&D capitalisée (Projet Nyong, développements CloudStore) peut être traitée en CAPEX — à clarifier avec le DAF selon le traitement comptable SYSCOHADA retenu.

---

## 5. Valeur terminale — Double méthode

### 5.1 Gordon-Shapiro (méthode principale)

```
TV = FCFF₂₀₃₁ × (1 + g) / (WACC − g)
   = FCFF₂₀₃₁ × (1 + 5%) / (18% − 5%)
   = FCFF₂₀₃₁ × 1,05 / 0,13
   = FCFF₂₀₃₁ × 8,08x
```

> **Alerte :** si TV > 75% de l'EV totale, le modèle dépend trop des hypothèses perpétuelles. Action : allonger l'horizon explicite à 7 ou 8 ans, ou réduire g.

### 5.2 Multiple de sortie (cross-check)

```
TV = EBITDA₂₀₃₁ × Multiple_sortie

Multiples de sortie défendables pour ST Digital :
  Bear  : 6,0x EBITDA (multiple comps trading africains, décote conservatrice)
  Base  : 9,0x EBITDA (médiane comps trading + transactions africaines)
  Bull  : 13,0x EBITDA (prime MainOne-like pour acteur Afrique de l'Ouest)
```

**Règle de convergence :** si Gordon-Shapiro et multiple de sortie divergent de plus de 20%, investiguer quelle hypothèse crée l'écart (g trop élevé ? multiple trop agressif ?). Présenter les deux en sensibilité.

---

## 6. Instructions de génération du workbook Excel

Quand on demande de générer le modèle financier ST Digital en Excel, utiliser OBLIGATOIREMENT les conventions suivantes :

### 6.1 Structure des onglets (dans cet ordre)
1. **README** — titre, date, auteur, synthèse de valorisation, lecture du modèle
2. **Hypotheses** — TOUS les inputs en bleu, avec colonne de justification à droite
3. **WACC** — décomposition complète avec sources citées
4. **Projections** — P&L → EBITDA → EBIT → NOPAT → FCFF, 2025A à 2031
5. **Valorisation** — actualisation FCFF + TV Gordon + TV multiple + EV synthèse
6. **Sensi_WACC_g** — matrice EV × WACC (13% → 22%) × g (3% → 8%)
7. **Sensi_Ops** — matrice EV × marge EBITDA × CapEx%
8. **Scenarios** — Bear / Base / Bull avec switch automatique des hypothèses

### 6.2 Conventions de couleur obligatoires
- **Texte bleu** : inputs hardcodés (chiffres que l'utilisateur peut changer)
- **Texte noir** : formules et calculs (jamais hardcoder un calcul)
- **Texte vert** : liens depuis un autre onglet
- **Fond jaune** : cellule à mettre à jour avec vraies données DAF

### 6.3 Conventions de formatage
- Années en texte (2025, pas 2,025)
- Montants en $M avec 1 décimale : 12,5
- Pourcentages avec 1 décimale : 18,0%
- Négatifs en parenthèses : (2,5) et non -2,5
- Zéros affichés comme tiret : —

### 6.4 Pré-remplissage ST Digital
Au lancement du modèle, pré-remplir automatiquement :
- WACC = 18% (fourchette 17-19%)
- Structure capital = 80/20 equity/dette
- Taux IS = 30%
- CRP = 8,5% (à mettre en jaune — à confirmer avec DAF)
- Beta levé = 1,23
- D&A = 10% du CA
- BFR = 8% du CA
- g terminal = 5% (à mettre en jaune)

---

## 7. Réconciliation DCF vs. Comparables — Tableau de synthèse

À inclure systématiquement dans toute présentation de valorisation ST Digital :

```
FOURCHETTE DE VALORISATION ST DIGITAL — SYNTHÈSE (données [date])

                        Bear        Base        Bull
DCF (EV, USD M)         0 – 5       7 – 15      40 – 50
Comps trading (EV)      8 – 12      15 – 22     25 – 35
Comps transactions(EV)  12 – 18     18 – 28     30 – 45

RECOMMANDATION
Fourchette de négociation (base) : 15 – 25 M USD EV
Equity Value (après net debt)    : EV − [dettes nettes DAF]
Ancre haute (narrative Bull)     : 35 – 45 M USD EV

NOTE : toutes les fourchettes sont indicatives et basées sur
[données synthétiques / données DAF confirmées — préciser].
À mettre à jour après réception du P&L 2025 consolidé.
```

---

## 8. Questions à poser systématiquement avant de modéliser

Avant de générer tout workbook DCF, vérifier que ces 5 points sont confirmés :

1. **Année de référence :** quel est le CA 2025 réel consolidé (pas estimé) ?
2. **EBITDA :** l'EBITDA est-il retraité (hors charges exceptionnelles et rémunérations non-récurrentes) ?
3. **CapEx :** distinction faite entre CapEx infrastructure et CapEx R&D capitalisée ?
4. **Répartition CA par pays :** pour calibrer le CRP pondéré réel ?
5. **Net debt :** position de trésorerie nette pour calculer l'Equity Value ?

Si l'un de ces 5 points manque, le modéliser avec les hypothèses de référence de ce skill ET marquer les cellules concernées en jaune (à mettre à jour).
