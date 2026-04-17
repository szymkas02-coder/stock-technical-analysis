# Day trading nie ma sensu — 40 lat danych Apple jako dowód

Miliony ludzi codziennie patrzy na RSI, MACD i stochastyczny, żeby zdecydować czy kupić czy sprzedać. Wziąłem 40 lat danych Apple i sprawdziłem empirycznie, czy te wskaźniki w ogóle coś mówią o jutrzejszym kursie.

Odpowiedź jest jednoznaczna — i ma konkretne konsekwencje dla każdego, kto myśli o day tradingu.

---

## Dane i metodologia

**Dane:** kurs AAPL z Yahoo Finance, okres 1980–2026, 11 365 sesji giełdowych.

**Cechy:** RSI(14), Stochastic %K i %D (14,3,3), EMA(20), MACD linia/sygnał/histogram, Williams %R(14), kurs zamknięcia poprzedniego dnia. To standardowy zestaw wskaźników z każdej platformy tradingowej.

**Pytanie:** czy znając te wartości z dnia t, można skuteczniej niż losowo przewidzieć ruch kursu w dniu t+1?

Walidacja uczciwa: chronologiczny podział 90%/10%, model trenowany na danych do 2021 roku, testowany na 2021–2026. Model: XGBoost z doborem hiperparametrów przez cross-validation.

[WYKRES: plot1_price_returns.png]

---

## Zanim odpalimy modele — autokorelacja zwrotów

Pierwsza analiza, zanim w ogóle trenujemy cokolwiek: autokorelacja dziennych zwrotów.

[WYKRES: plot3_acf_pacf.png]

Każdy słupek na wykresie ACF to korelacja dzisiejszego zwrotu ze zwrotem sprzed N dni. Niebieska wstęga to 95% przedział ufności — jeśli słupek jest w środku, nie ma statystycznie istotnego sygnału.

Przy żadnym opóźnieniu od 1 do 40 dni sygnał nie wychodzi poza wstęgę. Statystyka Durbina-Watsona wynosi ~2,0, co odpowiada brakowi autokorelacji.

**Co to znaczy praktycznie:** to, co Apple zrobiło wczoraj (i przed tygodniem, i przed miesiącem), nie mówi nic o tym co zrobi jutro. Zwroty dzienne to statystycznie szum losowy.

Jedyna rzecz, która *jest* autokorelowana, to **zmienność** — po burzliwym dniu zwykle następuje kolejny burzliwy dzień. Można przewidzieć *jak duży* będzie ruch, ale nie *w którą stronę*.

[WYKRES: plot4_volatility.png]

---

## Wyniki modeli

### Regresja: ile procent zmieni się kurs jutro?

| | R² |
|---|---|
| XGBoost (trening) | 0,013 |
| XGBoost (holdout 2021–2026) | **0,006** |
| Dummy baseline (zawsze przewiduj zero) | −0,001 |

R² holdoutowy wynosi 0,006 — model wyjaśnia **0,6% wariancji** dziennych zwrotów. RMSE to 1,76%, przy czym średnia absolutna dzienna zmiana kursu jest podobna — model jest równoważny przewidywaniu zera każdego dnia.

[WYKRES: plot6_regression_holdout.png]

### Klasyfikacja: góra czy dół?

| | Accuracy |
|---|---|
| XGBoost (trening) | 53,8% |
| XGBoost (holdout 2021–2026) | **53,3%** |
| Dummy baseline (zawsze „wzrost") | 53,0% |

Model klasyfikacyjny osiąga 53,3% trafności. Dummy classifier — który po prostu zawsze mówi "wzrost", bo statystycznie wzrostów jest trochę więcej — osiąga 53,0%. **Przewaga modelu nad losem: 0,3 punktu procentowego.**

[WYKRES: plot7_classification_results.png]

---

## Dlaczego wskaźniki techniczne nie działają?

RSI to funkcja ostatnich 14 zamknięć. MACD to różnica dwóch EMA. Stochastic to znormalizowane minima i maksima z ostatnich N dni. Wszystkie te wskaźniki są **matematycznymi przekształceniami tej samej ceny zamknięcia** — nie wnoszą nowej informacji.

A sama cena, jak pokazuje ACF, nie ma pamięci na poziomie jednodniowym. Nie ma więc czego wyciągać.

---

## Co to oznacza dla day tradingu?

Wyobraźmy sobie optymistyczny scenariusz: nasz model ma 53,3% trafności. Przy typowych kosztach transakcji (prowizja + spread, łącznie ~0,2–0,3% na obrót), żeby wyjść na zero przy codziennym tradingu, potrzebujemy trafności wyraźnie powyżej 50%. Nawet zanim uwzględnimy podatek Belki (19% od zysku), ta 0,3% przewaga nad dummy jest z dużym prawdopodobieństwem przypadkowa — szum statystyczny na 1137 dniach.

Badania empiryczne to potwierdzają: analizy detalicznych day traderów na rynku brazylijskim (Chague et al., 2020) i amerykańskim (Barber & Odean) pokazują, że ok. 70–80% z nich traci pieniądze po kosztach transakcji.

**Ważne zastrzeżenie:** ten wynik dotyczy konkretnej sytuacji — klasycznych wskaźników technicznych, danych dziennych, dużej płynnej spółki jak AAPL. Profesjonalni traderzy HFT operują na danych z rozdzielczością sekund, patrzą na przepływ zleceń (order flow), i mają przewagę technologiczną niedostępną dla detalicznego inwestora. To zupełnie inny świat niż RSI na wykresie dziennym.

---

## Wnioski

- Dzienne zwroty AAPL są statystycznie spójne z błądzeniem losowym (EMH, forma słaba).
- XGBoost wytrenowany na 9 klasycznych wskaźnikach technicznych nie bije trivialnego baseline na 5 latach niewidzianych danych.
- Zmienność jest częściowo przewidywalna — kierunek nie.
- Day trading oparty na wskaźnikach technicznych i danych dziennych to operowanie na szumie, nie na sygnale — koszty transakcji robią resztę.

> Dobry wynik w uczeniu maszynowym nie zawsze znaczy, że problem jest rozwiązany. Czasem znaczy, że nie ma tu żadnego sygnału do wyciągnięcia.

---

*Dane: Yahoo Finance. Kod: Python, XGBoost, TA-Lib. Holdout: sierpień 2021 – marzec 2026. Kod dostępny na GitHubie [link].*
