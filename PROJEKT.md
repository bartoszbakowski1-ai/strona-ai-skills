# PROJEKT.md - źródło prawdy o Twojej stronie (szablon)

Ten plik mówi skillom, **skąd brać dane** o Twojej stronie. Uzupełnij go raz pod siebie (miejsca
oznaczone `<...>` i „TODO"). Każdy skill czyta go na starcie, a potem **weryfikuje fakty na żywym
kodzie** (bo opis może się zdezaktualizować). Wszystkie ścieżki są **względne od roota repo strony** -
uruchamiaj Claude Code z głównego folderu projektu.

> Jak wypełnić: otwórz stronę w Claude Code i poproś „przeczytaj mój kod i uzupełnij PROJEKT.md" -
> albo wpisz wartości ręcznie. Im dokładniej, tym lepiej działają skille.

## Stack

- **Next.js** (App Router), React, TypeScript. Stylowanie: `<Tailwind / CSS variables w globals.css - wpisz co masz>`.
- Skrypty (`package.json`): `npm run dev`, `npm run build`, `npm run start`, `npm run lint`.
- Alias importów: `@/*` → root projektu (np. `@/components/Nav`, `@/lib/...`).
- Jeśli używasz nowej wersji Next (16+), pamiętaj o breaking changes - przy niepewności co do API
  sprawdź `node_modules/next/dist/` albo dokumentację, nie zgaduj.

## Design system - paleta i fonty

Tokeny marki trzymaj w jednym miejscu (`app/globals.css`, zwykle w `:root`). Wpisz swoje wartości:

```css
--background: <#kolor tła>;
--foreground: <#kolor tekstu>;
--accent:     <#kolor akcentu - jeden główny>;
/* ... pozostałe tokeny: tło kart, obrysy, drugi akcent itd. */
```

- **Fonty:** `<np. nagłówki: ..., body: ...>` - ładowane przez `next/font` w `app/layout.tsx`.
- **Stałe marki (nie zmieniaj bez wyraźnej zgody):** `<paleta + fonty>`. To one decydują, że strona
  wygląda spójnie. Źródłem prawdy designu jest `karty/karta-wizualna.md` (jeśli robiłeś kurs) oraz
  realne `app/globals.css`.

## Klasy / komponenty wielorazowe

Wypisz, czym budujesz sekcje (skille mają tego używać zamiast wymyślać nowy „system landingowy"):

- Kontener sekcji: `<np. własny <Section> albo klasy układu>`.
- Siatki kart: `<np. .grid-2 / .grid-3 albo Tailwind grid>`.
- Karty: `<np. .card / komponent <Card>>`.
- FAQ / akordeon: `<np. natywne <details><summary> albo komponent>`.
- Przyciski / CTA: `<np. komponent <Button> albo klasa .button>`.
- TODO: uzupełnij realnymi nazwami klas/komponentów z Twojego `globals.css` i `components/`.

## Wzorzec nowej podstrony

Nowe podstrony budujemy jako **czysty komponent React** w App Routerze (`app/<slug>/page.tsx`),
reużywając istniejący design system. Wzór do podejrzenia: `<wskaż istniejącą podstronę, np. app/o-mnie/page.tsx>`.

> Ważne: jeśli Twoja nawigacja i stopka to komponenty React renderowane per strona (a nie globalny
> layout), to **każda nowa podstrona musi sama wyrenderować nawigację i stopkę** odpowiednimi
> komponentami. Sprawdź to w `app/layout.tsx`.

## Nawigacja i stopka (komponenty)

- Nawigacja: `<np. components/Nav.tsx>` - sticky header, logo, lista linków, CTA.
  **Aby dodać podstronę do menu:** dopisz `{ href: "/slug", label: "..." }` do tablicy linków.
- Stopka: `<np. components/Footer.tsx>` - linki nawigacyjne, oferta, social, linki prawne.

## Źródła treści i danych ("skąd brać dane")

- **CMS (jeśli jest, np. Sanity)** - `<plik z konfiguracją, np. lib/sanity.ts; projectId/dataset; typy dokumentów; funkcje pobierające>`.
  Jeśli nie masz CMS, treść jest w kodzie - napisz to wprost.
- **Formularze (Resend)** - `app/api/contact/route.ts` (pola: `<imie, email, wiadomosc, ...>`).
  Env: `RESEND_API_KEY` (server-only), `KONTAKT_EMAIL` (odbiorca). Jeśli masz więcej endpointów
  (np. newsletter) - wypisz je tu.
- **Rezerwacja / kalendarz (jeśli jest)** - `<np. Cal.com embed, gdzie osadzony>`.
- **Obrazy** - `public/images/`. OG: `<ścieżka do obrazu OG>`. Logo: `<ścieżka>`.

## Metadata / SEO bazowe

- `app/layout.tsx`: `metadataBase: new URL("<https://twojadomena.pl>")`, `lang="pl"`, bazowy
  `openGraph` (`locale: "pl_PL"`, `images: [<OG>]`), Twitter card.
- Każda nowa podstrona dodaje WŁASNE `export const metadata` (unikalny `title`+`description`,
  `alternates.canonical`, własny obraz OG jeśli jest).

## Redirecty

`next.config.ts` - lista 301 (jeśli masz). Zmieniając slug istniejącej strony, dodaj tu redirect 301
ze starego adresu, żeby nie robić 404.

## Routes (stan na dziś)

`<wypisz swoje adresy, np. / /o-mnie /oferta /blog /blog/[slug] /kontakt ...>`

---

## ENV - zmienne środowiskowe

W `.env.local` (NIGDY w repo):

- `RESEND_API_KEY` - klucz Resend (zweryfikowana domena nadawcza).
- `KONTAKT_EMAIL` - mail, na który mają przychodzić zgłoszenia z formularza.
- `from:` w `app/api/contact/route.ts` - adres z Twojej zweryfikowanej domeny w Resend.
- Jeśli masz CMS / inne integracje - dopisz ich zmienne i zaznacz, które są **server-only**, a które
  publiczne (`NEXT_PUBLIC_`).
