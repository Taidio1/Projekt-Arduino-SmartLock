# SmartSafe Guardian – inteligentny sejf Arduino

**SmartSafe Guardian** to edukacyjny projekt zrealizowany w ramach przedmiotu **Systemy wbudowane** na uczelni **Uniwersytet Vizja w Warszawie**.

Projekt przedstawia prototyp inteligentnego sejfu / zamka elektronicznego opartego na platformie **Arduino UNO**. System wykorzystuje technologię **RFID** do autoryzacji użytkownika, **serwomechanizm** jako mechaniczny zamek, **buzzer aktywny** oraz **czerwoną diodę LED** jako sygnalizację alarmową. Dodatkowo projekt współpracuje z aplikacją okienkową stworzoną w **Processing**, która pełni funkcję prostego panelu Smart Home.

Celem projektu jest pokazanie, jak połączyć elektronikę, programowanie mikrokontrolera oraz komunikację z aplikacją komputerową w jeden działający system wbudowany.

---

## Spis treści

* [Opis projektu](#opis-projektu)
* [Cel edukacyjny](#cel-edukacyjny)
* [Główne funkcje](#główne-funkcje)
* [Zastosowane technologie](#zastosowane-technologie)
* [Wykorzystane komponenty](#wykorzystane-komponenty)
* [Zasada działania](#zasada-działania)
* [Komunikacja Arduino ↔ Processing](#komunikacja-arduino--processing)
* [Struktura repozytorium](#struktura-repozytorium)
* [Uruchomienie projektu](#uruchomienie-projektu)
* [Autor](#autor)

---

## Opis projektu

SmartSafe Guardian to prototyp inteligentnego sejfu, który można otworzyć tylko za pomocą autoryzowanej karty RFID lub breloka RFID. Po przyłożeniu poprawnej karty Arduino rozpoznaje użytkownika, odblokowuje mechanizm zamka za pomocą serwomechanizmu i wysyła informację o zdarzeniu do aplikacji Processing.

W przypadku użycia nieznanej karty system odmawia dostępu i zwiększa licznik błędnych prób. Po kilku nieudanych próbach może zostać uruchomiony alarm fizyczny, czyli buzzer oraz czerwona dioda LED.

Projekt pokazuje praktyczne zastosowanie systemów wbudowanych w kontekście prostego systemu kontroli dostępu.

---

## Cel edukacyjny

Projekt został przygotowany jako praca edukacyjna na przedmiot **Systemy wbudowane**.

Główne cele edukacyjne projektu:

* poznanie podstaw pracy z platformą Arduino,
* obsługa modułu RFID MFRC522,
* sterowanie serwomechanizmem,
* obsługa sygnalizacji alarmowej za pomocą buzzera i diody LED,
* komunikacja Arduino z aplikacją komputerową przez port szeregowy,
* stworzenie prostego panelu Smart Home w Processing,
* organizacja kodu w projekcie embedded,
* dokumentacja projektu technicznego na GitHubie.

Projekt może być również przydatny dla osób uczących się elektroniki, programowania mikrokontrolerów oraz podstaw automatyki domowej.

---

## Główne funkcje

Projekt obsługuje następujące funkcjonalności:

* odczyt karty lub breloka RFID,
* sprawdzanie, czy UID karty jest autoryzowany,
* otwieranie zamka po poprawnej autoryzacji,
* automatyczne zamykanie zamka po określonym czasie,
* odmowa dostępu dla nieznanej karty,
* licznik błędnych prób,
* uruchomienie alarmu po kilku nieudanych próbach,
* sygnalizacja alarmowa za pomocą czerwonej diody LED,
* sygnalizacja dźwiękowa za pomocą buzzera aktywnego,
* komunikacja z aplikacją Processing przez USB,
* wyświetlanie statusu sejfu w aplikacji okienkowej,
* logowanie zdarzeń w panelu aplikacji,
* możliwość ręcznego sterowania zamkiem z poziomu aplikacji.

---

## Zastosowane technologie

W projekcie wykorzystano:

* **Arduino IDE** – programowanie mikrokontrolera,
* **Arduino UNO R3** – główna płytka sterująca,
* **C / C++ dla Arduino** – logika urządzenia,
* **Processing** – aplikacja okienkowa / panel Smart Home,
* **Serial Communication** – komunikacja Arduino z komputerem,
* **RFID MFRC522** – autoryzacja dostępu,
* **Servo library** – sterowanie serwomechanizmem,
* **SPI** – komunikacja z modułem RFID.

---

## Wykorzystane komponenty

Do wykonania projektu wykorzystano następujące elementy:

| Komponent                       | Rola w projekcie                                 |
| ------------------------------- | ------------------------------------------------ |
| Arduino UNO R3                  | Główna jednostka sterująca                       |
| Moduł RFID MFRC522              | Odczyt karty lub breloka RFID                    |
| Karta RFID / brelok RFID        | Klucz dostępu do sejfu                           |
| Serwomechanizm SG90             | Mechaniczne otwieranie i zamykanie zamka         |
| Buzzer aktywny                  | Alarm dźwiękowy                                  |
| Czerwona dioda LED              | Sygnalizacja alarmu                              |
| Rezystor do diody LED           | Ograniczenie prądu diody                         |
| Płytka stykowa                  | Łączenie elementów bez lutowania                 |
| Przewody połączeniowe           | Połączenie modułów z Arduino                     |
| Przewód USB                     | Programowanie Arduino i komunikacja z komputerem |
| Komputer z aplikacją Processing | Panel Smart Home i podgląd zdarzeń               |

---

## Zasada działania

Po uruchomieniu system przechodzi w stan zamknięty. Arduino oczekuje na przyłożenie karty RFID do czytnika.

Jeżeli użytkownik przyłoży autoryzowaną kartę:

1. Arduino odczytuje UID karty.
2. UID jest porównywany z zapisanym identyfikatorem.
3. System uznaje dostęp za poprawny.
4. Serwomechanizm obraca się do pozycji otwartej.
5. Do aplikacji Processing wysyłany jest komunikat o poprawnym otwarciu.
6. Po kilku sekundach sejf zostaje automatycznie zamknięty.

Jeżeli użytkownik przyłoży nieznaną kartę:

1. Arduino odczytuje UID karty.
2. UID nie pasuje do zapisanej karty.
3. System odmawia dostępu.
4. Licznik błędnych prób zostaje zwiększony.
5. Aplikacja Processing zapisuje zdarzenie w logach.
6. Po przekroczeniu limitu prób uruchamia się alarm.

Alarm fizyczny składa się z:

* czerwonej diody LED,
* buzzera aktywnego.

---

## Komunikacja Arduino ↔ Processing

Arduino komunikuje się z aplikacją Processing przez port szeregowy USB.

Arduino wysyła do aplikacji informacje o stanie systemu, na przykład:

* poprawny dostęp,
* odmowa dostępu,
* otwarcie zamka,
* zamknięcie zamka,
* alarm,
* liczba błędnych prób,
* UID przyłożonej karty.

Aplikacja Processing pełni rolę panelu Smart Home. Dzięki niej można:

* obserwować aktualny stan sejfu,
* odczytywać logi zdarzeń,
* sprawdzać, czy alarm jest aktywny,
* sterować zamkiem ręcznie z poziomu komputera,
* prezentować działanie projektu w bardziej czytelny sposób.

Ważne: w aplikacji Processing należy ustawić poprawny port COM, do którego podłączone jest Arduino.

Przykład:

```java
myPort = new Serial(this, "COM6", 9600);
```

Jeżeli Arduino jest podłączone do innego portu, należy zmienić `"COM6"` na właściwy numer portu.

---

## Struktura repozytorium

Przykładowa struktura projektu:

```text
SmartSafe-Guardian/
│
├── README.md
│
├── arduino/
│   └── SmartSafeGuardian.ino
│
├── processing/
│   └── SmartSafeGuardianDashboard.pde
│
├── docs/
│   ├── components.md
│   └── project-description.md
│
├── images/
│   ├── circuit.jpg
│   ├── components.jpg
│   └── dashboard.png
```

Opis folderów:

| Folder / plik | Opis                                                 |
| ------------- | ---------------------------------------------------- |
| `README.md`   | Główna dokumentacja projektu                         |
| `arduino/`    | Kod dla Arduino UNO                                  |
| `processing/` | Kod aplikacji Processing                             |
| `docs/`       | Dodatkowa dokumentacja techniczna                    |
| `images/`     | Zdjęcia układu i aplikacji                           |

---

## Uruchomienie projektu

### 1. Podłączenie elementów

Najpierw należy połączyć elementy elektroniczne zgodnie ze schematem projektu:

* Arduino UNO,
* moduł RFID MFRC522,
* serwomechanizm,
* buzzer aktywny,
* czerwona dioda LED z rezystorem,
* wspólna masa GND dla wszystkich elementów.

Wszystkie elementy powinny mieć wspólną masę. Jest to bardzo ważne, ponieważ bez wspólnego GND moduły mogą działać niestabilnie lub w ogóle nie reagować.

---

### 2. Wgranie kodu Arduino

Aby uruchomić część Arduino:

1. Otwórz Arduino IDE.
2. Podłącz Arduino UNO do komputera przez USB.
3. Wybierz odpowiednią płytkę:
   `Tools -> Board -> Arduino UNO`
4. Wybierz odpowiedni port COM.
5. Otwórz plik `SmartSafeGuardian.ino`.
6. Zainstaluj wymagane biblioteki.
7. Wgraj program na Arduino.

W projekcie mogą być potrzebne biblioteki:

* `SPI.h`,
* `MFRC522.h`,
* `Servo.h`.

---

### 3. Sprawdzenie UID karty RFID

Każda karta RFID ma własny identyfikator UID. Aby karta mogła otwierać sejf, jej UID musi zostać zapisany w kodzie Arduino jako autoryzowany identyfikator.

Typowy proces:

1. Otwórz monitor portu szeregowego w Arduino IDE.
2. Przyłóż kartę RFID do czytnika.
3. Odczytaj UID karty.
4. Wklej UID do tablicy lub zmiennej z autoryzowanymi kartami.
5. Wgraj kod ponownie.

---

### 4. Uruchomienie aplikacji Processing

Aby uruchomić panel Smart Home:

1. Otwórz Processing.
2. Otwórz plik `.pde` z folderu `processing/`.
3. Sprawdź, czy port COM w kodzie zgadza się z portem Arduino.
4. Uruchom aplikację.
5. Obserwuj status sejfu i logi zdarzeń.

---

## Możliwe rozszerzenia

Projekt można rozbudować o dodatkowe funkcje, na przykład:

* dodawanie nowych kart RFID z poziomu aplikacji,
* zapis historii zdarzeń do pliku,
* ekran logowania administratora,
* czujnik przechyłu wykrywający potrząsanie sejfem,
* czujnik światła wykrywający otwarcie obudowy,
* czujnik temperatury i wilgotności,
* moduł czasu rzeczywistego RTC,
* zasilanie bateryjne,
* obudowę wydrukowaną w 3D,
* integrację z aplikacją webową,
* komunikację przez Wi-Fi z użyciem ESP8266 lub ESP32.

---
## Wnioski

SmartSafe Guardian pokazuje, jak z prostych komponentów Arduino można stworzyć działający system wbudowany. Projekt łączy kilka ważnych zagadnień:

* elektronikę,
* programowanie mikrokontrolera,
* komunikację szeregową,
* mechaniczne sterowanie zamkiem,
* autoryzację RFID,
* alarm fizyczny,
* aplikację komputerową jako panel użytkownika.

Dzięki temu projekt dobrze pokazuje praktyczne zastosowanie systemów wbudowanych w prostym scenariuszu Smart Home.

---

## Autor
Kacper Łacniak
Projekt wykonany na potrzeby przedmiotu **Systemy wbudowane**.

**Uczelnia:** Uniwersytet Vizja w Warszawie
**Projekt:** SmartSafe Guardian
**Technologie:** Arduino, RFID, Servo, Processing
**Charakter projektu:** edukacyjny prototyp systemu wbudowanego

---

## Licencja
Projekt może zostać udostępniony jako projekt edukacyjny.
