---
layout: post
title: CourtiConnect — Plateforme PropTech Québec
image: "/posts/courticonnect.jpg"
tags: [Next.js, Supabase, IA, PropTech, TypeScript]
---

## Plateforme de matching immobilier avec veille marché automatisée

**Statut : Live en production** → [courticonnect.ca](https://courticonnect.ca)

---

### Le problème

Les vendeurs immobiliers à Montréal cherchent un courtier de confiance mais naviguent à l'aveugle. Les courtiers, eux, n'ont pas de système pour identifier les propriétaires motivés avant qu'ils aillent voir la concurrence.

---

### La solution

CourtiConnect automatise le pipeline complet : identification des propriétaires potentiels → veille marché en temps réel → matching courtier × client → nurturing automatisé.

---

### Architecture technique

```
Next.js 14 (App Router) · TypeScript · Supabase · Vercel · Claude API
```

**17 endpoints API en production :**

- `/api/chat` — RAG Chat avec données marché Supabase en temps réel
- `/api/lead` — Capture leads avec source tracking (UTM + referrer)
- `/api/social/generate` — Génère 7 posts sociaux/semaine automatiquement
- `/api/scrape/[quartier]` — 9 endpoints scraping (Firecrawl)
- `/api/aggregate` — Scoring rues chaudes + détection presumed_sold
- `/api/send-weekly-report` — Email veille hebdomadaire automatique

---

### Pipeline veille marché

11 cron jobs Vercel tournent chaque semaine :

1. **Steps 1-9** : Scraping 9 quartiers Montréal (Montréal-Nord, RDP, Saint-Léonard, Hochelaga, Villeray, Parc-Extension, Saint-Laurent, Pointe-aux-Trembles, Anjou)
2. **Step 10** : Agrégation → table `weekly_hot_streets` + détection `presumed_sold`
3. **Step 11** : Email hebdo avec rues score ≥7 + briefs cold call auto-générés

---

### RAG Chat MVP

- Interroge `market_listings` + `weekly_hot_streets` en temps réel
- Extraction d'intention déterministe (quartier, type de bien, intention)
- Réponses via Claude Sonnet avec données réelles injectées
- Masqué sur pages de conversion, visible sur pages informationnelles

---

### SEO & Contenu

- **64 articles** de blog (34 FR + 30 EN) avec Schema.org FAQPage
- **17 200 impressions** Google Search Console (28 jours, mars 2026)
- **34/34 pages indexées** — IndexNow soumis à Bing/Yandex/Naver
- Rich Results validés sur 55 pages
- Position moyenne : 6.6 (progression : 22.9 → 10.1 → 6.6 en 6 semaines)

---

### Résultats

| Métrique | Valeur |
|----------|--------|
| Pages indexées | 34/34 |
| Impressions GSC (28j) | 17 200 (+99%) |
| Avis Google 5★ | 30 |
| Leads capturés | 8/25 objectif mars |
| Articles blog | 64 (FR + EN) |
| Crons actifs | 11 |
| Endpoints API | 17 |

---

### Stack complet

- **Frontend** : Next.js 14, TypeScript, React, Tailwind CSS
- **Backend** : API Routes Next.js, Supabase (PostgreSQL + RLS)
- **IA** : Claude API (Sonnet), RAG architecture
- **Scraping** : Firecrawl
- **Email** : Resend (transactionnel), Brevo (marketing)
- **Analytics** : GA4, Microsoft Clarity, Meta Pixel
- **Infra** : Vercel (hosting + crons), Supabase
- **Paiement** : Stripe
