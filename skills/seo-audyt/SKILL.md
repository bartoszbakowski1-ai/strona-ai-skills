---
name: seo-audyt
description: Robi audyt SEO strony zbudowanej w kursie "Stwórz stronę z AI" (Next.js, App Router) przed publikacją i opcjonalnie na żywej domenie. Sprawdza metadane per podstrona, kanoniczne, OpenGraph, jeden H1, alty, sitemap/robots, JSON-LD, wagę obrazów i linkowanie wewnętrzne. Uruchom, gdy użytkownik mówi "audyt seo", "sprawdź seo", "czy strona jest pod seo", "/seo-audyt". Zwraca raport PASS/FAIL/TODO, nie zmyśla wyników.
---

# Audyt SEO strony

Lekki, praktyczny audyt techniczno-onpage. Pracujesz **z roota repo strony**. Raportujesz fakty z
realnego kodu / wyrenderowanej strony - **nigdy nie zmyślasz** wyników, pozycji ani liczb.

## Krok 0 - kontekst

Przeczytaj `PROJEKT.md` z tego repo skilli (metadata bazowe w `app/layout.tsx`, lista routes,
źródła treści). Potwierdź, że to projekt Next.js (`package.json`, folder `app/`).

## Krok 1 - metadane per podstrona

Przejdź po wszystkich `app/**/page.tsx` (`find app -name page.tsx`). Dla każdej:
- Czy jest `export const metadata` (lub `generateMetadata` dla `[slug]`)? Strony dynamiczne
  (np. `blog/[slug]`) powinny mieć `generateMetadata` z tytułem z treści.
- **Unikalny** `title` (~50-60 znaków) i `description` (~150 znaków) - nie placeholder typu „Moja
  strona", nie duplikat między podstronami.
- `alternates.canonical` ustawione (lub świadomie dziedziczone z `metadataBase`).
- `openGraph` z `images` (bazowy obraz OG; mocne podstrony mogą mieć własny OG).
- W `app/layout.tsx`: `metadataBase`, `lang="pl"`, bazowy OG `locale: "pl_PL"`.

## Krok 2 - struktura treści

- Dokładnie **jeden `<h1>`** na podstronie.
- Hierarchia nagłówków bez przeskoków (h1 → h2 → h3).
- `alt` na istotnych obrazach (dekoracyjne mogą mieć `alt=""`).

## Krok 3 - pliki techniczne

- `app/sitemap.ts` - jeśli brak, **zaproponuj** (natywny generator Next, wylistuj statyczne routes
  + dynamiczne slugi z funkcji pobierających z CMS, jeśli jest).
- `app/robots.ts` - jeśli brak, zaproponuj (allow + wskazanie sitemap).
- Sprawdź `next.config.ts` pod kątem redirectów 301 (czy stare URL-e nie zwracają 404).

## Krok 4 - dane strukturalne (JSON-LD)

Zaproponuj `Person` / `ProfessionalService` / `LocalBusiness` na stronie głównej i `Service` na
stronach ofertowych (np. `/oferta`). `Article` dla wpisów blogowych. To propozycja do wdrożenia, nie
twierdzenie, że już jest.

## Krok 5 - wydajność i linkowanie

- Ciężkie obrazy: `find public -size +600k` - wskaż kandydatów do kompresji / konwersji na AVIF/WebP.
- Nowy kod powinien używać `next/image` zamiast surowego `<img>`.
- Linkowanie wewnętrzne: czy nowe/ważne podstrony są podlinkowane z nawigacji, stopki, CTA.

## Krok 6 - audyt live (opcjonalnie, gdy podana domena)

Jeśli strona jest publiczna i użytkownik chce sprawdzić wersję live: pobierz **wyrenderowany** HTML
(Firecrawl / WebFetch) i zweryfikuj realny `<title>`, meta description, `<h1>`, tagi OG. Przypomnij
o zgłoszeniu sitemapy w Google Search Console i sprawdzeniu indeksacji.

## Krok 7 - raport

Zwięzła tabela: pozycja → **PASS / FAIL / TODO** → krótka rekomendacja. Na końcu max 3 priorytety
(co najbardziej ruszy SEO). Zero zmyślonych metryk - jeśli czegoś nie da się sprawdzić bez dostępu
do GSC/analityki, napisz to wprost.

## Guardrail

Audyt = czytanie i raport. Zmiany w kodzie (dodanie sitemap/robots/JSON-LD) proponuj i wprowadzaj
**tylko za zgodą** użytkownika. Nie commituj ani nie deployuj bez zgody.
