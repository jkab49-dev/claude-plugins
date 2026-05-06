# Plugin — ST Digital Valorisation

Agent de coaching et de modélisation pour la valorisation de ST Digital.

**Prérequis :** installer d'abord `financial-analysis` depuis le marketplace Anthropic Financial Services.

## Skills inclus

| Skill | Description |
|-------|-------------|
| `risque-pays-cemac-uemoa` | CRP Damodaran pour les 7 pays d'opération + WACC consolidé pondéré |
| `comps-cloud-africain` | Comparables boursiers et transactionnels cloud africain + grille de multiples |

## Skills à venir

| Skill | Description | Statut |
|-------|-------------|--------|
| `coaching-valorisation-pdg` | Mode coaching pédagogique — challenge hypothèses, questions socratiques | En construction |
| `narrative-valeur-st-digital` | Leviers de prime souveraineté + partenariat Anthropic | En construction |
| `dcf-st-digital` | Modèle DCF pré-configuré pour le profil ST Digital (WACC, CRP, CapEx infra) | En construction |

## Usage

Une fois installé dans Cowork ou Claude Code, les skills s'activent automatiquement
dès que la conversation aborde les sujets couverts (WACC, comparables, multiples, CRP).

Utiliser en combinaison avec le plugin `financial-analysis` pour générer les workbooks
Excel et les tableaux de sensibilité.

## Installation

```bash
# Dans Claude Code
/plugin marketplace add anthropics/financial-services-plugins
/plugin install financial-analysis@financial-services-plugins

# Installer ce plugin depuis le dossier local
/plugin marketplace add ./st-digital-valorisation
/plugin install st-digital-valorisation@st-digital-valorisation
```

Dans **Cowork** : Customize > + > Add marketplace from GitHub (une fois publié sur GitHub ST Digital).
