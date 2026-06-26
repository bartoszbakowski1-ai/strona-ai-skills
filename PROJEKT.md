# PROJEKT.md — strona jacekfiedziukiewicz.pl (źródło prawdy dla skilli)

Ten plik mówi skillom, **skąd brać dane** o stronie. Każdy skill czyta go na starcie, a potem
**weryfikuje fakty na żywym kodzie** (bo opis może się zdezaktualizować). Wszystkie ścieżki są
**względne od roota repo strony** — uruchamiaj Claude Code z głównego folderu projektu.

## Stack

- **Next.js 16.2.9**, App Router. React 19.2.4. TypeScript. Tailwind v4 (`@tailwindcss/postcss`).
- Ważne: `AGENTS.md` w repo ostrzega, że Next 16 ma breaking changes vs starsze wersje. Przy
  niepewności co do API Next 16 sprawdź `node_modules/next/dist/` albo dokumentację, nie zgaduj.
- Skrypty (`package.json`): `npm run dev`, `npm run build`, `npm run start`, `npm run lint`.
- Alias importów: `@/*` → root projektu (np. `@/components/jaca/Nav`, `@/lib/sanity`).

## Design system — paleta i fonty

Tokeny w `app/globals.css` (`:root`):

```css
--background:       #0b1218;              /* navy, tło strony */
--foreground:       #ffffff;              /* tekst */
--jaca-orange:      #ff9022;              /* JEDYNY akcent: koralowo-pomarańczowy */
--jaca-card-bg:     #0e1620;              /* tło kart */
--jaca-card-border: rgba(255,255,255,.08);/* obrys kart */
```

Fonty ładowane w `app/layout.tsx` (Google Fonts `<link>`): **IBM Plex Sans** (300-700, domyślny
body + nagłówki), **IBM Plex Mono** (400-600, akcenty/kod), **Inter** (400-700, zapas).
Nagłówki: waga **300** (lekka), rozmiary przez `clamp()`.

Stałe marki Jacka (nie zmieniaj bez wyraźnej zgody): navy `#0b1218` + orange `#ff9022`, IBM Plex.

## Trzy warstwy CSS dostępne na KAŻDEJ stronie

`app/layout.tsx` importuje globalnie `./webflow.css` **i** `./globals.css`, plus Tailwind v4. Na
dowolnej stronie masz więc do dyspozycji:

1. **Klasy Webflow** (layout/typografia portu): `.padding-global`, `.container-large`,
   `.padding-section-large`, `.heading-style-h2`, `.heading-style-h4`, `.text-size-medium`,
   `.text-size-regular`, `.text-style-tagline`, `.button`, `.button.is-secondary`, `.button-group`,
   `.margin-bottom`, `.margin-small`, `.max-width-medium`, `.is-orange`.
2. **Klasy `.jaca-*`** (komponenty AI design systemu, w `globals.css`):
   `.jaca`, `.jaca-grid-2` / `.jaca-grid-3` / `.jaca-grid-4`, `.jaca-card`, `.jaca-offer-card`,
   `.jaca-step-card`, `.jaca-blog-card`, `.jaca-case-card`, `.jaca-ic` (kafelek ikony),
   `.jaca-pills`, `.jaca-faq` (akordeon details/summary), `.jaca-consult`, `.jaca-cta-box`.
3. **Tailwind v4** — używany w komponentach React (`components/jaca/Nav.tsx`, `Footer.tsx`).
   Kolory podawane literalnie: `bg-[#0b1218]`, `text-[#ff9022]`.

## Dwa wzorce strony

**(A) Port z Webflow** — istniejące podstrony (o-mnie, kontakt, wdrozenie itd.). Statyczne HTML
leży w `app/_webflow/<nazwa>.html`, renderowane przez `components/WebflowPage.tsx`:

```tsx
// app/o-mnie/page.tsx
import { WebflowPage } from "@/components/WebflowPage";
export const dynamic = "force-static";
export default function Page() {
  return <WebflowPage file="o-mnie.html" />;
}
```

**(B) Czysty React z `.jaca-*` / Tailwind** — **to wzór dla NOWYCH podstron** (Jacek nie ma
eksportów Webflow dla nowych stron). Wzór hybrydowy listy: `app/blog/page.tsx`.

