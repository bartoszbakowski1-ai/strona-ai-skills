# jaca-claude-skills

Skille Claude Code do zarządzania stroną **jacekfiedziukiewicz.pl** (Next.js + Vercel). Dzięki nim
możesz przez Claude Code dodawać nowe podstrony w tym samym designie i sprawdzać stronę przed
publikacją — bez znajomości całego kodu.

## Co jest w środku

| Skill | Do czego | Jak wywołać |
|---|---|---|
| `nowa-podstrona` | nowa podstrona / landing w designie JACA AI (navy + orange, nawigacja, stopka, SEO) | `/nowa-podstrona` lub „stwórz nową podstronę" |
| `seo-audyt` | audyt SEO przed publikacją (metadane, H1, sitemap, OG, JSON-LD) | `/seo-audyt` lub „sprawdź seo" |
| `bezpieczenstwo` | audyt bezpieczeństwa (sekrety, formularze, nagłówki, zależności) | `/bezpieczenstwo` lub „czy strona jest bezpieczna" |

- **`PROJEKT.md`** — źródło prawdy o stronie (design system, ścieżki, źródła danych, handoff env).
  Każdy skill czyta go na starcie. Gdy coś w stronie się zmieni — zaktualizuj ten plik.
- **`references/anti-ai-look.md`** — reguły wyglądu, żeby strona nie wyglądała generycznie.

## Instalacja

Skille Claude Code ładuje z folderu `.claude/skills/` w projekcie albo z `~/.claude/skills/`.
Repo trzymasz osobno i podpinasz jego skille do repo strony.

**Rekomendacja — symlink do repo strony** (skille zawsze aktualne po `git pull`):

```bash
# 1. sklonuj to repo (raz)
git clone <URL-twojego-repo> ~/jaca-claude-skills

# 2. w folderze repo STRONY podlinkuj skille
cd /sciezka/do/jacekfiedziukiewicz.pl
mkdir -p .claude/skills
ln -s ~/jaca-claude-skills/skills/nowa-podstrona  .claude/skills/nowa-podstrona
ln -s ~/jaca-claude-skills/skills/seo-audyt        .claude/skills/seo-audyt
ln -s ~/jaca-claude-skills/skills/bezpieczenstwo   .claude/skills/bezpieczenstwo
```

Żeby skille widziały `PROJEKT.md` i `references/`, najprościej trzymać klon repo skilli obok i
odwoływać się do niego, albo skopiować `PROJEKT.md` + `references/` do repo strony. Wariant
najprostszy bez symlinków — **skopiuj** całość:

```bash
cp -R ~/jaca-claude-skills/skills/*      .claude/skills/
cp    ~/jaca-claude-skills/PROJEKT.md    .claude/
cp -R ~/jaca-claude-skills/references    .claude/
```

> Alternatywa zaawansowana: spakować to repo jako plugin Claude Code (marketplace) i instalować
> przez `/plugin`. Niewymagane na start.

## Jak używać

1. Otwórz terminal **w głównym folderze repo strony** (tam, gdzie `package.json`).
2. Zainstaluj zależności i ustaw `.env.local` (`RESEND_API_KEY`, `KONTAKT_EMAIL`).
3. Uruchom Claude Code i wpisz np. `/nowa-podstrona`.

Każdy skill najpierw czyta `PROJEKT.md` i realny kod, więc działa na aktualnym stanie strony.

## Wymagania

- Node + `npm install` w repo strony.
- `.env.local`: `RESEND_API_KEY`, `KONTAKT_EMAIL` (patrz sekcja HANDOFF w `PROJEKT.md`).
- Claude Code uruchamiany z roota repo strony.
