# Zainwestowałem 10 000 zł w algorytm giełdowy. Po 25 latach miałem 282 000 zł.

*Czas czytania: ~7 min*

---

## Skąd ten pomysł?

Większość algorytmów giełdowych które widzicie w internecie jest albo zbyt prosta (kup i trzymaj), albo zbyt skomplikowana (modele ML z dziesiątkami parametrów). Chciałem sprawdzić coś pośrodku — klasyczne wskaźniki techniczne, portfel 10 spółek, i jedno kluczowe pytanie: **jak szeroki powinien być stop-loss?**

Dane: 354 spółki z GPW, ceny dzienne od 2000 do 2025. Kod w Pythonie, zero płatnych bibliotek.

---

## Jak działa algorytm?

Każdego dnia każda spółka otrzymuje wynik punktowy od 0 do 1, obliczony z siedmiu klasycznych wskaźników technicznych:

- **RSI(14)** i **MACD** — najsilniejszy sygnał momentum (łącznie 40% wagi)
- **ROC(10)** — tempo zmiany ceny (20%)
- **Stochastic %K** i **CCI** — oscylatory wyprzedania/wykupienia (20%)
- **EMA20 vs EMA50** — potwierdzenie trendu (20%)

Portfel trzyma jednocześnie **10 najlepiej scorowanych spółek**. Gdy spółka spada poniżej progu stop-loss od ceny zakupu — wylatuje, a jej miejsce zajmuje kolejna najlepsza kandydatka.

Żadnej czarnej skrzynki. Każdy może to zreplikować.

---

## Trzy warianty stop-loss

Testowałem trzy progi wyjścia z pozycji:

| | Stop −10% | Stop −20% | Stop −30% |
|---|---|---|---|
| Kapitał końcowy | **282 200 PLN** | 215 777 PLN | 168 130 PLN |
| Zwrot | **+2722%** | +2058% | +1581% |
| Max Drawdown | −58,6% | −69,8% | −74,0% |
| Transakcje (BUY) | 129 | 83 | 83 |
| Win Rate | **46,9%** | 42,1% | 42,0% |

---

## Wyniki

*(tu wstaw wykres `h01_kapital_porownanie.png`)*

10 000 PLN zainwestowane w 2000 roku zamieniło się w **282 200 PLN** przy wariancie ze stop-lossem −10%. Wszystkie trzy warianty zarobiły wielokrotnie — ale różnice między nimi są znaczące.

**Stop −10% wygrał pod każdym względem** — najwyższy kapitał końcowy, najwyższy win rate i paradoksalnie najniższy max drawdown spośród trzech. Krótszy stop-loss chroni kapitał szybciej i pozwala wcześniej rotować do lepszych spółek.

**Stop −30% mimo najmniejszej liczby transakcji wypadł najsłabiej.** Szeroki próg oznacza że portfel "siedzi" w przegrywających pozycjach dłużej, tracąc więcej zanim zareaguje. Mniej transakcji nie oznacza lepszego wyniku — to wniosek powtarzający się w każdym z moich testów.

---

## Rok po roku

*(tu wstaw wykres `h04_zwrot_roczny.png`)*

Algorytm nie był równy w czasie. Lata 2000–2003 (pęknięcie bańki dot-com) i 2008–2009 (kryzys finansowy) przyniosły straty we wszystkich wariantach. Szczególnie 2008 rok był bolesny — Stop −30% stracił 55% wartości w ciągu jednego roku.

Ale w latach wzrostowych algorytm kompensował to z nawiązką: 2004 (+89% dla Stop −10%), 2007 (+78%), 2012–2013 (+76%). Lata 2020–2025 to seria kolejnych wzrostów przekraczających 40% rocznie.

---

## Drawdown — koszt agresywnej strategii

*(tu wstaw wykres `h02_drawdown.png`)*

To jest największe zastrzeżenie do tych wyników. Max drawdown −58,6% przy Stop −10% oznacza, że w najgorszym momencie portfel był wart o ponad połowę mniej niż w szczycie. Przy Stop −30% było to −74%.

Czy dałoby się to emocjonalnie wytrzymać? Wątpię żeby większość inwestorów trzymała się algorytmu przez dołek 2008–2009 widząc jak portfel traci 60% wartości. To jest różnica między backtestem a rzeczywistością — liczby są prawdziwe, ale psychologia inwestora nie jest.

---

## Co te wyniki oznaczają — i czego nie oznaczają

**Kilka ważnych zastrzeżeń:**

Brak kosztów transakcyjnych — przy 129 transakcjach BUY przez 25 lat to nie jest problem (prowizje zmniejszyłyby wynik marginalnie), ale warto o tym pamiętać.

Survivorship bias — dane mogą nie zawierać spółek które zbankrutowały lub zostały wycofane z GPW w tym czasie. To zawyża wyniki, trudno powiedzieć o ile.

Look-ahead bias — scoring i zakup realizowane są po tej samej cenie dnia. W praktyce sygnał byłby dostępny dopiero po zamknięciu sesji, zakup następnego dnia rano po innej cenie.

Brak podatku Belki — w rzeczywistości każda sprzedaż z zyskiem to 19% podatku. Przy 25 latach i wielokrotnym wzroście kapitału to by znacząco obniżyło wyniki.

**Dane bez dywidend** — `open_prices.csv` zawiera prawdopodobnie ceny bez reinwestowanych dywidend. Spółki dywidendowe (banki, PZU, KGHM) są więc systematycznie niedocenione. Prawdziwy total return byłby wyższy.

---

## Podsumowanie

Algorytm hybrydowy łączący scoring techniczny z portfelem 10 spółek i stop-lossem −10% dał +2722% w 25 lat na GPW. To imponujące — ale zanim wyciągniecie wnioski inwestycyjne, warto mieć w głowie wszystkie ograniczenia powyżej.

Kod dostępny na GitHubie [link]. Następny krok który mnie interesuje: jak te wyniki wyglądają z podatkiem Belki i kosztami transakcyjnymi wliczonymi wprost w model.

---

*Analiza i kod: Python + Claude AI (Anthropic). Nie jest to rekomendacja inwestycyjna.*
