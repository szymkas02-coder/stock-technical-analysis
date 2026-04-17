# 8 strategii intraday na kryptowalutach — która naprawdę działa?

Przetestowałem 8 strategii intraday na 20 największych kryptowalutach. Metodologia taka sama jak w poprzednim projekcie dotyczącym spółek z GPW — żadnych skrótów, żadnego oszukiwania danych. Wyniki są zaskakujące.

---

## Skąd pomysł?

W poprzednim projekcie sprawdzałem strategie intraday na polskich spółkach giełdowych. Naturalne pytanie: czy te same mechanizmy działają na kryptowalutach? Krypto to zupełnie inny rynek — handluje 24/7, jest znacznie bardziej zmienny, a gracze detaliczni dominują nad instytucjonalnymi. Postanowiłem to sprawdzić empirycznie.

Ale nie chciałem testować strategii tylko na Bitcoinie. To zbyt wąski wycinek rynku. Zamiast tego przyjąłem podejście portfelowe: **każdego dnia rankinguję wszystkie 20 kryptowalut** według sygnału danej strategii i inwestuję równomiernie w **top-3** najlepiej rokujące. Dokładnie tak samo jak przy spółkach.

---

## Dane i metodologia

**Dane:** Yahoo Finance, interwał 1 godzina, ostatnie 30 dni, 20 największych kryptowalut według kapitalizacji rynkowej (Bitcoin, Ethereum, BNB, Solana, XRP, Cardano, Avalanche, Dogecoin, TRON, Polkadot, Chainlink, Polygon, Litecoin, Uniswap, Cosmos, Stellar, NEAR, Internet Computer, Filecoin, Algorand).

**Kapitał startowy:** 10 000 USD

**Koszty transakcyjne:** 0,1% na stronę (0,2% round-trip) — typowe dla Binance czy Coinbase.

**Kluczowa zasada (brak lookahead bias):**
```
Sygnał  → bazuje na danych z zamknięcia świecy [0]
Wejście → Open[1], pierwsza cena po potwierdzeniu sygnału
```
To pozornie drobny szczegół ma ogromne znaczenie. W poprzednim projekcie pominięcie tej zasady (kupno na Open[0] zamiast Open[1]) zmieniło wynik z +90% na -3%. Dlatego pilnujemy jej rygorystycznie.

[WYKRES: crypto_plot1_equity_curves.png]

---

## 8 strategii — skąd się wzięły?

Przetestowałem dwie grupy strategii:

**Przeniesione z projektu GPW** (dostosowane do 24/7 krypto):
1. Momentum pierwszej godziny
2. Mean Reversion pierwszej godziny
3. Gap Up (luka vs poprzednie zamknięcie)
4. EoD Streak (ciąg wzrostowych świec)

**Nowe — specyficzne dla krypto:**
5. RSI Oversold
6. Bollinger Bands
7. EMA Golden Cross
8. MACD + RSI Hybrid

---

## Jak działają strategie

### 1. Momentum 1h
Każdego dnia obliczamy zwrot pierwszej godziny handlu `(Close[0] - Open[0]) / Open[0]` dla każdej z 20 kryptowalut. Kupujemy top-3 z najsilniejszym momentum na otwarciu drugiej świecy. Wyjście gdy cena zaczyna spadać (trailing stop) lub na zamknięciu doby.

### 2. Mean Reversion 1h
Odwrotność poprzedniej — kupujemy bottom-3, czyli krypto które w pierwszej godzinie spadły najmocniej. Zakład: duże krótkoterminowe ruchy są przesadzone i cena wróci do średniej. Wyjście EOD.

### 3. Gap Up
Szukamy kryptowalut, które otworzyły się więcej niż 0,5% powyżej poprzedniego zamknięcia. Kupujemy top-3 według wielkości luki. Wyjście EOD.

### 4. EoD Streak
Liczymy ile kolejnych świec z rzędu dana kryptowaluta rośnie (na całej dostępnej historii godzinowej, kończąc na Close[0]). Top-3 z najdłuższym ciągiem wzrostów — zakład na kontynuację trendu. Wyjście EOD.

### 5. RSI Oversold
Wskaźnik RSI(14) obliczany na pełnej historii godzinowej. Kwalifikuje się kryptowaluta z RSI poniżej 30 na Close[0] — klasyczny sygnał wyprzedania. Rankujemy wg najniższego RSI, kupujemy top-3. Wyjście EOD.

### 6. Bollinger Bands
Pasma Bollingera (okno 20 świec, 2 odchylenia standardowe). Sygnał gdy Close[0] spada poniżej dolnej bandy. Rankujemy wg odchylenia od bandy (im dalej poniżej, tym wyższy priorytet). Wyjście EOD.

### 7. EMA Golden Cross
Szukamy kryptowalut, gdzie EMA(8) właśnie przekroczyła EMA(21) w górę w ciągu ostatnich 3 świec. Rankujemy wg siły separacji między liniami. To klasyczny sygnał trendowy. Wyjście EOD.

### 8. MACD + RSI Hybrid
Sygnał tylko gdy spełnione są dwa warunki jednocześnie: MACD > linia sygnału MACD **oraz** RSI < 40. Rankujemy wg siły łączonego sygnału: `(MACD - Signal) × (40 - RSI)`. Wyjście EOD.

---

## Wyniki

