# Czy da się zarobić intraday na GPW? Testujemy 4 strategie na spółkach WIG20/mWIG40/sWIG80

Handel intraday kojarzy się z dynamicznymi rynkami — Wall Street, kryptowaluty, forex. Ale co z GPW? Czy krótkoterminowe strategie mają sens na polskim rynku akcji, gdzie płynność jest nieporównywalnie mniejsza? Postanowiliśmy to sprawdzić — backtestem na danych godzinowych z ostatnich 30 dni, na szerokim universum ~140 spółek z indeksów WIG20, mWIG40 i sWIG80.

---

## Zasady testu

Wszystkie strategie działają na tym samym zestawie założeń:

- **Dane:** godzinowe świece OHLCV z Yahoo Finance, ostatnie 30 dni sesyjnych
- **Universum:** ~140 spółek z WIG20, mWIG40 i sWIG80 (skład na 06.03.2026)
- **Kapitał startowy:** 100 000 PLN
- **Koszty transakcyjne:** 0,05% na stronę (0,1% round-trip) — typowe dla polskiego rynku detalicznego
- **Podział kapitału:** równy między wybrane spółki danego dnia
- **Ważna zasada:** sygnał jest wyznaczany na podstawie zamknięcia pierwszej świecy (Close[0]), a wejście następuje dopiero na otwarciu *kolejnej* świecy (Open[1]) — żeby uniknąć lookahead bias

---

## 4 strategie

### 1. Momentum pierwszej godziny

Strategia rankinguje spółki według zwrotu w pierwszej godzinie sesji `(Close[0] - Open[0]) / Open[0]`, wybiera top 3 i kupuje je na otwarciu drugiej godziny. Sprzedaż następuje przy pierwszym spadku zamknięcia poniżej poprzedniego zamknięcia, lub na końcu dnia jeśli pozycja cały czas rosła.

**Logika:** spółka która mocno rośnie w pierwszej godzinie ma momentum — inni inwestorzy zauważą i dołączą.

### 2. Mean Reversion

Odwrotność momentum — wybiera 3 spółki z *największymi spadkami* w pierwszej godzinie, zakładając że po silnym ruchu w dół nastąpi odbicie. Kupuje na Open[1], trzyma do końca dnia.

**Logika:** na płytkim rynku jak GPW przereagowania są częste — spółka która spadła 2% w jednej godzinie bez powodu fundamentalnego często odbija do południa.

### 3. Gap Up

Kupuje spółki które otworzyły się luką w górę powyżej 0,5% względem poprzedniego zamknięcia. Wejście na Open[0] (bo sygnał pochodzi z poprzedniego dnia), wyjście na zamknięciu dnia.

**Logika:** luka w górę sygnalizuje pozytywną zmianę sentymentu — ktoś kupował w nocy lub rano przed otwarciem.

### 4. End-of-Day Momentum

Szuka spółek z co najmniej 2 kolejnymi wzrostowymi świecami w trakcie dnia. Wchodzi po cenie zamknięcia świecy sygnałowej, trzyma do końca dnia.

**Logika:** konsekwentny trend w ciągu dnia ma większą szansę na kontynuację niż jednorazowy impuls.

---

## Wyniki

[WYKRES: plot1_equity_curves.png]

| Strategia | Zwrot | Max DD | Sharpe | Win Rate | Transakcji |
|---|---|---|---|---|---|
| **Momentum** | -3,23% | -8,85% | -1,55 | 31,7% | 63 |
| **Mean Reversion** | **+5,33%** | -5,49% | **2,64** | **55,6%** | 63 |
| **Gap Up** | -17,67% | -16,46% | -10,69 | 30,0% | 60 |
| **EoD Momentum** | -2,98% | -3,12% | -5,91 | 23,8% | 63 |

Wyniki są wymowne. Spośród czterech strategii tylko jedna zakończyła test na plusie — **Mean Reversion** z wynikiem +5,33% w ciągu miesiąca i współczynnikiem Sharpe'a 2,64. Pozostałe trzy przyniosły straty.

[WYKRES: plot2_drawdown.png]

### Co poszło nie tak z Momentum?

Momentum pierwszej godziny wydaje się intuicyjne — podążaj za silnymi spółkami. Problem w tym, że na GPW silny ruch w pierwszej godzinie często *wyczerpuje* popyt na dany dzień. Spółka która wzrosła 3% do 10:00 ma duże szanse na konsolidację lub korektę przez resztę sesji. Wynik: 31,7% skuteczność i -3,23% zwrotu.

[WYKRES: plot3_return_distributions.png]

### Dlaczego Gap Up tak rozczarował?

Gap Up wypadł najgorzej ze wszystkich strategii (-17,67%, Sharpe -10,69). Intuicja jest prosta — luka w górę to dobry znak. W praktyce jednak na GPW luki często są "łatane" tego samego dnia: rynek otwiera się wysoko, ale brakuje dalszych kupujących, więc kurs spada przez resztę sesji. Strategia konsekwentnie kupowała blisko dziennych szczytów.

### Mean Reversion — przypadek czy przewaga?

+5,33% w miesiącu przy Sharpe 2,64 to imponujący wynik — ale należy zachować ostrożność. 30 dni to zbyt krótki okres, żeby wyciągać daleko idące wnioski. Mechanizm jest jednak logiczny dla GPW: przy niskiej płynności przereagowania są częste, a duże instytucje często "zbierają" przecenione akcje w ciągu dnia.

[WYKRES: plot4_win_rate_avg_return.png]

### Najlepsze spółki per strategia

[WYKRES: plot5_top_tickers.png]

Warto zwrócić uwagę na **Captor Therapeutics** — pojawia się zarówno w top Momentum (+18,38%) jak i Mean Reversion (+2,98%), co sugeruje że była to spółka z wyjątkową zmiennością w testowanym okresie, a nie efekt systemowy strategii.

---

## Koszty transakcyjne mają znaczenie

Przy założeniu 0,1% round-trip, każda transakcja "startuje" ze stratą. Przy 63 transakcjach w miesiącu i średniej alokacji ~33 333 PLN na pozycję, same koszty pochłaniają ~2 100 PLN miesięcznie. Dla strategii z małymi średnimi zwrotami (Momentum: -0,14%, EoD: -0,14%) to różnica między zyskiem a stratą.

W praktyce na GPW warto rozważyć:
- Obniżenie częstości transakcji (top 1 zamiast top 3)
- Wyższy próg sygnału (np. gap >1% zamiast >0,5%)
- Filtr płynności — pomijanie spółek z dziennym obrotem poniżej 1 mln PLN

---

## Podsumowanie

Krótkoterminowe strategie intraday na GPW są możliwe — ale rynek nagradza *odwrotność* intuicji. Mean Reversion, czyli zakład na cofnięcie po silnym ruchu, okazuje się skuteczniejszy niż podążanie za momentum. Gap Up, który wielu inwestorów traktuje jako sygnał do kupna, w naszym teście konsekwentnie generował straty.

**To jednak nie jest rekomendacja inwestycyjna** — 30 dni to za mało na wiarygodne wnioski, a wyniki backtestów rzadko przekładają się 1:1 na live trading. Traktuj te wyniki jako punkt wyjścia do dalszych badań, nie jako gotową strategię.

Kod do notebooka dostępny jest na GitHubie [link].

---

*Dane: Yahoo Finance. Backtest wykonany w Pythonie (yfinance, pandas, matplotlib). Koszty transakcyjne 0,1% round-trip. Brak uwzględnienia podatku Belki, poślizgu cenowego i braku płynności.*
