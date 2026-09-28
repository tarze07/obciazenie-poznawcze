# Symulator obciążenia poznawczego: zespół vs mały zespół + LLM

Interaktywny kalkulator porównujący obciążenie poznawcze **jednej osoby** w zespole pierwotnym (domyślnie 8 osób) z obciążeniem jednej osoby w nowym modelu (domyślnie 2 osoby pracujące z LLM-ami).

## Uruchomienie

Wystarczy otworzyć `index.html` w przeglądarce. Brak zależności i kroku budowania. Plik nadaje się też do GitHub Pages / GitLab Pages.

## Model

Obciążenie jednej osoby:

```
L = (1 − α + α·β) · C/n  +  γ·(n − 1)  +  δ·m  +  κ·log₂(C/n)
```

| Symbol | Znaczenie |
|---|---|
| C | całkowita złożoność projektu (decyzje, zadania, informacje) |
| n | liczba osób |
| α | część pracy wykonawczej przejęta przez LLM-y (0–1) |
| β | koszt weryfikacji wyniku LLM względem zrobienia tego samemu (0–1) |
| γ | koszt koordynacji z jedną osobą |
| δ·m | koszt sterowania agentami: prompty, kontekst, dzielenie zadań, restarty (bez weryfikacji, która jest w β) |
| κ | koszt szerokości kontekstu, jaki trzeba ogarnąć |

W wersji pierwotnej α = 0 i δ·m = 0.

Stosunek obciążeń:

```
R = L_nowy / L_stary
```

R > 1 oznacza, że każda osoba w nowym modelu ma większe obciążenie.

Próg opłacalności (R = 1):

```
α* = (C/n_nowy + γ·(n_nowy − 1) + δ·m + κ·log₂(C/n_nowy) − L_stary) / ((C/n_nowy)·(1 − β))
```

Przy pominięciu kosztów stałych i przejściu z 8 na 2 osoby warunek sprowadza się do `α(1 − β) ≥ 0,75`.

## Kalibracja δ·m

δ·m jest wyrażone w tych samych jednostkach co C. Najprościej oszacować je przez udział czasu: jeśli osoba o łącznym obciążeniu L ≈ 30 spędza ok. 20% czasu na sterowaniu agentami, to δ·m ≈ 6.

Orientacyjnie: 0–3 jeden asystent, 4–10 kilka agentów z kontekstem, 10–20 orkiestracja wielu agentów, 20–30 intensywna praca „dyspozytorska”.

## Czynnik stresu

Stres liczony jest według modelu wymagania–kontrola (Job Demand-Control) Karaska (1979), najlepiej potwierdzonego empirycznie modelu stresu w pracy. Napięcie to iloraz wymagań i kontroli:

```
S = (L / L_max) / c
```

| Symbol | Znaczenie |
|---|---|
| L_max | pojemność poznawcza: obciążenie, przy którym wymagania są pełne (D = 1) |
| c | kontrola (swoboda decyzji), 0,1–1, osobno dla zespołu pierwotnego i nowego modelu |

Stosunek stresu: `R_S = S_nowy / S_stary = R · c_stary / c_nowy`. Próg S = 1 (wymagania równe kontroli) to umowna granica przyjęta w symulatorze; w badaniach wysokie napięcie wyznacza się zwykle względem mediany.

## Ograniczenia

- δ·m jest stałą niezależną od C; w praktyce rośnie z wielkością projektu.
- Model nie rozróżnia rodzajów obciążenia (wykonawcze vs decyzyjne), a praca decyzyjna męczy szybciej.
- Parametry są szacunkami, nie pomiarami: wynik pokazuje zależności, nie dokładne wartości.
- Stres rośnie liniowo z obciążeniem; model nie uwzględnia wsparcia społecznego ani zmęczenia narastającego w czasie.