| Strategia | Zwrot | Max DD | Sharpe | Win Rate | Transakcji |
|---|---|---|---|---|---|
| 1. Momentum | -2,29% | -6,22% | -1,61 | 30,2% | 96 |
| 2. Mean Reversion | -16,83% | -15,39% | -2,49 | 34,4% | 96 |
| 3. Gap Up | brak | — | — | — | 0 |
| 4. EoD Streak | -14,40% | -4,42% | -3,55 | 52,1% | 48 |
| 5. RSI Oversold | -14,25% | -12,93% | -4,11 | 30,6% | 36 |
| 6. Bollinger Bands | -3,48% | -3,22% | -6,58 | 12,5% | 8 |
| 7. **EMA Golden Cross** | **+21,79%** | **-2,01%** | **7,09** | **52,2%** | 23 |
| 8. MACD + RSI | -2,69% | -5,24% | -1,95 | 28,6% | 14 |

[WYKRES: crypto_plot4_win_rate.png]

---

## Co wynika z tych liczb?

### EMA Golden Cross — jedyny wyraźny zwycięzca

+21,79% w 30 dni przy max drawdown zaledwie -2,01% i Sharpe 7,09 — to bardzo dobry wynik. Strategia wygenerowała tylko 23 transakcje, co oznacza selektywność: wchodzi rzadko, ale celnie. Top krypto to NEAR (+16,4%), Chainlink (+6%), Cardano (+3,7%).

Intuicja jest prosta: złoty krzyż EMA(8/21) na wykresie godzinowym oznacza że krótkoterminowe momentum właśnie zmieniło kierunek na wzrostowy. W środowisku krypto, gdzie trendy bywają gwałtowne, taki sygnał ma realne znaczenie.

Warto jednak zachować ostrożność — 23 transakcje to mało obserwacji. Wynik może być częściowo szczęściem albo przypadkiem trafionego okna czasowego.

[WYKRES: crypto_plot2_drawdown.png]

### Mean Reversion — najgorszy wynik

-16,83% i win rate 34,4%. Strategia Mean Reversion działa odwrotnie do intuicji: kryptowaluta która mocno spada w pierwszej godzinie... często spada dalej. Na rynku krypto momentum jest ważniejsze niż powrót do średniej. Spadające krypto przyciągają kolejnych sprzedających, nie kupujących.

To odwrotność wyniku z GPW, gdzie Mean Reversion jako jedyna strategia zakończyła się na plusie (+5,33%). Rynki akcji i krypto zachowują się inaczej.

### RSI i Bollinger — pułapka klasycznych wskaźników

RSI Oversold: -14,25%, win rate 30,6%. Bollinger Bands: -3,48%, win rate 12,5%. Oba wskaźniki zostały zaprojektowane z myślą o rynkach o ograniczonej zmienności. Na krypto RSI < 30 wcale nie oznacza że coś jest "tanie" — może oznaczać że właśnie zaczął się crash. Dolna banda Bollingera to nie podłoga, tylko obserwacja że cena jest nisko względem ostatnich 20 świec.

### Gap Up — zero transakcji

Strategia Gap Up nie wygenerowała ani jednej transakcji. Próg 0,5% luki okazał się zbyt wysoki albo krypto w badanym oknie czasowym nie tworzyły istotnych luk godzinowych. Rynek 24/7 nie ma "zamknięcia" w tradycyjnym sensie — kryptowaluty handlują nieustannie, więc luki overnight praktycznie nie istnieją.

[WYKRES: crypto_plot3_distributions.png]

### EoD Streak — wysoki win rate, ale ogromna strata

Ciekawy przypadek: win rate 52,1% (większość transakcji na plusie) ale łączny zwrot -14,40%. Dlaczego? Rozkład zwrotów jest asymetryczny — małe zyski i sporadyczne katastrofalne straty. Najgorsza transakcja: -15,27%. Na krypto długie serie wzrostów często kończą się gwałtownym odwróceniem.

[WYKRES: crypto_plot5_top_tickers.png]

---

## Porównanie z wynikami na GPW

| Strategia | GPW (30 dni) | Krypto (30 dni) |
|---|---|---|
| Momentum | -3,23% | -2,29% |
| Mean Reversion | **+5,33%** | -16,83% |
| Gap Up | -17,67% | brak transakcji |
| EoD Streak | -2,98% | -14,40% |

Momentum daje podobne słabe wyniki na obu rynkach. Mean Reversion jest najlepszą strategią na GPW i najgorszą na krypto. Gap Up jest katastrofalny na GPW i bezużyteczny na krypto. To pokazuje że te same mechanizmy działają zupełnie inaczej w zależności od rynku.

---

## Ograniczenia i zastrzeżenia

**Krótki okres testów.** 30 dni to za mało żeby wyciągać mocne wnioski. Wyniki EMA Golden Cross mogą być specyficzne dla konkretnego miesiąca, w którym kryptowaluta NEAR akurat przeżywała silny ruch.

**Dane Yahoo Finance.** Dane godzinowe dla kryptowalut z Yahoo Finance mogą zawierać luki i nieścisłości, szczególnie dla mniej płynnych tokenów.

**Brak slippage.** Model zakłada egzekucję po dokładnym Open[1]. W rzeczywistości dla dużych zleceń cena wykonania będzie gorsza.

**Podatki i regulacje.** Handel kryptowalutami podlega opodatkowaniu. W Polsce zyski z krypto są opodatkowane 19% podatkiem od zysków kapitałowych (Belka). Częste transakcje intraday generują dużo zdarzeń podatkowych.

**To nie jest doradztwo inwestycyjne.** Wyniki historyczne nie gwarantują przyszłych zysków. Inwestowanie w kryptowaluty wiąże się z wysokim ryzykiem utraty kapitału.

---

## Kod

Pełny kod dostępny w repozytorium: notebook Python z pobieraniem danych, implementacją wszystkich 8 strategii i wizualizacjami.
