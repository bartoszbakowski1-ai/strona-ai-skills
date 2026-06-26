---
name: bezpieczenstwo
description: Lekki audyt bezpieczeństwa strony jacekfiedziukiewicz.pl (Next.js) przed publikacją. Sprawdza wycieki sekretów i kluczy, walidację formularzy/API, ryzykowne wzorce frontendu, nagłówki bezpieczeństwa i podatności zależności. Uruchom, gdy użytkownik mówi "czy strona jest bezpieczna", "czy wyciekły klucze", "audyt bezpieczeństwa", "/bezpieczenstwo". Zwraca werdykt zielony/żółty/czerwony, nic nie usuwa bez zgody.
---

# Audyt bezpieczeństwa — strona JACA AI

Lekki audyt przed publikacją (~15 min), nie pentest. Pracujesz **z roota repo strony**. Raportujesz
i proponujesz — **nie wykonujesz akcji nieodwracalnych** bez wyraźnej zgody.

## Krok 0 — kontekst

Przeczytaj `PROJEKT.md` (gdzie są API, jakie env, gdzie świadomie używany `dangerouslySetInnerHTML`).
Potwierdź `package.json` + `app/` (Next.js) i `git status`.

## Krok 1 — sekrety i .env (NAJWAŻNIEJSZE)

- `git ls-files | grep -i "\.env"` — żaden `.env*` (poza `.env.example`) NIE może być w repo.
- Sprawdź, że `.gitignore` zawiera `.env*`.
- `find public -type f` — żadnych kluczy/eksportów w `public/`.
- Ripgrep na zahardkodowane klucze w kodzie:
  `rg -n "re_[A-Za-z0-9]{10,}|sk_live|sk_test|AIza[0-9A-Za-z_-]{20,}|-----BEGIN" --glob '!node_modules'`
- `RESEND_API_KEY` musi być **server-only** (używany w `app/api/*/route.ts`). Upewnij się, że
  ŻADEN sekret nie jest pod prefiksem `NEXT_PUBLIC_` (to trafia do przeglądarki):
  `rg -n "NEXT_PUBLIC_" --glob '!node_modules'` — sprawdź, czy to tylko nie-tajne wartości.
- Uwaga handoff: jeśli klucz Jacka był kiedyś w historii gita — sama zmiana pliku nie wystarczy,
  trzeba **wymienić klucz** u operatora (Resend) i wyczyścić.

## Krok 2 — formularze / API (Resend)

Sprawdź `app/api/kontakt/route.ts` i `app/api/newsletter/route.ts`:
- Walidacja serwerowa wymaganych pól (jest: odrzuca brak `email`/`imie` → 400). 
- Limity długości pól (dodaj proste `if (d.wiadomosc?.length > 5000) ...`), żeby uniknąć abuse.
- Honeypot / proste anti-spam na froncie formularza.
- Generyczne komunikaty błędów (bez wycieku stack trace do klienta — jest `console.error` + ogólny
  komunikat, ok).
- **Rate limiting**: po deployu ustaw limit w Vercel WAF / Firewall na `/api/kontakt` i
  `/api/newsletter` (np. kilka żądań na minutę z IP) — chroni przed spam-botami. To konfiguracja w
  panelu Vercel, nie kod.

## Krok 3 — ryzykowne wzorce frontendu

- `rg -n "dangerouslySetInnerHTML|innerHTML|eval\(" --glob '!node_modules'`.
  **Świadomie akceptowane** (udokumentuj, nie alarmuj): `components/WebflowPage.tsx` (HTML z
  własnego eksportu Webflow) i `bodyToHtml` w `lib/sanity.ts` (treść z własnego Sanity Studio).
  To NIE jest input od anonimowego usera → ryzyko niskie. Zaalarmuj tylko, gdy `dangerouslySet...`
  pojawi się na **danych z formularza / query / usera**.
- `rg -n 'target="_blank"' ` — linki zewnętrzne powinny mieć `rel="noopener"` (Footer już ma).

## Krok 4 — nagłówki bezpieczeństwa i zależności

- `next.config.ts`: zaproponuj `async headers()` z `X-Content-Type-Options: nosniff`,
  `Referrer-Policy: strict-origin-when-cross-origin`, `X-Frame-Options: SAMEORIGIN`,
  `Permissions-Policy` (wyłącz nieużywane: kamera/mikrofon/geolokacja).
- `npm audit --audit-level=high` — wypisz realne podatności (jeśli są) i czy jest `npm audit fix`.
- `rm -rf .next && npm run build` — czy projekt się buduje (zepsuty build = nie publikujemy).

## Krok 5 — werdykt

Podsumuj jako:
- 🟢 **Zielone** — można publikować.
- 🟡 **Żółte** — publikacja ok, ale dorób X (np. nagłówki, rate limit).
- 🔴 **Czerwone** — NIE publikować, dopóki nie naprawione (np. sekret w repo).

## Guardrail (krytyczny)

Audyt = czytanie + raport + propozycje. **Nie usuwaj plików, nie nadpisuj `.env`, nie commituj, nie
pushuj, nie deployuj, nie rotuj kluczy** bez wyraźnej zgody użytkownika — nawet jeśli sam to
proponujesz. Przy znalezisku typu „sekret w repo" opisz krok po kroku, co zrobić, i poczekaj na OK.
