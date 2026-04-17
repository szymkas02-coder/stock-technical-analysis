# Sprawdziłem strategię momentum na GPW przez 10 lat. 1 000 zł zamieniło się w 13 000 zł.

*Czas czytania: ~6 min*

---

## Pomysł

Idea brzmi przekonująco: kupuj spółki, które ostatnio rosną, sprzedawaj te, które zwalniają. Momentum działa — przynajmniej tak mówi literatura akademicka. Postanowiłem sprawdzić, czy zadziała na Giełdzie Papierów Wartościowych w Warszawie w latach 2015–2025.

Do analizy użyłem Pythona i danych dla 354 spółek z GPW. Kod i notebook dostępne na GitHubie [link].

---

## Jak działa strategia?

Każdego miesiąca oceniam każdą spółkę punktowo — od 0 do 6 punktów — na podstawie trzech horyzontów:

- **1 miesiąc** (waga: 3 pkt) — bieżące momentum
- **12 miesięcy** (waga: 2 pkt) — trend roczny
- **36 miesięcy** (waga: 1 pkt) — fundamentalny wzrost długoterminowy

Punkty przyznawane są liniowo proporcjonalnie w zadanym przedziale. Wynik przeliczam na procenty (0–100%). Najlepsze 10 spółek wchodzi do portfela.

Nie ma tu żadnej czarnej skrzynki — każdy może to zreplikować.

---

## Trzy warianty

Testowałem trzy podejścia, żeby zobaczyć jak sposób zarządzania portfelem wpływa na wynik — niezależnie od samego scoringu.

| | S1: Miesięczny rebalans | S2: Minimalny rebalans | S3: Szeroki korytarz |
|---|---|---|---|
| Próg wejścia | 99% | 99% | 70% |
| Próg sprzedaży | 90% | 90% | 30% |
| Rebalansacja | Co miesiąc | Tylko przy zmianie | Tylko przy zmianie |

---

## Wyniki

*(tu wstaw wykres `01_kapital_porownanie.png`)*

*(tu wstaw tabelę `05_tabela_podsumowanie.png`)*

Wszystkie trzy strategie zarobiły. Ale różnice są ogromne.

**S2 — minimalny rebalans** okazała się bezkonkurencyjna: z 1 000 PLN do **13 355 PLN**, czyli zwrot +1 235% w 10 lat. Co ciekawe — zapłaciła przy tym aż **6 595 PLN podatku**. Mimo to zdominowała wynik, bo zyski były po prostu na tyle duże, że podatek nie miał znaczenia.

**S3 — szeroki korytarz** zarobiła +472% przy zaledwie 200 transakcjach i 1 356 PLN podatku. Znacznie spokojniejsza w obsłudze, solidny wynik — ale ponad 2,5x gorszy od S2. Mniej rotacji nie oznacza lepszego wyniku.

**S1 — miesięczny rebalans** wypadła najsłabiej: +290% i 1 646 transakcji. Ciągłe wyrównywanie wag zmuszało do sprzedawania wygrywających pozycji — to właśnie tutaj rebalansacja szkodziła.

---

## Główna lekcja: nie przeszkadzaj wygrywającym

To był dla mnie kluczowy wniosek. S2 nie robiła nic szczególnego — po prostu **nie sprzedawała spółek, które nadal rosły**. Próg sprzedaży na 90% oznacza, że spółka musiała wyraźnie wyhamować, żeby wylecieć z portfela. Dzięki temu najlepsze pozycje mogły rosnąć latami.

S1 co miesiąc sprzedawała część wygrywających, żeby wyrównać wagi — i dokupowała słabsze. Klasyczna pułapka rebalansacji w strategii momentum.

Ciekawy paradoks pojawia się przy drawdownie: S3 z najszerszym korytarzem miała największy max drawdown (-32,5%), podczas gdy S1 i S2 zatrzymały się na ok. -25%. Mniej transakcji oznaczało też wolniejszą reakcję na pogorszenie koniunktury.

---

## Co dalej?

Model ma kilka uproszeń, które warto mieć w głowie:

- **Brak kosztów transakcyjnych** — prowizje maklerskie (0,2–0,4%) zmniejszyłyby wyniki, szczególnie S1 z 1 646 transakcjami.
- **Survivorship bias** — dane mogą nie obejmować spółek, które zbankrutowały lub zostały wycofane z obrotu. To zawyża wyniki.
- **Look-ahead bias** — scoring i transakcja wykonywane są po tej samej cenie zamknięcia miesiąca. W praktyce sygnał byłby dostępny dopiero po sesji.
- **Podatek uproszczony** — model pobiera go od każdej transakcji na bieżąco; realnie rozlicza się go raz do roku w PIT-38.

Kierunki do dalszej eksploracji: wyniki na rachunku IKE/IKZE (brak podatku Belki), wpływ kosztów transakcyjnych na S1, replikacja na rynkach zachodnich.

---

## Podsumowanie

Momentum na GPW zadziałało — w odpowiedniej formie. Kluczem okazało się nie przeszkadzanie spółkom, które rosną. S2, która po prostu trzymała pozycje dopóki wyraźnie nie wyhamowały, zarobiła ponad 13x w 10 lat, mimo zapłacenia 6 600 PLN podatku.

Kod dostępny na GitHubie [link]. Jeśli masz pomysł jak poprawić model albo chcesz zobaczyć wersję na IKE — napisz w komentarzu.

---

*Analiza i kod: Python + Claude AI (Anthropic). Nie jest to rekomendacja inwestycyjna.*
