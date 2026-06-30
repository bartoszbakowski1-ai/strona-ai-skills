---
name: strona-oferty
description: Buduje STRONĘ oferty (podstronę `/oferta`) na stronie zbudowanej w kursie "Stwórz stronę z AI" (Next.js, App Router) w Twoim design systemie. Przeprowadza krótki wywiad o tym, co sprzedajesz, dla kogo, w jakich pakietach i z jakim efektem, a potem składa konwertującą podstronę (problem klienta, pakiety, jak to wygląda, dowód, FAQ, jedno CTA). Uruchom, gdy użytkownik mówi "zrób stronę oferty", "zbuduj podstronę z ofertą", "strona z cennikiem/pakietami", "/strona-oferty". Jeśli istnieje plik OFERTA.md (ze skilla zaprojektuj-oferte) - weź treść stamtąd. NIE zmyśla cen, wyników ani opinii - braki oznacza [do potwierdzenia]. To buduje STRONĘ z gotowej treści; jeśli dopiero układasz samą ofertę, najpierw użyj skilla zaprojektuj-oferte.
---

# Strona oferty Twoich usług

Budujesz **jedną podstronę**, na której Twój klient rozumie, co oferujesz, dla kogo to jest, ile to
kosztuje (albo jak wycenić) i co ma zrobić dalej. To strona oferty dla Twoich usług - pisana Twoimi
słowami, w designie Twojej strony. Pracujesz **z roota repo strony** (tam, gdzie `package.json`).

Złota zasada: **nic nie zmyślasz.** Ceny, efekty, liczby, opinie - tylko to, co poda użytkownik.
Czego nie wiesz, zostaje jako `[do potwierdzenia]`, nie jako wymyślony konkret.

## Krok 0 - zgrunduj się na realnym kodzie (ZAWSZE)

Przeczytaj, zanim cokolwiek napiszesz:
1. `PROJEKT.md` z tego repo skilli (design system, nazwy komponentów nawigacji/stopki, formularz).
2. `app/globals.css` - tokeny `:root` i klasy design systemu (zweryfikuj, że istnieją).
3. Komponenty nawigacji i stopki + jedną istniejącą podstronę jako wzór (z `PROJEKT.md`).
4. **`OFERTA.md`** (jeśli istnieje) - plik ze skilla `zaprojektuj-oferte` z gotową ofertą: klient,
   efekt, pakiety, cena, gwarancja, hooki. Jeśli go masz, to jest Twoje główne źródło treści -
   przepisz ofertę na stronę zamiast pytać o wszystko od nowa. Potwierdź tylko luki `[do potwierdzenia]`.
5. Jeśli robiłeś kurs - `kontekst/profil.md`, `kontekst/persona.md`, `karty/karta-strategiczna.md`.
   Tam jest już sporo o tym, co sprzedajesz i komu. Wykorzystaj to zamiast pytać o to samo.

Jeśli plik/klasa nie istnieje - powiedz to i nie buduj na założeniu.

## Krok 1 - wywiad o ofercie (max 4 pytania naraz, po polsku, prostym językiem)

Pytaj jak człowiek, nie jak formularz. Najpierw to, bez czego nie ruszysz:

1. **Co dokładnie sprzedajesz?** (np. sesja zdjęciowa nieruchomości, pakiet 3 miesiące współpracy).
   Jedna usługa czy kilka pakietów?
2. **Dla kogo to jest?** Kto jest idealnym klientem i z jakim problemem przychodzi.
3. **Jaki efekt dostaje klient?** Co się zmienia po skorzystaniu (konkret, nie hasło).
4. **Cena / model wyceny.** Stała cena, widełki, „od X", czy „wycena po rozmowie". Jeśli nie chcesz
   podawać ceny - ustalamy CTA „zapytaj o wycenę".

W drugiej turze (jeśli potrzeba): jak wygląda proces krok po kroku, co dokładnie wchodzi w pakiet
(deliverables), czas realizacji, czym się wyróżniasz, jakie masz opinie/realizacje, jedno główne CTA
(formularz / kalendarz / mail).

Reguła: **jedno pytanie = jedna rzecz.** Nie wszystko naraz. Jeśli użytkownik czegoś nie wie albo nie
chce podać - zapisz `[do potwierdzenia]` i jedź dalej, nie blokuj się.

## Krok 2 - ułóż ofertę (zanim zaczniesz kodować)

Zbierz odpowiedzi w krótki szkielet i **pokaż go użytkownikowi do akceptacji** (1-2 zdania na sekcję):

