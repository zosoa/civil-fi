# Civil-Fi PAFC® — Site institutionnel

Site officiel de l'**Institut Civil-Fi PAFC®** — premier OS juridique fiduciaire civil français. Single-page statique, autonome, zéro dépendance backend.

**URL de production :** *(à venir, via Vercel)*
**Domaine cible :** `civil-fi.org`

---

## 🎯 Contexte

Civil-Fi est l'infrastructure juridique qui rend opérable la fiducie civile française à grande échelle. Le standard PAFC® est publié par l'Institut, qui s'appuie sur trois organes : l'**Académie** (CCA), le **Comité de Surveillance** (CCS), le **Registre Fiduciaire Central** (RFC).

- Marques INPI déposées le 18.03.2026 — n°5238667 (classes 35, 36, 42, 45)
- Doctrine v4.0 publiée le 18.04.2026 (280 pages, addenda F1-F6 / D1-D6 / E1-E6)
- OJP signée par AARPI Fiducie Legal — Me Pierre-Alain Ravot
- 4 dépôts Soleau · Rescrit DGFiP art. L.64B LPF

---

## 🏗 Stack & architecture

- **HTML statique single-page** (~5500 lignes, ~190 KB)
- **CSS inline** — pas de framework, palette navy/terracotta/peach/cream/burgundy
- **GSAP + ScrollTrigger** via CDN (jsdelivr/cloudflare) pour les animations
- **Fonts** : Poppins (display) + Nunito Sans (body) + Fraunces (serif italique éditorial) via Google Fonts
- **Aucun build step** · aucun npm install · aucun backend (formulaires en mailto fallback)

### Sections du parcours

```
Hero (slide-stack peel)
  ↓
Primer "Civil-Fi · PAFC · Institut" (3 cartes + 4 principes normatifs)
  ↓
État du réseau (compteurs animés 5 / 12 / 80 / 4)
  ↓
L'architecture normative (5 couches du standard sur timeline navy)
  ↓
Mécanisme — Comment ça marche (4 étapes du réel à la communauté)
  ↓
Cas d'usage B2B2C (4 verticales : Charia · UHNW · Énergie · Tokenisation)
  ↓
Pull-quote Article 2011 du Code civil (sur image Palais de Justice)
  ↓
Académie · Comité · Registre · Bibliothèque (5 sections numérotées)
  ↓
Bulletin signup · CTA section · Footer institutionnel
```

---

## 📁 Structure

```
civil-fi/
├── index.html              ← Le site complet
├── vercel.json             ← Config Vercel (headers, cache, clean URLs)
├── .gitignore
├── README.md               ← Ce fichier
├── assets/
│   └── courthouse-avocats.png   ← Image Palais de Justice (pull-quote)
└── logos/                  ← SVG de référence (logos déjà inlinés dans index.html)
    ├── B_filet.svg         ← Logo nav (filet tricolore)
    ├── 01_CCFP.svg         ← Médaillon CCFP
    ├── 02_CCFS.svg         ← Médaillon CCFS Charia
    ├── ACC_cachet.svg      ← Cachet ACC
    └── D_cocarde.svg       ← Variante cérémonielle
```

---

## 🚀 Développement local

```bash
# Servir le site (n'importe quel serveur statique)
npx http-server . -p 8000

# OU via Python
python -m http.server 8000
```

Puis ouvrir `http://localhost:8000`.

---

## 🌐 Déploiement Vercel

Le site est déployé via Vercel. Config dans `vercel.json` :
- Clean URLs (sans `.html`)
- Cache long (1 an) sur `/assets` et `/logos`
- Cache court sur HTML
- Headers de sécurité (HSTS, X-Frame-Options, Referrer-Policy)

```bash
# Première fois — login + lien projet
vercel login
vercel link

# Déployer en production
vercel --prod
```

---

## 📧 Adresses e-mail à configurer

Le formulaire de contact (modal) et tous les CTAs pointent vers ces adresses (à wirer côté MX) :

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

---

## ⚙️ Backend à wirer (post-MVP)

1. **Formulaire de contact** (modal) — actuellement simulé (preventDefault + animation succès). À brancher sur Formspree, Brevo, ou un endpoint custom.
2. **Bulletin signup** — même chose, à brancher sur Mailchimp/Brevo/Buttondown.
3. **Pages légales** — Mentions légales, Confidentialité, Cookies, CGU à créer en HTML séparés et router via `vercel.json`.
4. **Schema.org** — Compléter avec `sameAs` réels (LinkedIn, Twitter de l'Institut) une fois créés.

---

## 🔗 Liens institutionnels

- INPI marque n°5238667 : <https://data.inpi.fr/marques/n%C2%B05238667>
- Article 2011 Code civil sur Légifrance : <https://www.legifrance.gouv.fr/codes/article_lc/LEGIARTI000006444097/>

---

## 📜 Licence & propriété

© Institut Civil-Fi PAFC® · SC CDDEV · RealSmart® · MMXXVI. Tous droits réservés.
Marques **Civil-Fi®**, **PAFC®**, CCFP, CCFS, ACC, RFC sont la propriété de leurs titulaires.

Co-fondateurs : **Clément Éric Dijoux** (SC CDDEV, Vénissieux, France) · **Zo** (RealSmart®).
