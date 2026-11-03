# aMAZEing
projekt na studia na przedmiot Sztuczna Inteligencja w Grach Komputerowych
# Warsztat 2 — Agent uczący się przejścia labiryntu

## 1. Temat i autorzy

**Temat:** Agent uczący się przejścia labiryntu — tablicowy Q-learning i porównanie wyuczonej strategii z agentem losowym  
**Data:** 04.11.2026r.  
**Numer zespołu:** _[uzupełnijcie]_  
**Członkowie zespołu (imiona i nazwiska):** _[uzupełnijcie]_  
**Opiekun z zespołu liderskiego (imię i nazwisko):** _[uzupełnijcie]_

## 2. Cel ćwiczenia

**Cel:** Nauczyć agenta docierania do celu na planszy 5 × 5 metodą tablicowego Q-learningu. Porównać zamrożoną strategię wyuczoną z agentem losowym na jednakowym zestawie 20 pozycji startowych. Sprawdzić wpływ wartości ε podczas treningu oraz odnieść długości uzyskanych ścieżek do najkrótszych tras wyznaczonych przez BFS.

## 3. Opis środowiska

### 3.1. Plansza i reprezentacja stanu

**Reprezentacja labiryntu:** Labirynt ma rozmiary 5x5 pól. Każde pole opisane jest krotką `(wiersz,kolumna)`. Numeracja wierszy rośnie w dół, a kolumn w prawo. Celem jest pole (4, 4), a przeszkody znajdują się na polach (0, 1), (1, 1), (3, 1) i (3, 3).

_[uzupełnijcie(zdjęcie_labiryntu)]_

**Reprezentacja stanu:** krotka `(wiersz, kolumna)`, z indeksowaniem od zera.  
**Reprezentacja akcji:** liczba 0–3 (góra, prawo, dół, lewo).  
**Tablica Q:** `np.ndarray` o wymiarach `(5, 5, 4)`; `Q[wiersz, kolumna, akcja]` przechowuje oszacowaną wartość akcji w danym stanie.

### 3.2. Funkcja nagrody

| Zdarzenie                      | Nagroda | Efekt                   |
| ------------------------------ | ------: | ----------------------- |
| Dotarcie do celu               |   `+10` | Koniec epizodu — sukces |
| Dozwolony ruch na zwykłe pole  |  `−0,1` | Zmiana pozycji          |
| Ruch w ścianę lub poza planszę |    `−1` | Pozycja bez zmian       |

### 3.3. Konfiguracja

| Parametr                                | Użyta wartość                     |
| --------------------------------------- | --------------------------------- |
| Liczba epizodów treningowych na wariant | _[uzupełnijcie (domyślnie 300)]_  |
| Współczynnik uczenia (`alpha`)          | _[uzupełnijcie (domyślnie 0,1)]_  |
| Współczynnik dyskontowania (`gamma`)    | _[uzupełnijcie (domyślnie 0,95)]_ |
| Epsilon                                 | _[uzupełnijcie (domyślnie 0,2)]_  |
| Maksymalna liczba kroków w epizodzie    | _[uzupełnijcie (domyślnie 50)]_   |
| `start_seed`                            | _[uzupełnijcie (domyślnie 2026)]_ |
| `action_seed`                           | _[uzupełnijcie (domyślnie 2027)]_ |
| `eval_seed`                             | _[uzupełnijcie (domyślnie 2028)]_ |

### 3.4. Test agenta losowego

**Pole startowe:** _[uzupelnijcie]_  
**Czy sukces:** _[uzupelnijcie]_  
**Liczba kroków:** _[uzupelnijcie]_  
**Nagroda:** _[uzupelnijcie]_

**Znaleziona trasa:** _[uzupelnijcie(zdjecie)]_

## 4. Aktualizacja Q i trening

**Starty testowe:** 20 wolnych pól poza celem, każde dokładnie raz.

### 4.1. Wybór akcji i aktualizacja wartości Q

**Zasada ε-zachłanna:** z prawdopodobieństwem ε wybierana jest losowa akcja spośród czterech, w przeciwnym przypadku jedna z akcji o maksymalnej wartości Q. Remisy są rozstrzygane losowo, także podczas oceny z ε = 0.

**Aktualizacja:**

