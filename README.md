# strona-ai-skills

Zestaw skilli Claude Code do **rozbudowy i utrzymania strony zbudowanej w kursie "Stwórz stronę z AI"**
(Next.js + Vercel). Dzięki nim możesz przez Claude Code dodawać nowe podstrony w spójnym designie,
zbudować stronę oferty swoich usług i sprawdzić stronę przed publikacją - bez znajomości całego kodu.

Skille są **ogólne**: działają na dowolnej stronie zrobionej w standardzie kursu (App Router, design
tokens w `app/globals.css`, komponenty nawigacji i stopki, formularz kontaktowy przez Resend).
Wszystkie fakty o Twojej konkretnej stronie skille biorą z pliku **`PROJEKT.md`**, który raz
uzupełniasz pod siebie.

## Co jest w środku

| Skill | Do czego | Jak wywołać |
|---|---|---|
| `oferta` | strona oferty Twoich usług (pakiety, korzyści, CTA) w designie Twojej strony | `/oferta` lub „zrób stronę oferty" |
| `nowa-podstrona` | nowa podstrona / landing w istniejącym design systemie (nawigacja, stopka, SEO) | `/nowa-podstrona` lub „stwórz nową podstronę" |
| `seo-audyt` | audyt SEO przed publikacją (metadane, H1, sitemap, OG, JSON-LD) | `/seo-audyt` lub „sprawdź seo" |
| `bezpieczenstwo` | audyt bezpieczeństwa (sekrety, formularze, nagłówki, zależności) | `/bezpieczenstwo` lub „czy strona jest bezpieczna" |

- **`PROJEKT.md`** - źródło prawdy o Twojej stronie (design system, ścieżki, źródła danych, env).
  Uzupełnij go raz pod swoją stronę. Każdy skill czyta go na starcie. Gdy coś w stronie się zmieni -
  zaktualizuj ten plik.
- **`references/anti-ai-look.md`** - reguły wyglądu, żeby strona nie wyglądała generycznie.

## Instalacja

Skille Claude Code ładują się z folderu `.claude/skills/` w projekcie albo z `~/.claude/skills/`.
Repo trzymasz osobno i podpinasz jego skille do repo strony.

**Rekomendacja - symlink do repo strony** (skille zawsze aktualne po `git pull`):

```bash
# 1. sklonuj to repo (raz)
git clone <URL-tego-repo> ~/strona-ai-skills

# 2. w folderze repo STRONY podlinkuj skille
cd /sciezka/do/twojej-strony
mkdir -p .claude/skills
ln -s ~/strona-ai-skills/skills/oferta          .claude/skills/oferta
ln -s ~/strona-ai-skills/skills/nowa-podstrona  .claude/skills/nowa-podstrona
ln -s ~/strona-ai-skills/skills/seo-audyt       .claude/skills/seo-audyt
ln -s ~/strona-ai-skills/skills/bezpieczenstwo  .claude/skills/bezpieczenstwo
```

Żeby skille widziały `PROJEKT.md` i `references/`, najprościej trzymać klon repo skilli obok i
odwoływać się do niego, albo skopiować `PROJEKT.md` + `references/` do repo strony. Wariant
najprostszy bez symlinków - **skopiuj** całość:

```bash
cp -R ~/strona-ai-skills/skills/*      .claude/skills/
cp    ~/strona-ai-skills/PROJEKT.md    .claude/
cp -R ~/strona-ai-skills/references    .claude/
```

> Alternatywa zaawansowana: spakować to repo jako plugin Claude Code (marketplace) i instalować
> przez `/plugin`. Niewymagane na start.

## Jak używać

1. **Uzupełnij `PROJEKT.md`** pod swoją stronę (raz). To 10 minut, a wszystkie skille działają potem
   na faktach Twojej strony zamiast zgadywać.
2. Otwórz terminal **w głównym folderze repo strony** (tam, gdzie `package.json`).
3. Zainstaluj zależności i ustaw `.env.local` (`RESEND_API_KEY`, `KONTAKT_EMAIL`).
4. Uruchom Claude Code i wpisz np. `/oferta` albo `/nowa-podstrona`.

Każdy skill najpierw czyta `PROJEKT.md` i realny kod, więc działa na aktualnym stanie strony.

## Wymagania

- Node + `npm install` w repo strony.
- `.env.local`: `RESEND_API_KEY`, `KONTAKT_EMAIL` (patrz sekcja ENV w `PROJEKT.md`).
- Claude Code uruchamiany z roota repo strony.
