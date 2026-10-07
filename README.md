# 🚦 Sterowanie sygnalizacją świetlną – Arduino

 ## 📌 Opis projektu

 Program steruje **4 zestawami świateł**, z których każdy składa się z jednej diody zielonej i jednej czerwonej.

 Program działa cyklicznie:

 - wszystkie światła ustawiane są na czerwono,
- następnie dla jednego wybranego przejazdu zapalana jest dioda zielona,
- jego dioda czerwona zostaje wyłączona,
- po **3 sekundach** zielone światło przełącza się na kolejny przejazd.

 Dzięki temu w danym momencie świeci:

 - 🟢 **1 dioda zielona**
- 🔴 **3 diody czerwone**

 Cykl powtarza się bez końca.

---

 ## 🔌 Podłączenie

 ### Diody zielone

 | Przejazd | Pin Arduino |
| --- | --- |
| 1 | 8 |
| 2 | 9 |
| 3 | 10 |
| 4 | 11 |

### Diody czerwone

 | Przejazd | Pin Arduino |
| --- | --- |
| 1 | 0 |
| 2 | 1 |
| 3 | 2 |
| 4 | 3 |

W programie piny zostały zapisane w tablicach:

```
const int ZIELONE[4] = {8, 9, 10, 11};
const int CZERWONE[4] = {0, 1, 2, 3};
```

 > ⚠️ **Uwaga:** Na wielu płytkach Arduino, np. Arduino Uno, piny **0 i 1** są wykorzystywane przez komunikację szeregową (RX/TX). Używanie ich do sterowania diodami może powodować problemy podczas komunikacji z komputerem. W razie potrzeby warto przenieść czerwone diody na inne piny.

---

 ## ⚙️ Jak działa program?

 ### 1\. Konfiguracja

 W funkcji `setup()` wszystkie używane piny zostają ustawione jako wyjścia:

```
for (int i = 0; i < 4; i++) {
  pinMode(ZIELONE[i], OUTPUT);
  pinMode(CZERWONE[i], OUTPUT);
}
```

---

 ### 2\. Główna pętla

 Funkcja `loop()` przechodzi przez wszystkie cztery przejazdy:

```
for (int i = 0; i < 4; i++) {
```

 Zmiennej `i` przypisywane są kolejno wartości:

```
0 → 1 → 2 → 3
```

 Odpowiada to kolejnym zestawom świateł.

---

 ### 3\. Wyłączenie wszystkich zielonych

 Na początku każdej zmiany wszystkie zielone diody są gaszone:

```
digitalWrite(ZIELONE[0], LOW);
digitalWrite(ZIELONE[1], LOW);
digitalWrite(ZIELONE[2], LOW);
digitalWrite(ZIELONE[3], LOW);
```

---

 ### 4\. Włączenie wszystkich czerwonych

 Następnie wszystkie czerwone diody zostają zapalone:

```
digitalWrite(CZERWONE[0], HIGH);
digitalWrite(CZERWONE[1], HIGH);
digitalWrite(CZERWONE[2], HIGH);
digitalWrite(CZERWONE[3], HIGH);
```

 W tym momencie wszystkie cztery przejazdy mają czerwone światło.

---

 ### 5\. Wybór przejazdu

 Dla aktualnie wybranego przejazdu:

```
digitalWrite(ZIELONE[i], HIGH);
digitalWrite(CZERWONE[i], LOW);
```

 zapala się zielona dioda, a czerwona zostaje wyłączona.

 Przykładowo dla:

```
i = 0;
```

 otrzymujemy:

```
Przejazd 1 → 🟢
Przejazd 2 → 🔴
Przejazd 3 → 🔴
Przejazd 4 → 🔴
```

 Po 3 sekundach program przechodzi do następnego przejazdu.

---

 ## ⏱️ Czas działania

 Czas świecenia zielonego światła określa:

```
delay(3000);
```

 `3000` oznacza **3000 milisekund**, czyli:

 **3 sekundy.**

 Każdy przejazd otrzymuje zielone światło przez 3 sekundy.

 Pełny cykl:

```
Przejazd 1 → 3 s
Przejazd 2 → 3 s
Przejazd 3 → 3 s
Przejazd 4 → 3 s
```

 Łącznie jeden cykl trwa około **12 sekund**.

---

 ## 🔄 Schemat działania

```
START
  │
  ▼
Wszystkie czerwone 🔴🔴🔴🔴
  │
  ▼
Przejazd 1: 🟢🔴🔴🔴
  │
  │ 3 sekundy
  ▼
Przejazd 2: 🔴🟢🔴🔴
  │
  │ 3 sekundy
  ▼
Przejazd 3: 🔴🔴🟢🔴
  │
  │ 3 sekundy
  ▼
Przejazd 4: 🔴🔴🔴🟢
  │
  │ 3 sekundy
  ▼
Powrót do przejazdu 1
  │
  └───────────────↺
```

---

 ## 🛠️ Wymagane elementy

 Do wykonania projektu potrzebne są:

 - Arduino,
- 4 × dioda zielona,
- 4 × dioda czerwona,
- 8 × rezystor ograniczający prąd, np. **220 Ω**,
- płytka stykowa,
- przewody połączeniowe,
- kabel USB do programowania Arduino.

---

 ## ▶️ Uruchomienie

 1. Podłącz diody do odpowiednich pinów Arduino.
2. Każdą diodę podłącz przez rezystor ograniczający prąd.
3. Podłącz Arduino do komputera.
4. Otwórz kod w **Arduino IDE**.
5. Wybierz odpowiednią płytkę oraz port COM.
6. Wgraj program na Arduino.
7. Po uruchomieniu pierwszy przejazd otrzyma zielone światło.
8. Co 3 sekundy zielone światło zostanie przekazane kolejnemu przejazdowi.

---

 ## 🎯 Cel projektu

 Celem projektu jest przedstawienie podstawowego sterowania sygnalizacją świetlną za pomocą Arduino. Program wykorzystuje:

 - tablice,
- pętle `for`,
- funkcję `digitalWrite()`,
- funkcję `pinMode()`,
- funkcję `delay()`,
- sterowanie wyjściami cyfrowymi.

 Projekt może być podstawą do stworzenia bardziej rozbudowanej sygnalizacji, np. z przyciskami dla pieszych, światłem żółtym lub czujnikami ruchu.

---

 ## 📄 Licencja

 Projekt może być wykorzystywany i modyfikowany do celów edukacyjnych.
