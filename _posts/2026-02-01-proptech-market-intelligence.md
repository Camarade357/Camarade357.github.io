---
layout: post
title: PropTech Market Intelligence — Veille immobilière automatisée Montréal
image: "/posts/courticonnect_logo_gbp.png"
tags: [Python, Supabase, Cron, Scraping, PropTech]
---

## Système de veille marché immobilier automatisé — 9 quartiers Montréal

**Extrait de CourtiConnect** · Système en production depuis janvier 2026

---

### Le problème

Un courtier immobilier passe des heures chaque semaine à surveiller manuellement les nouvelles inscriptions sur Centris et DuProprio pour identifier les propriétaires motivés. Ce travail est répétitif, lent, et difficile à prioriser.

---

### La solution

Un pipeline automatisé qui scrape 9 quartiers de Montréal chaque semaine, score les rues selon l'activité du marché, détecte les biens probablement vendus, et génère des briefs d'appel prêts à l'emploi.

---

### Architecture du pipeline

```
Vercel Cron Jobs → Firecrawl → Supabase → Email automatique
```

**11 étapes automatiques chaque semaine :**

```
Steps 1-9  : Scraping parallèle des 9 quartiers
Step 10    : Agrégation + scoring rues + détection presumed_sold
Step 11    : Génération email veille + briefs cold call
```

---

### Algorithme de scoring

Chaque rue reçoit un score basé sur :
- Nombre d'inscriptions actives
- Vélocité (nouvelles inscriptions vs semaine précédente)
- Prix médian vs médiane du quartier
- Jours sur le marché
- Détection `presumed_sold` (inscriptions disparues)

**Seuil d'action** : Score ≥ 7 → brief cold call auto-généré avec adresses ciblées.

---

### Données stockées (Supabase)

```sql
market_listings       -- Tous les listings scrapés avec historique
weekly_hot_streets    -- Agrégation hebdo par rue (score, quartier, stats)
cold_call_results     -- Résultats appels (224 historiques importés)
```

---

### Output automatique

Chaque semaine, le CFO reçoit un email contenant :
- Top 10 rues par score
- Nouvelles inscriptions par quartier
- Biens présumés vendus (opportunités de prospection)
- Briefs d'appel formatés par adresse

---

### Quartiers couverts

Montréal-Nord · Rivière-des-Prairies · Saint-Léonard · Hochelaga-Maisonneuve · Villeray · Parc-Extension · Saint-Laurent · Pointe-aux-Trembles · Anjou

---

### Résultats

| Métrique | Valeur |
|----------|--------|
| Quartiers couverts | 9 |
| Cron jobs actifs | 11 |
| Fréquence | Hebdomadaire |
| Listings en base | 500+ |
| Briefs auto-générés | Rues score ≥7 |

---

### Stack

- **Scraping** : Firecrawl API
- **Database** : Supabase PostgreSQL
- **Scheduling** : Vercel Cron Jobs
- **Email** : Resend
- **Runtime** : Next.js API Routes (TypeScript)
