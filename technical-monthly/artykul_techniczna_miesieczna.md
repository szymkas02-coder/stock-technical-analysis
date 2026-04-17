# Algorytm zamienił 10 000 zł w 500 000 zł. Ale bez podatku byłoby 2 miliony.

*Czas czytania: ~7 min*

---

## Pytanie które mnie nurtowało

W poprzednim teście strategia hybrydowa z dziennym stop-lossem dała +2722% w 25 lat — ale bez podatku Belki. Czy to samo zadziała z miesięcznym cyklem decyzyjnym i z podatkiem wliczonym wprost w model?

Dane: te same — 354 spółki GPW, 2000–2025. Kapitał startowy: 10 000 PLN. Tym razem jednak każda sprzedaż z zyskiem od razu odejmuje 19%.

---

## Jak działa ten model?

Scoring techniczny pozostał identyczny — RSI, MACD, Stochastic, CCI, ROC i relacja EMA20/EMA50, łączone w wynik 0–100% dla każdej spółki. Zmiana jest w rytmie decyzji: zamiast reagować codziennie na stop-loss, portfel jest przeglądany **raz w miesiącu**.

Spółka wypada z portfela gdy jej score spada poniżej progu sprzedaży. Nowe pozycje wchodzą gdy score przekracza próg wejścia. Portfel trzyma do 10 spółek jednocześnie.

Trzy warianty różnią się szerokością "korytarza":

| | S1 Konserwatywny | S2 Standardowy | S3 Agresywny |
|---|---|---|---|
| Próg wejścia | 80% | 60% | 40% |
| Próg sprzedaży | 40% | 30% | 20% |

---

## Wyniki

*(tu wstaw wykres `m01_kapital_porownanie.png`)*
*(tu wstaw tabelę `m07_tabela.png`)*

Wszystkie trzy strategie zarobiły wielokrotnie. Ale kolejność zaskoczyła.

**S3 Agresywny wygrał z +4997%** — 10 000 PLN zamieniło się w 509 673 PLN mimo zapłacenia 136 387 PLN podatku. Niski próg wejścia (40%) oznacza że portfel jest prawie zawsze w pełni zainwestowany, a niski próg sprzedaży (20%) daje spółkom dużo przestrzeni zanim zostaną wyrzucone. Tylko 211 transakcji przez 25 lat.

**S1 Konserwatywny uplasował się drugi z +3222%** — 332 182 PLN końcowego kapitału, ale kosztem 1025 transakcji i 228 095 PLN podatku. Wysoki próg wejścia (80%) sprawia że portfel jest selektywny — co ironicznie generuje częstszą rotację i wyższy rachunek podatkowy.

**S2 Standardowy wypadł najsłabiej z +2285%** — 238 523 PLN. Środkowe progi nie dają ani ochrony konserwatywnego podejścia, ani swobody agresywnego.

---

## Główna lekcja: podatek Belki może zabrać 85% zysku

*(tu wstaw wykres `m04_brutto_vs_netto.png`)*

To jest wykres który zatrzymuje.

S1 bez podatku zarobiłby **2 168 667 PLN**. Z podatkiem — 332 182 PLN. Podatek zabrał **1 836 485 PLN** — ponad 84% całego wypracowanego zysku.

Skąd taka różnica? S1 wykonał 1025 transakcji — średnio ponad 3 miesięcznie przez 25 lat. Każda sprzedaż z zyskiem to natychmiastowe 19% do urzędu skarbowego. Zyski które mogłyby procentować latami są realizowane i opodatkowywane na bieżąco.

S3 przy 211 transakcjach zapłacił "tylko" 136 387 PLN podatku — sześciokrotnie mniej niż S1, mimo lepszego wyniku końcowego. Rzadsze decyzje = rzadziej realizowane zyski = rzadziej płacony podatek.

---

## Rok po roku

*(tu wstaw wykres `m05_zwrot_roczny.png`)*

Strategia nie jest równa w czasie. Lata 2000–2001 przyniosły straty (bańka dot-com). Rok 2008 był bolesny — S1 i S2 straciły ponad 60% w jednym roku.

Ale lata wzrostowe kompensują z nawiązką: S1 w 2002 +254%, w 2020 +362%. S3 w 2021 +214%. Te kilka wyjątkowych lat robi cały wynik — co jest zarówno siłą jak i słabością strategii momentum.

---

## Drawdown — cena tej strategii

*(tu wstaw wykres `m02_drawdown.png`)*

Max drawdown S1 wynosi -80,2%. W najgorszym momencie portfel był wart jedną piątą swojej szczytowej wartości. Szczególnie uderzający jest okres 2008–2020 dla S1 — przez ponad dekadę strategia nie wróciła do poprzednich szczytów. Kto by to psychicznie wytrzymał?

S3 paradoksalnie radzi sobie tu najlepiej — max drawdown "tylko" -73,2% i szybciej wraca do szczytów dzięki szerokiemu korytarzowi który nie wyrzuca spółek przy każdym chwilowym osłabieniu.

---

## Ograniczenia

- **Survivorship bias** — spółki które zbankrutowały mogą nie być w danych
- **Look-ahead bias** — scoring i transakcja po tej samej cenie końca miesiąca
- **Podatek uproszczony** — pobierany natychmiast zamiast raz rocznie; nie uwzględnia odliczenia strat od zysków
- **Brak kosztów transakcyjnych** — przy 1025 transakcjach S1 prowizje maklerskie dodatkowo zmniejszyłyby wynik
- **Dane bez dywidend** — spółki dywidendowe są niedocenione

---

## Podsumowanie

Strategia działa — ale sposób zarządzania portfelem ma fundamentalne znaczenie. S3 z niskim progiem i rzadkimi transakcjami pobił S1 z wysokim progiem i częstą rotacją.

Najważniejszy wniosek tej analizy: przy aktywnej strategii podatek Belki potrafi zabrać 85% zysku. Rachunek IKE lub IKZE przestaje być "miłym dodatkiem" — staje się różnicą między 332 000 a 2 000 000 PLN na tym samym algorytmie.

To będzie temat następnego testu.

Kod dostępny na GitHubie [link].

---

*Analiza i kod: Python + Claude AI (Anthropic). Nie jest to rekomendacja inwestycyjna.*
