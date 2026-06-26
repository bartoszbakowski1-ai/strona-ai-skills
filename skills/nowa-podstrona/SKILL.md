---
name: nowa-podstrona
description: Tworzy nową podstronę lub landing na stronie jacekfiedziukiewicz.pl w tym samym design systemie co istniejące podstrony (navy #0b1218 + orange #ff9022, IBM Plex, klasy .jaca-* i Webflow, nawigacja + stopka). Uruchom, gdy użytkownik mówi "nowa podstrona", "stwórz stronę", "dodaj landing", "dodaj stronę w tym samym designie", "/nowa-podstrona". Buduje czystą stronę React (App Router) z gotowym SEO i dopina ją do menu.
---

# Nowa podstrona w designie JACA AI

Tworzysz nową podstronę spójną z resztą strony Jacka. Pracujesz **z roota repo strony**
(`jacekfiedziukiewicz.pl`). Nie budujesz od zera designu — reużywasz istniejący system.

## Krok 0 — zgrunduj się na realnym kodzie (ZAWSZE)

Zanim cokolwiek napiszesz, przeczytaj (nie zgaduj):
1. `PROJEKT.md` z tego repo skilli (design system, ścieżki, źródła danych).
2. `app/globals.css` — tokeny `:root` i definicje klas `.jaca-*` (zweryfikuj, że nadal istnieją).
3. `components/jaca/Nav.tsx` i `components/jaca/Footer.tsx` — jak wyglądają i jaka jest tablica `LINKS`.
4. Jedną istniejącą stronę z `.jaca-*` jako wzór: `app/blog/page.tsx`.

Jeśli któryś plik/klasa nie istnieje — zatrzymaj się i powiedz to, zamiast budować na założeniu.

## Krok 1 — krótki wywiad (max 4 pytania naraz, po polsku)

Ustal:
- **Cel** strony (sprzedaż usługi / landing pod lead / strona informacyjna).
- **Slug** (np. `/audyt-ai`) — krótki, po polsku, z myślnikami.
- **Sekcje** w kolejności (typowo: hero → problem → oferta/korzyści → kroki → FAQ → CTA).
- **CTA**: dokąd prowadzi główny przycisk (`/kontakt`, `/wdrozenie`, Cal.com, `/newsletter`).
- Czy potrzebny **formularz** (jeśli tak — patrz Krok 4).

Jeśli użytkownik podał już brief, nie pytaj o oczywiste — działaj.

## Krok 2 — zbuduj stronę jako czysty React (wzór B)

Utwórz `app/<slug>/page.tsx` jako **komponent React** (NIE WebflowPage — dla nowych stron nie ma
eksportu Webflow). Szkielet:

```tsx
import type { Metadata } from "next";
import { Nav } from "@/components/jaca/Nav";
import { Footer } from "@/components/jaca/Footer";

export const metadata: Metadata = {
  title: "...",                       // unikalny, ~50-60 znaków, z marką
  description: "...",                 // unikalny, ~150 znaków, konkret + korzyść
  alternates: { canonical: "/<slug>" },
};

export default function Page() {
  return (
    <>
      <Nav />
      <main>
        {/* sekcje */}
      </main>
      <Footer />
    </>
  );
}
```

Sekcje buduj z **istniejących klas** (sprawdzonych w globals.css / webflow.css), np.:
- Kontener sekcji: `<section><div class="padding-global"><div class="container-large"><div class="padding-section-large"> ... </div></div></div></section>`.
- Eyebrow: `<div className="text-style-tagline">Krótki nadtytuł</div>`.
- Nagłówek sekcji: `<h2 className="heading-style-h2">...</h2>`; pod-nagłówek `heading-style-h4`.
- Akapit: `<p className="text-size-medium">...</p>`.
- Siatka kart: `<div className="jaca-grid-3">` z `.jaca-offer-card` / `.jaca-step-card` / `.jaca-card`.
- Kafelek ikony: `<div className="jaca-ic">...</div>`.
- FAQ: `.jaca-faq` (natywny `<details><summary>`).
- Box CTA: `.jaca-cta-box` albo `.jaca-consult`.
- Przyciski: `<a className="button w-button">Tekst →</a>`, wariant `button is-secondary w-button`.
- Akcent w nagłówku: `<span className="is-orange">słowo</span>`.

Dokładnie **jeden `<h1>`** na stronie (w hero). Pozostałe nagłówki to `h2`/`h3`.

## Krok 3 — dopnij do nawigacji

- Dodaj `{ href: "/<slug>", label: "Etykieta" }` do `LINKS` w `components/jaca/Nav.tsx`, jeśli ma
  być w menu głównym.
- W razie potrzeby dodaj wpis do `NAV` lub `OFERTA` w `components/jaca/Footer.tsx`.
- Jeśli strona zastępuje stary URL — dodaj redirect 301 w `next.config.ts`.

## Krok 4 — formularz (tylko jeśli potrzebny)

Reużyj istniejącego API. Najprościej: prosty formularz `POST` na `/api/kontakt` (pola `imie`,
`email`, `temat`, `wiadomosc`) lub `/api/newsletter` (pola `imie`, `email`). Nie twórz nowego
endpointu, jeśli istniejący wystarcza. Dodaj honeypot (ukryte pole) i walidację po stronie klienta.
Nie dotykaj env ani kluczy.

## Krok 5 — anti-AI-look + polskie znaki

Przepuść treść i layout przez `references/anti-ai-look.md` z tego repo. Minimum:
- Polskie ogonki **wszędzie** (treść, `title`, `description`, `alt`, etykiety). Zero em-dash (—).
- Realne, konkretne liczby i przykłady zamiast buzzwordów ("AI która sprzedaje" > "synergiczne rozwiązania").
- Oddech między sekcjami (duże odstępy), nie wszystko wyśrodkowane, nie identyczne karty z tym
  samym hoverem w kółko.
- Tylko paleta marki (navy/orange) — żadnych gradientów violet/blue, żadnego dark+neon.

## Krok 6 — build i weryfikacja

```bash
rm -rf .next && npm run build
```

Jeśli build zielony: pokaż użytkownikowi co powstało i zaproponuj `/bezpieczenstwo` oraz
`/seo-audyt` przed publikacją. **Deploy na Vercel tylko za wyraźną zgodą** — nie wypychaj
automatycznie. Jeśli build czerwony — napraw błąd (typy, `'use client'`, importy) i powtórz.

## Czego NIE robić

- Nie używaj WebflowPage dla nowej strony (brak HTML w `app/_webflow/`).
- Nie wymyślaj nowych kolorów/fontów ani własnego "systemu landingowego".
- Nie commituj, nie pushuj, nie deployuj bez zgody użytkownika.
- Nie zmieniaj `.env`, kluczy, `from:` w API ani konfiguracji Sanity/Resend.
