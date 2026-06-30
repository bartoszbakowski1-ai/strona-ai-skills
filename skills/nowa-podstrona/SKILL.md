---
name: nowa-podstrona
description: Tworzy nową podstronę lub landing na stronie zbudowanej w kursie "Stwórz stronę z AI" (Next.js, App Router) w tym samym design systemie co istniejące podstrony - paleta, fonty, klasy/komponenty z globals.css, nawigacja i stopka. Uruchom, gdy użytkownik mówi "nowa podstrona", "stwórz stronę", "dodaj landing", "dodaj stronę w tym samym designie", "/nowa-podstrona". Buduje czystą stronę React z gotowym SEO i dopina ją do menu. Paletę, fonty i nazwy klas bierze z PROJEKT.md i realnego kodu, nie zgaduje.
---

# Nowa podstrona w designie Twojej strony

Tworzysz nową podstronę spójną z resztą strony. Pracujesz **z roota repo strony** (tam, gdzie
`package.json`). Nie budujesz designu od zera - reużywasz istniejący system.

## Krok 0 - zgrunduj się na realnym kodzie (ZAWSZE)

Zanim cokolwiek napiszesz, przeczytaj (nie zgaduj):
1. `PROJEKT.md` z tego repo skilli (design system, ścieżki, źródła danych, nazwy komponentów).
2. `app/globals.css` - tokeny `:root` i definicje klas design systemu (zweryfikuj, że nadal istnieją).
3. Komponenty nawigacji i stopki (z `PROJEKT.md`, np. `components/Nav.tsx` i `components/Footer.tsx`) -
   jak wyglądają i jaka jest tablica linków.
4. Jedną istniejącą podstronę jako wzór (z `PROJEKT.md`, np. `app/o-mnie/page.tsx`).

Jeśli któryś plik/klasa nie istnieje - zatrzymaj się i powiedz to, zamiast budować na założeniu.

## Krok 1 - krótki wywiad (max 4 pytania naraz, po polsku)

Ustal:
- **Cel** strony (sprzedaż usługi / landing pod lead / strona informacyjna).
- **Slug** (np. `/audyt`) - krótki, po polsku, z myślnikami.
- **Sekcje** w kolejności (typowo: hero → problem → oferta/korzyści → kroki → FAQ → CTA).
- **CTA**: dokąd prowadzi główny przycisk (`/kontakt`, kalendarz, `/oferta`).
- Czy potrzebny **formularz** (jeśli tak - patrz Krok 4).

Jeśli użytkownik podał już brief, nie pytaj o oczywiste - działaj.

## Krok 2 - zbuduj stronę jako czysty React

Utwórz `app/<slug>/page.tsx` jako **komponent React** w App Routerze. Szkielet (dopasuj importy
nawigacji/stopki do realnych nazw z `PROJEKT.md`):

```tsx
import type { Metadata } from "next";
import { Nav } from "@/components/Nav";
import { Footer } from "@/components/Footer";

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

> Jeśli w Twoim projekcie nawigacja i stopka są w globalnym `app/layout.tsx`, nie dubluj ich tu -
> sprawdź `layout.tsx` i zrób tak jak istniejące podstrony.

Sekcje buduj z **istniejących klas/komponentów** (sprawdzonych w `globals.css` i `PROJEKT.md`):
eyebrow (nadtytuł), nagłówek sekcji (`h2`), akapit, siatka kart, kafelek ikony, FAQ
(`<details><summary>`), box CTA, przyciski, akcent w nagłówku. Nazwy klas bierz z realnego kodu, nie
wymyślaj nowych.

Dokładnie **jeden `<h1>`** na stronie (w hero). Pozostałe nagłówki to `h2`/`h3`.

## Krok 3 - dopnij do nawigacji

- Dodaj `{ href: "/<slug>", label: "Etykieta" }` do tablicy linków w komponencie nawigacji, jeśli ma
  być w menu głównym.
- W razie potrzeby dodaj wpis do stopki.
- Jeśli strona zastępuje stary URL - dodaj redirect 301 w `next.config.ts`.

## Krok 4 - formularz (tylko jeśli potrzebny)

Reużyj istniejącego API (z `PROJEKT.md`, zwykle `app/api/contact/route.ts`). Nie twórz nowego
endpointu, jeśli istniejący wystarcza. Dodaj honeypot (ukryte pole) i walidację po stronie klienta.
Nie dotykaj env ani kluczy.

## Krok 5 - anti-AI-look + polskie znaki

Przepuść treść i layout przez `references/anti-ai-look.md` z tego repo. Minimum:
- Polskie ogonki **wszędzie** (treść, `title`, `description`, `alt`, etykiety). Zero em-dash (—).
- Realne, konkretne liczby i przykłady zamiast buzzwordów.
- Oddech między sekcjami (duże odstępy), nie wszystko wyśrodkowane, nie identyczne karty z tym
  samym hoverem w kółko.
- Tylko paleta marki z design systemu - żadnych obcych gradientów violet/blue ani dark+neon.

## Krok 6 - build i weryfikacja

```bash
rm -rf .next && npm run build
```

Jeśli build zielony: pokaż użytkownikowi co powstało i zaproponuj `/bezpieczenstwo` oraz `/seo-audyt`
przed publikacją. **Deploy na Vercel tylko za wyraźną zgodą** - nie wypychaj automatycznie. Jeśli
build czerwony - napraw błąd (typy, `'use client'`, importy) i powtórz.

## Czego NIE robić

- Nie wymyślaj nowych kolorów/fontów ani własnego "systemu landingowego" - reużywaj design system.
- Nie commituj, nie pushuj, nie deployuj bez zgody użytkownika.
- Nie zmieniaj `.env`, kluczy, `from:` w API ani konfiguracji CMS/Resend.