- **Hero:** dla kogo + jaki efekt + nazwa oferty. Jeden `<h1>`.
- **Problem / sytuacja klienta** - jego słowami, bez straszenia.
- **Co oferuję / pakiety** - 1-3 pakiety. Każdy: nazwa, dla kogo, co wchodzi, cena lub „wycena", CTA.
- **Jak to wygląda (proces)** - 3-5 kroków od „odzywasz się" do „masz efekt".
- **Dlaczego ja / dowód** - opinie, realizacje, liczby. **Tylko realne.** Brak → pomiń sekcję.
- **FAQ** - 3-6 realnych pytań klientów (czas, zakres, co jeśli..., jak płatność).
- **CTA końcowe** - jedno, to samo działanie co w hero.

Gdy użytkownik zaakceptuje szkielet - budujesz. Jeśli czegoś brak na dowód, **nie wymyślaj** opinii
ani logo klientów.

## Krok 3 - zbuduj jako czysty React, w design systemie

Utwórz `app/oferta/page.tsx` (lub inny uzgodniony slug) jako komponent React. Dopasuj importy
nawigacji/stopki do realnych nazw z `PROJEKT.md`:

```tsx
import type { Metadata } from "next";
import { Nav } from "@/components/Nav";
import { Footer } from "@/components/Footer";

export const metadata: Metadata = {
  title: "...",                       // unikalny, ~50-60 znaków: usługa + dla kogo
  description: "...",                 // ~150 znaków: efekt + konkret
  alternates: { canonical: "/oferta" },
};

export default function Page() {
  return (
    <>
      <Nav />
      <main>
        {/* hero → problem → pakiety → proces → dowód → FAQ → CTA */}
      </main>
      <Footer />
    </>
  );
}
```

> Jeśli nawigacja i stopka są w globalnym `app/layout.tsx`, nie dubluj ich - zrób jak istniejące
> podstrony.

Sekcje buduj z **istniejących klas/komponentów** design systemu (sprawdzonych w `globals.css` i
`PROJEKT.md`): eyebrow, nagłówek sekcji `h2`, akapit, siatka kart na pakiety, kafelek ikony na kroki
procesu, FAQ (`<details><summary>`), box CTA, przyciski. Nie wymyślaj nowych kolorów/fontów ani
własnego „systemu landingowego".

Pakiety: karta = nazwa + krótki opis dla kogo + lista „co wchodzi" + cena/„wycena" + przycisk CTA.
Jeden pakiet możesz wyróżnić (np. „najczęściej wybierany"), ale tylko jeśli to prawda.

## Krok 4 - jedno CTA i formularz (jeśli trzeba)

- **Jedno główne działanie** na całej stronie (np. „Napisz do mnie" albo „Zarezerwuj termin").
  Każdy przycisk prowadzi do tego samego. Nie mieszaj 4 różnych CTA.
- Jeśli CTA to formularz - reużyj istniejącego API z `PROJEKT.md` (zwykle `app/api/contact/route.ts`).
  Nie twórz nowego endpointu, jeśli istniejący wystarcza. Dodaj honeypot i walidację po stronie
  klienta. Nie dotykaj env ani kluczy.
- Jeśli CTA to kalendarz (np. Cal.com) - użyj sposobu osadzenia z `PROJEKT.md`.

## Krok 5 - anti-AI-look + polskie znaki

Przepuść przez `references/anti-ai-look.md`. Minimum:
- Polskie ogonki **wszędzie** (treść, `title`, `description`, `alt`, etykiety). Zero em-dash (—).
- Konkret zamiast buzzwordów. Pakiety mają realnie różną treść, nie trzy identyczne karty.
- Oferta mówi do jednej osoby (Twojego klienta), nie do „wszystkich".
- Tylko paleta marki z design systemu.
- Każdy `[do potwierdzenia]` w treści **zostaje widoczny** dla użytkownika do uzupełnienia - nie
  zastępuj go zmyślonym konkretem.

## Krok 6 - dopnij do nawigacji, build, weryfikacja

- Dodaj `{ href: "/oferta", label: "Oferta" }` do nawigacji (i ew. stopki), jeśli ma być w menu.
- ```bash
  rm -rf .next && npm run build
  ```
- Build zielony: pokaż użytkownikowi co powstało, wypisz wszystkie miejsca `[do potwierdzenia]` do
  uzupełnienia, zaproponuj `/seo-audyt` i `/bezpieczenstwo` przed publikacją. **Deploy tylko za
  wyraźną zgodą.** Build czerwony - napraw (typy, `'use client'`, importy) i powtórz.

## Czego NIE robić

- Nie zmyślaj cen, wyników, liczb, opinii, logo klientów ani zakresu pakietu. Brak = `[do potwierdzenia]`.
- Nie obiecuj gwarancji ani efektów, których użytkownik nie potwierdził.
- Nie rób 5 różnych CTA - jedno działanie.
- Nie wymyślaj nowego designu - reużywaj system strony.
- Nie commituj, nie pushuj, nie deployuj bez zgody. Nie zmieniaj `.env`, kluczy, `from:` w API.
