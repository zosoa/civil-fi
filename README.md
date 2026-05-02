# Civil-Fi PAFC® — Site institutionnel

Site officiel de l'**Institut Civil-Fi PAFC®** — premier OS juridique fiduciaire civil français. Single-page statique, autonome, zéro dépendance backend.

**URL de production** : https://civil-fi.org · https://civil-fi.org/en/ (English)
**Repo** : https://github.com/zosoa/civil-fi
**Hébergement** : Vercel (CDN edge)
**Domaine** : civil-fi.org (Spaceship registrar, DNS A→Vercel)

---

## 🎯 Contexte

Civil-Fi est l'infrastructure juridique qui rend opérable la fiducie civile française à grande échelle. Le standard PAFC® est publié par l'Institut, qui s'appuie sur trois organes : l'**Académie** (CCA), le **Comité de Surveillance** (CCS), le **Registre Fiduciaire Central** (RFC).

- Marques INPI déposées le 18.03.2026 — n°5238667 (classes 35, 36, 42, 45)
- Doctrine v4.0 publiée le 18.04.2026 (280 pages, addenda F1-F6 / D1-D6 / E1-E6)
- OJP signée par AARPI Fiducie Legal — Me Pierre-Alain Ravot
- 4 dépôts Soleau · Rescrit DGFiP art. L.64B LPF

---

## 📱 Approche mobile-first

**90% du trafic attendu est mobile.** Le site est testé et optimisé en priorité pour :
- iPhone SE (320×568) — le plus petit viewport courant
- iPhone 14/15 (375×812)
- Android moyen (390×844)
- Tablet (768×1024)
- Desktop (1280+)

**Décisions architecturales mobile-first** :
- Le slide-stack peel (sticky 200vh) est **désactivé sous 980px** via `@media (max-width: 980px)` — sur mobile, les sections coulent naturellement, pas de clipping 100vh.
- Le hero affiche systématiquement son illustration SVG (la maison française pencil-sketch animée). L'animation CSS `stroke-dashoffset` tourne en boucle 14s, indépendante du scroll.
- La nav s'auto-réduit sur mobile : logo passe de 76px → 44px, le label CTA passe de "Contacter l'Institut" → "Contact" (via deux `<span>` swappés en CSS display).
- Les boutons hero passent en colonne pleine largeur sous 480px.
- Les grids 2/3/4 colonnes tombent en single-col entre 480 et 768px selon densité.
- Touch targets ≥ 44×44px partout (boutons, fermeture modal).
- Aucun overflow horizontal à aucun breakpoint.

⚠️ **À tester impérativement** sur iPhone réel (Safari) et Android Chrome avant tout passage en prod sur civil-fi.org. Le headless preview n'exécute pas les animations correctement (tabs hidden), donc certains effets (drawing house, counter, modal animation succès) ne s'affichent qu'en navigateur réel actif.

---

## 🏗 Stack technique

- **HTML statique single-page** — `index.html` ~5700 lignes ~205 KB
- **CSS inline** dans `<style>` — pas de framework, palette navy/terracotta/peach/cream/burgundy
- **GSAP 3.12.5 + ScrollTrigger** via CDN cloudflare pour les animations
- **Fonts Google** : Poppins (display) + Nunito Sans (body) + Fraunces (serif italique éditorial)
- **Aucun build step** · aucun npm install · aucun backend (formulaires en mailto fallback)

### Architecture du fichier `index.html`

```
Lignes 1–80         <head> : meta, fonts, GSAP CDN, Schema.org JSON-LD, favicon SVG inline
Lignes 80–3320      <style> : tout le CSS (variables :root, sections, mobile media queries)
Lignes 3320–5500    <body> : 11 sections + nav + footer
Lignes 5500–5640    <script> : GSAP animations, modal, smooth-scroll, counter
Lignes 5640–5700    Modal HTML + script init modal + safety net
```

### Sections du parcours (mobile-first scroll order)

```
1. Nav (sticky)
2. Hero (slide-stack desktop / normal flow mobile)
3. Primer "Civil-Fi · PAFC · Institut" (3 cartes + 4 principes normatifs)
4. État du réseau (compteurs animés 5 / 12 / 80 / 4)
5. L'architecture normative (5 couches sur timeline navy)
6. Mécanisme — Comment ça marche (4 étapes)
7. Cas d'usage B2B2C (4 verticales : Charia · UHNW · Énergie · Tokenisation)
8. Pull-quote Article 2011 (sur image Palais de Justice)
9. Académie (CCA) · Comité (CCS) · Registre (RFC)
10. Bibliothèque (5 catégories, 12 documents)
11. Bulletin signup
12. CTA section (single CTA "Rejoindre l'Institut")
13. Footer institutionnel (5 cols + crédentials + mentions légales)
```

---

## 📁 Structure du livrable

```
civil-fi/
├── index.html              ← Site FR (single-file complet)
├── en/
│   └── index.html          ← Site EN (single-file complet)
├── assets/
│   └── courthouse-avocats.png   ← Image Palais de Justice (pull-quote)
├── logos/                  ← SVGs validés (réservés, déjà inlinés dans HTML)
│   ├── B_filet.svg         ← Logo nav (filet tricolore)
│   ├── 01_CCFP.svg         ← Médaillon CCFP
│   ├── 02_CCFS.svg         ← Médaillon CCFS Charia
│   ├── ACC_cachet.svg      ← Cachet ACC
│   └── D_cocarde.svg       ← Variante cérémonielle
├── vercel.json             ← Config Vercel (headers, cache, clean URLs)
├── robots.txt              ← Crawl rules + IA bots whitelist
├── sitemap.xml             ← FR + EN avec hreflang
├── llms.txt                ← GEO: contenu structuré pour LLMs
├── README.md               ← Ce fichier
└── .gitignore
```