$$
Q(s,a) \leftarrow Q(s,a) + \alpha\left[r + \gamma V(s') - Q(s,a)\right],
$$

gdzie:

- `α` — współczynnik uczenia,
- `γ` — współczynnik dyskontowania przyszłych nagród,
- `r` — nagroda za ostatnią akcję,
- `V(s') = 0` po osiągnięciu celu, a w pozostałych przypadkach `V(s') = max Q(s', a')`.

### 4.2. Trening agenta

Trening agenta polega na wielokrotnym przechodzeniu przez środowisko i aktualizowaniu wartości Q na podstawie otrzymywanych nagród. W każdym kroku agent wybiera akcję zgodnie z polityką ε-zachłanną, wykonuje ją, a następnie aktualizuje tablicę Q według wcześniej przedstawionego wzoru.

#### 4.2.1 Wyniki treningu

_[uzupelnijcie(wykres)]_

#### 4.2.2 Analiza

**Jak zmieniała się suma nagród w kolejnych epizodach:** _[uzupelnijcie]_  
**Dlaczego agent podczas treningu może wybrać gorsze pole:** _[uzupelnijcie]_

## 5. Ocena gotowej strategii

Dla wszystkich 20 pozycji startowych przetestowano obu agentów, a następnie wyznaczono metryki:

- sukcesy - liczba prób zakończonych dotarciem do celu
- porażki - liczba prób w których agent nie dotarł do celu
- skuteczność - udział sukcesów wśród wszystkich prób wyrażony w procentach
- średnia kroków - podaje średnią liczbę kroków w próbach zakończonych dotarciem do celu

**Zmiany parametrów na czas testów:** _[uzupelnijcie]_

### 5.1. Wyniki

| Agent / strategia |  Sukcesy / próby |      Skuteczność |    Niepowodzenia | Śr. liczba kroków w sukcesach |
| ----------------- | ---------------: | ---------------: | ---------------: | ----------------------------: |
| Losowy            | _[uzupelnijcie]_ | _[uzupelnijcie]_ | _[uzupelnijcie]_ |              _[uzupelnijcie]_ |
| Q-learning        | _[uzupelnijcie]_ | _[uzupelnijcie]_ | _[uzupelnijcie]_ |              _[uzupelnijcie]_ |

**Przykładowa ścieżka agenta losowego:**_[uzupelnijcie(zdjęcie)]_

**Przykładowa ścieżka wyuczonego agenta (Q-learning):**_[uzupelnijcie(zdjęcie)]_

### 5.2. Analiza wyników

**Porównanie wyników:** _[uzupelnijcie]_

**Dlaczego należało wyłączyć ekspolrację i naukę na czas testów:** _[uzupelnijcie]_

**Dlaczego uzyskane wyniki odnoszą się do używanej planszy i nie wystarczają do oceny działania agenta w dowolnym labiryncie:** _[uzupelnijcie]_

## 6. Zakres dodatkowy: Porównanie epsilon 0,05 i 0,3.

_[Dodatkowe rozszerzenie: uzupełnijcie lub usuńcie CAŁĄ sekcję]_  
W celu porównania wpływu parametru epsilon, wyuczono dwóch niezależnych agentów w identycznych środowiskach. Jedyną różnicą między agentami była wartość epsilon podczas uczenia. Wykorzystano wartości 0,05 i 0,3. Następnie zapisano i porównano proces uczenia.

### 6.1. Wyniki

**Wykres zmian nagrody dla epsilon = 0,05:** _[uzupelnijcie(wykres)]_

**Wykres zmian nagrody dla epsilon = 0,3:** _[uzupelnijcie(wykres)]_

**Skuteczność obu agentów:**
| Epsilon treningowe | Sukcesy / próby | Skuteczność | Śr. liczba kroków w sukcesach |
| ----------------- | ---------------: | ---------------: | ----------------------------: |
| 0,05 | _[uzupelnijcie]_ | _[uzupelnijcie]_ | _[uzupelnijcie]_ |
| 0,3 | _[uzupelnijcie]_ | _[uzupelnijcie]_ | _[uzupelnijcie]_ |

### 6.2. Analiza

**Różnice między agentami podczas treningu:** _[uzupelnijcie]_

**Czy otrzymane wyniki są wystarczające by wyłonić najlepszą wartość epsilon:** _[uzupelnijcie]_

## 7.Podsumowanie i wnioski

**Czy wyuczony agent był lepszy i jak duża była różnica:** _[uzupełnijcie]_

**Stan rozwiązania na koniec 90 minut:** _[uzupełnijcie]_.

**Wkład członków zespołu:**

| Imię i nazwisko            | Wykonane zadania podczas zajęć |
| -------------------------- | ------------------------------ |
| _[osoba 1]_                | _[uzupełnijcie]_               |
| _[osoba 2]_                | _[uzupełnijcie]_               |
| _[osoba 3]_                | _[uzupełnijcie]_               |
| _[osoba 4, jeśli dotyczy]_ | _[uzupełnijcie]_               |
| _[osoba 5, jeśli dotyczy]_ | _[uzupełnijcie]_               |
