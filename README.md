# Kousek Řecka na talíři — web

Webová prezentace řecké taverny v Brně (Anenská 4) s admin systémem pro týdenní menu.

## Struktura

```
KRNT/
├── kousek-recka-final.html    ← hlavní web (4 stránky: Úvod / Menu / O nás / Rezervace)
├── admin.html                  ← admin pro Marcelu (týdenní menu + PDF generátor)
├── data/
│   └── weekly-menu.json        ← aktuální data pro denní menu
└── README.md                   ← tento soubor
```

Ostatní `demo-*.html` soubory jsou starší prototypy z fáze návrhu, lze ignorovat / smazat.

## Pro Marcelu — admin

URL: `kousekreckanataliri.cz/admin.html` (nebo `/admin/`)

**Workflow:**

1. Marcela jde na admin stránku
2. Vyplní 5 řádků pro Po–Pá (název jídla, popis, cena)
3. Volitelně přepne polévku v ceně, vegetariánskou alternativu
4. Klikne **„Uložit a publikovat"** → web se aktualizuje (data v localStorage)
5. Klikne **„Stáhnout PDF"** → vygeneruje se hezký jídelní lístek
6. Volitelně **„Export dat (JSON)"** → stáhne JSON pro zálohu

**Současná verze (bez backendu):**
- Data se ukládají do **localStorage** (jen Marcelin prohlížeč)
- Pro plnou produkci nahradit za backend (viz níže)

## Pro produkční deploy (TODO)

### 1. Hosting
- **Cloudflare Pages** — doporučeno (zdarma + Cloudflare Workers pro backend)
- Alternativa: Netlify, GitHub Pages

### 2. Backend pro admin
Nutné pro to, aby Marceliny změny v admin formuláři viděli i ostatní návštěvníci webu (ne jen Marcela). Plán:

- **Cloudflare Worker** (serverless function) — pro upload + autentizaci
- **Cloudflare R2** — storage pro JSON + PDF (zdarma do 10 GB)
- **Magic link auth** přes Resend.com (zdarma do 100 mailů/měs)
- Marcela klikne odkaz v mailu → přihlášena → upload funguje

### 3. Domain
- Marcelina doména `kousekreckanataliri.cz` je u **Forpsi**
- DNS A-record přesměrovat z DISH na Cloudflare Pages
- Důležité: získat přístupy do Forpsi admin panelu od Marcely

### 4. Email
- Možnost A: **Cloudflare Email Routing** (zdarma) — `info@kousekreckanataliri.cz` přeposílat na seznam
- Možnost B: **Forpsi Mail S** (~300 Kč/rok) — plná schránka s vlastní doménou

### 5. Form backend
- Aktuálně `Formspree` placeholder (`YOUR_FORM_ID`)
- Doporučená alternativa: **FormSubmit.co** (zdarma navěky, bez limitu)
- Setup: `<form action="https://formsubmit.co/info@kousekreckanataliri.cz">` + první aktivace mailem

## Funkční přehled webu

### Hlavní web (kousek-recka-final.html)
- ✅ 4 stránky (Úvod / Menu / O nás / Rezervace) s JS routerem
- ✅ Live indikátor otevřeno/zavřeno (čte aktuální čas)
- ✅ Týdenní rozvrh otevírací doby (Po-Pá 10:30-21:00, So 11:00-21:00, Ne 11:00-17:00)
- ✅ Denní menu na homepage — auto-update z `data/weekly-menu.json` nebo localStorage
- ✅ Filtry v menu (Vše / Předkrmy / Hlavní / Saláty / Pro skupiny / Přílohy / Vege / Pro děti)
- ✅ Polaroid galerie s rotací
- ✅ Kontaktní formulář (mailto fallback, ready pro Formspree/FormSubmit)
- ✅ Mobilní drawer menu
- ✅ Sticky CTA na mobilu
- ✅ Google Maps embed
- ✅ Schema.org structured data (Restaurant) — pro Google rich results
- ✅ Print CSS pro tisk menu
- ✅ Favicon set (SVG + Apple Touch + Mask)
- ✅ Skip link pro klávesnici (a11y)
- ✅ ARIA labels na drawer + sociálních ikonách

### Admin (admin.html)
- ✅ Formulář pro 5 dnů týdenního menu
- ✅ Vegetariánská značka per den
- ✅ Polévka v ceně toggle
- ✅ Vegetariánská alternativa textbox
- ✅ Týdenní label
- ✅ PDF generátor (html2canvas + jsPDF) — design webu
- ✅ Export JSON
- ✅ LocalStorage uložení
- ✅ Toast notifikace
- ✅ Reset (znovu načíst z JSON souboru)

## Jak otevřít lokálně

Web je čistý statický HTML, ale fetch JSON z `data/` vyžaduje HTTP server (ne `file://` protokol).

```bash
# Python 3
cd KRNT
python -m http.server 8000

# Node.js
npx serve KRNT -p 8000
```

Pak otevřít:
- Hlavní web: http://localhost:8000/kousek-recka-final.html
- Admin: http://localhost:8000/admin.html

## Vývojářské poznámky

- Tailwind CSS přes CDN (postupně migrovat na build pro produkci)
- Žádný build step zatím
- Vanilla JS, žádný framework
- Fonts: Noto Serif + Manrope + JetBrains Mono (vše Google Fonts)
- Všechny obrázky jsou stock z původního Stitch konceptu (`lh3.googleusercontent.com/aida-public/`)
- Po nasazení nahradit reálnými fotkami z taverny

## Co řeší co

| Soubor | Účel |
|---|---|
| `kousek-recka-final.html` | Veřejný web pro klienty |
| `admin.html` | Admin pro Marcelu, weekly menu management |
| `data/weekly-menu.json` | Aktuální týdenní menu (zdrojová data) |
| `kousek-recka-warm.html` | Alternativní design návrh (cookbook style, terracotta) |
| `demo-*.html` | Starší prototypy z fáze návrhu |
| `index.html`, `index - kopie.html` | Původní verze poslaná Marcele 4/2025 |

## Kontakt

Patrik Simeon Obadal
