# Anti-AI-look — reguły wyglądu dla strony JACA AI

Skrót sprawdzonego rulesetu, dopasowany do marki Jacka. Cel: strona ma wyglądać jak zaprojektowana
przez człowieka, nie jak generyczny output AI. Przepuść przez to każdą nową podstronę.

## Stałe marki (NIE zmieniać bez zgody)
- Paleta: navy `#0b1218` (tło), biały tekst, **jeden** akcent orange `#ff9022`.
- Fonty: IBM Plex Sans (body + nagłówki, nagłówki waga 300), IBM Plex Mono (akcenty).
- Żadnego gradientu violet/blue, żadnego "dark mode + neon", żadnego tęczowego gradient-text.

## Język i typografia
- Polskie ogonki **wszędzie**: treść, `title`, `description`, `alt`, etykiety przycisków, nagłówki.
- **Zero em-dash (—) i en-dash (–).** Krótki łącznik `-` ok. Nie sklejaj dwóch zdań myślnikiem
  ze spójnikiem ("- i", "- a", "- oraz").
- Nadtytuły (eyebrow) krótkie, mogą być wersalikami (`text-style-tagline`), ale nie krzycz CAPSEM
  w całych nagłówkach.
- Nagłówki tej samej sekcji nie muszą mieć identycznej długości — naturalny rytm > sztuczna symetria.

## Layout
- Hero z prawdziwą treścią, nie pustym wyśrodkowanym claimem na pół ekranu.
- Oddech: duże odstępy między sekcjami (`padding-section-large`). Nie upychaj.
- Nie wszystko wyśrodkowane — mieszaj wyrównanie do lewej i do środka.
- Karty: gdy powtarzasz `.jaca-card` / `.jaca-offer-card`, niech treść realnie się różni; unikaj
  rzędu identycznych kafelków z tym samym hoverem i tą samą ikoną.
- Wypełniaj siatki: `.jaca-grid-3` → liczba kart wielokrotność 3 (albo zmień layout). Zero pustych
  komórek i osieroconych elementów.

## Treść (copy)
- Konkret > buzzword. Realne liczby, nazwy, przykłady ("oszczędza ~10 h tygodniowo") zamiast
  "synergiczne, holistyczne rozwiązania AI".
- Jeśli nie znasz liczby/case'u — nie zmyślaj. Napisz uczciwie lub pomiń.
- Mówisz do konkretnej osoby (przedsiębiorca, który chce wdrożyć AI), nie do "wszystkich".

## Obrazy i ikony
- Realne zdjęcia/grafiki tam, gdzie się da; nie sama typografia na pustym tle.
- Ikony spójne (np. jeden zestaw), nie emoji jako ikony interfejsu, nie każda ikona w identycznym
  kółku w kółko.

## Ruch
- Minimum jedna subtelna animacja (np. delikatny fade/scroll-in), żeby strona nie była martwa.
- Bez animacji w nieskończonej pętli (pulse/bounce/blob) na całej stronie. Animacje wejścia raz
  (`once`), z poszanowaniem `prefers-reduced-motion`.

## Szybki test przed publikacją
1. Czy widać tylko navy + orange (zero obcych kolorów)?  
2. Czy są polskie ogonki i zero em-dash?  
3. Czy jest dokładnie jeden `<h1>`?  
4. Czy sekcje mają oddech i nie są rzędem identycznych kart?  
5. Czy copy ma konkret zamiast buzzwordów?