---

## 🚀 Lancer en local

```bash
# Servir le site (n'importe quel serveur statique)
npx http-server . -p 8000
# OU via Python
python3 -m http.server 8000
```

Ouvre `http://localhost:8000`. Pour tester le mobile, **DevTools → Device Mode → iPhone SE (320×568) ou iPhone 14 Pro (393×852)**.

---

## 🌐 Déploiement Vercel

Config dans `vercel.json` :
- Clean URLs (sans `.html`)
- Cache long (1 an immutable) sur `/assets` et `/logos`
- Cache court (must-revalidate) sur HTML
- Headers de sécurité (HSTS, X-Frame-Options, Referrer-Policy, Permissions-Policy)

```bash
# Déployer en production
vercel --prod
```

---

## 🔍 SEO + GEO (Generative Engine Optimization)

### Optimisations en place
- **Title + meta description** uniques par langue
- **Open Graph** (FB, LinkedIn) + **Twitter cards**
- **Schema.org JSON-LD** : Organization, WebSite, EducationalOrganization, Service, FAQPage
- **Canonical** + **hreflang** (fr / en / x-default)
- **Sitemap** XML avec hreflang
- **robots.txt** : whitelist explicite GPTBot, ClaudeBot, PerplexityBot, Google-Extended, CCBot, etc. → autorise les crawlers IA pour le GEO
- **llms.txt** : page structurée Markdown destinée aux LLMs (ChatGPT, Claude, Perplexity, Gemini), résumé doctrinal complet
- **Favicon SVG** inline data: URI (zéro requête)
- **Lang attributes** : `lang="fr"` / `lang="en"`
- **Sémantique HTML5** : `<nav>`, `<section>`, `<article>`, `<footer>`, `<aside>`
- **ARIA labels** sur tous les éléments interactifs
- **Heading hierarchy** : H1 unique par page (hero), H2 par section, H3-H4 imbriqués proprement

### À faire post-déploiement
1. **Google Search Console** : submit sitemap.xml, claim domain
2. **Bing Webmaster Tools** : same
3. **Vérification INPI sameAs** : confirmer que la fiche INPI référence bien civil-fi.org
4. **LinkedIn / X (Twitter) profiles** : créer les comptes officiels et ajouter les URLs au `sameAs` du Schema.org Organization
5. **Google Business Profile** (si applicable) pour Vénissieux
6. **Backlinks institutionnels** : Légifrance, AFNOR, IFRS Foundation, INPI, AARPI Fiducie Legal site
7. **Vitesse** : Lighthouse pass attendu — score 90+ partout en mobile

---

## 📧 Adresses e-mail à configurer (MX Spaceship)

| Adresse | Usage |
|---|---|
| `presse@civil-fi.org` | Demandes presse, journalistes, chercheurs |
| `licences@civil-fi.org` | Candidatures FAC-Mère / cabinets licenciés |
| `academie@civil-fi.org` | Programme certification CCFP / CCFS |
| `comite@civil-fi.org` | Soumission projets à l'ACC |
| `registre@civil-fi.org` | Inscription au RFC |
| `ressources@civil-fi.org` | Demandes de documents bibliothèque |
| `bulletins@civil-fi.org` | Inscription aux bulletins mensuels |
| `legal@civil-fi.org` | Mentions légales, RGPD, cookies |

⚠️ **Sans MX configurés, tous les CTAs mailto ne reçoivent rien.** Priorité critique pour le passage en production.

---

## ⚙️ Backend à wirer (post-MVP)

1. **Formulaire de contact (modal)** — actuellement simulé (preventDefault + animation succès). À brancher sur **Formspree**, **Brevo**, **SendinBlue** ou un endpoint custom. JS dans `index.html` ligne ~5570 :
   ```js
   form.addEventListener('submit', (e) => {
     e.preventDefault();
     // Replace setTimeout simulation with fetch('/api/contact', { method: 'POST', body: ... })
     ...
   });
   ```

2. **Bulletin signup** — même chose, à brancher sur **Mailchimp**, **Buttondown**, **ConvertKit**.

3. **Pages légales** — Mentions légales, Confidentialité, Cookies, CGU à créer en HTML séparés (`/mentions-legales.html` etc.) et router via `vercel.json` quand prêts. Les liens fallback actuels pointent vers `mailto:legal@civil-fi.org`.

4. **Schema.org sameAs** — Compléter avec LinkedIn, Twitter de l'Institut une fois créés.

5. **Analytics** — Plausible (recommandé, RGPD-friendly) ou Vercel Analytics (intégré).

---

## 🔗 Liens institutionnels

- INPI marque n°5238667 : <https://data.inpi.fr/marques/n%C2%B05238667>
- Article 2011 Code civil sur Légifrance : <https://www.legifrance.gouv.fr/codes/article_lc/LEGIARTI000006444097/>
- Loi n°2007-211 du 19 février 2007

---

## 📜 Licence & propriété

© Institut Civil-Fi PAFC® · SC CDDEV · RealSmart® · MMXXVI. Tous droits réservés.
Marques **Civil-Fi®**, **PAFC®**, CCFP, CCFS, ACC, RFC sont la propriété de leurs titulaires.

Co-fondateurs : **Clément Éric Dijoux** (SC CDDEV, Vénissieux, France) · **Zo** (RealSmart®).

---

## 🤝 Contact dev

- Repo GitHub : https://github.com/zosoa/civil-fi
- Issues / questions : ouvrir une issue sur le repo
- Doctrine de référence : `/docs/Doctrine_v4.0_GRAND_OEUVRE.docx` (si fourni séparément)
