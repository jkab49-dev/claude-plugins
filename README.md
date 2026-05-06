# claude-plugins — Jacques Ludovic / ST Digital

Marketplace de plugins Claude personnels. Compatible **Cowork** et **Claude Code**.

## Installation

### Dans Cowork
Customize → + → Add marketplace from GitHub → `jkab49/claude-plugins`

### Dans Claude Code
```bash
/plugin marketplace add jkab49/claude-plugins
```

## Plugins disponibles

| Plugin | Description | Version |
|--------|-------------|---------|
| `st-digital-valorisation` | Agent coaching valorisation ST Digital — WACC CEMAC/UEMOA, comparables cloud africain | v0.1.0 |

## Roadmap

- `st-digital-valorisation` v0.2 — skill `coaching-valorisation-pdg` (mode coach socratique)
- `st-digital-valorisation` v0.3 — skill `dcf-st-digital` (DCF pré-configuré)
- `st-digital-valorisation` v0.4 — skill `narrative-valeur-st-digital`
- `st-digital-demand-gen` — plugin marketing demand gen (à venir)

## Prérequis

Le plugin `st-digital-valorisation` requiert d'avoir préalablement installé
le plugin core `financial-analysis` du marketplace Anthropic Financial Services :

```bash
/plugin marketplace add anthropics/financial-services-plugins
/plugin install financial-analysis@financial-services-plugins
```