> Uwaga nawigacyjna (ważne): globalny `<SiteNav/>` w `app/layout.tsx` ożywia navbar pochodzący z
> markupu Webflow — na czystej stronie React tego markupu **nie ma**. Dlatego nowa podstrona React
> MUSI sama wyrenderować nawigację i stopkę komponentami `components/jaca/Nav.tsx` + `Footer.tsx`.

## Nawigacja i stopka (komponenty React)

- `components/jaca/Nav.tsx` — sticky header, logo `JACA.AI`, lista `LINKS`, CTA „Odbierz Mapę AI".
  **Aby dodać podstronę do menu:** dopisz `{ href: "/slug", label: "..." }` do tablicy `LINKS`.
- `components/jaca/Footer.tsx` — tablice `NAV`, `OFERTA`, `SOCIAL` + linki prawne. Dodaj wpis do
  `NAV` lub `OFERTA` jeśli podstrona ma być w stopce.

## Źródła treści i danych ("skąd brać dane")

- **Sanity (blog + case studies)** — `lib/sanity.ts`. `projectId: "oje1v5sp"`, `dataset:
  "production"`, CDN, **bez tokenu** (projectId/dataset są publiczne, nie sekrety). Studio:
  `jaca-ai.sanity.studio` (Jacek edytuje treść sam). Typy dokumentów: `post`, `caseStudy`.
  Funkcje: `getBlogCards()`, `getBlogSlugs()`, `getBlogPost(slug)`, `getCaseCards()`,
  `getCaseSlugs()`, `getCaseStudy(slug)`, `bodyToHtml(body)`, `fixOrphans(html)`.
- **Formularze (Resend)** — `app/api/kontakt/route.ts` (pola: `imie`, `email`, `sociale`, `temat`,
  `wiadomosc`) i `app/api/newsletter/route.ts` (pola: `imie`, `email`). Env: `RESEND_API_KEY`
  (server-only), `KONTAKT_EMAIL` (odbiorca). Formularze w HTML Webflow są przechwytywane przez
  `components/WebflowForms.tsx` (mapa `wf-form-*` → endpoint).
- **Rezerwacja** — Cal.com `jacekfiedziukiewicz/30min`, embed `components/CalInline.tsx` (na
  `/wdrozenie`).
- **Obrazy** — `public/images/`. OG: `/images/og-image.avif`. Logo: `/images/...Logo.svg`.

## Metadata / SEO bazowe

- `app/layout.tsx`: `metadataBase: new URL("https://jacekfiedziukiewicz.pl")`, `lang="pl"`,
  bazowy `openGraph` (`locale: "pl_PL"`, `images: ["/images/og-image.avif"]`), Twitter card.
- Każda nowa podstrona dodaje WŁASNE `export const metadata` (unikalny `title`+`description`,
  `alternates.canonical`, własny obraz OG jeśli jest).

## Redirecty

`next.config.ts` — 301: `/mentoring → /wdrozenie`, `/imperium-tresci → /system-tresci-ai`.
Zmieniając slug istniejącej strony, dodaj tu redirect 301 ze starego adresu.

## Routes (stan na przejęcie)

`/` `/o-mnie` `/wdrozenie` `/blog` `/blog/[slug]` `/case-study` `/case-study/[slug]` `/kontakt`
`/newsletter` `/linki` `/rezerwacja` `/system-tresci-ai` `/polityka-prywatnosci` `/regulamin`.

---

## HANDOFF — env do podmiany na własne Jacka

Po przeniesieniu repo na konto Jacka i nowy Vercel, podmień (to konfiguracja, nie kod):

- `RESEND_API_KEY` — własny klucz Resend Jacka (zweryfikowana jego domena nadawcza).
- `KONTAKT_EMAIL` — mail Jacka (dziś fallback to `hey@bartbakowski.com`).
- `from:` w `app/api/kontakt/route.ts` i `app/api/newsletter/route.ts` — dziś
  `noreply@bartbakowski.com`. Zmień na adres z domeny Jacka, gdy zweryfikuje ją w Resend.
- **Sanity** — własność projektu `oje1v5sp` / dostęp do `jaca-ai.sanity.studio` przeniesione na
  konto Jacka.
- **Vercel** — projekt + domena `jacekfiedziukiewicz.pl` na koncie Jacka; env jak wyżej w Project
  Settings → Environment Variables (potem redeploy).
