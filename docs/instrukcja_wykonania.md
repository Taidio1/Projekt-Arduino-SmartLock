# Instrukcja wykonania projektu Arduino – SmartLock / SmartSafe Guardian

## 1. Cel projektu

Celem projektu jest wykonanie prototypu inteligentnego zamka / sejfu **SmartLock / SmartSafe Guardian**, który pozwala na otwieranie zamka za pomocą autoryzowanej karty RFID lub breloka RFID.

Projekt łączy:

- Arduino UNO,
- czytnik RFID MFRC522,
- serwomechanizm pełniący rolę zamka,
- czerwoną diodę LED jako sygnalizację alarmową,
- aktywny buzzer jako alarm dźwiękowy,
- aplikację komputerową w Processing jako panel Smart Home.

System działa w taki sposób, że po przyłożeniu poprawnej karty RFID zamek otwiera się na kilka sekund, a następnie automatycznie zamyka. Po przyłożeniu nieautoryzowanej karty system zlicza błędne próby. Po kilku błędnych próbach uruchamia alarm: czerwoną diodę LED oraz buzzer.

---

## 2. Wymagane elementy

Do wykonania projektu potrzebne są:

| Element | Ilość | Opis |
|---|---:|---|
| Arduino UNO R3 | 1 | Główna płytka sterująca projektem |
| Czytnik RFID MFRC522 | 1 | Moduł do odczytu kart i breloków RFID |
| Karta RFID / brelok RFID | min. 1 | Nośnik identyfikatora użytkownika |
| Serwomechanizm | 1 | Element symulujący mechanizm zamka |
| Czerwona dioda LED | 1 | Sygnalizacja alarmu |
| Rezystor 220 Ω / 330 Ω | 1 | Ograniczenie prądu dla diody LED |
| Aktywny buzzer | 1 | Alarm dźwiękowy |
| Płytka stykowa | 1 | Wygodne połączenie elementów |
| Przewody połączeniowe | kilka | Do połączenia modułów z Arduino |
| Kabel USB do Arduino | 1 | Zasilanie i komunikacja z komputerem |
| Komputer z Arduino IDE | 1 | Wgrywanie programu do Arduino |
| Komputer z Processing | 1 | Uruchomienie panelu Smart Home |

---

## 3. Schemat logiczny działania

1. Arduino uruchamia czytnik RFID, serwomechanizm, LED i buzzer.
2. Serwo ustawia zamek w pozycji zamkniętej.
3. System czeka na przyłożenie karty lub breloka RFID.
4. Po odczytaniu UID karty Arduino sprawdza, czy UID znajduje się na liście autoryzowanych kart.
5. Jeżeli karta jest poprawna:
   - system otwiera zamek,
   - wysyła informację do aplikacji Processing,
   - po kilku sekundach automatycznie zamyka zamek.
6. Jeżeli karta jest błędna:
   - system odrzuca dostęp,
   - zwiększa licznik błędnych prób,
   - wysyła komunikat do aplikacji Processing.
7. Po przekroczeniu limitu błędnych prób:
   - uruchamia się alarm,
   - zapala się czerwona dioda LED,
   - aktywuje się buzzer,
   - aplikacja Processing pokazuje stan alarmowy.

---

## 4. Proponowane podłączenie elementów

> Ważne: poniższe piny muszą być zgodne z definicjami w kodzie Arduino.  
> Jeżeli w kodzie masz inne wartości, podłącz elementy zgodnie z kodem albo zmień definicje pinów w programie.

### 4.1. Czytnik RFID MFRC522

| Pin MFRC522 | Pin Arduino UNO |
|---|---|
| SDA / SS | D10 |
| SCK | D13 |
| MOSI | D11 |
| MISO | D12 |
| RST | D9 |
| 3.3V | 3.3V |
| GND | GND |

**Uwaga:** moduł MFRC522 powinien być zasilany z **3.3V**, nie z 5V.

---

### 4.2. Serwomechanizm

| Przewód serwa | Pin Arduino |
|---|---|
| Sygnał | D6 |
| VCC | 5V |
| GND | GND |

Jeżeli serwo zachowuje się niestabilnie, można użyć osobnego zasilania 5V dla serwa. Wtedy masa zasilacza serwa musi być połączona z masą Arduino.

---

### 4.3. Czerwona dioda LED

| Element | Pin Arduino |
|---|---|
| Dłuższa nóżka LED, czyli anoda | D4 przez rezystor 220 Ω / 330 Ω |
| Krótsza nóżka LED, czyli katoda | GND |

Dioda LED służy wyłącznie do sygnalizacji alarmu.

---

### 4.4. Aktywny buzzer

| Pin buzzera | Pin Arduino |
|---|---|
| Plus / S | D5 |
| Minus / GND | GND |

Aktywny buzzer wydaje dźwięk po podaniu stanu wysokiego `HIGH`.

---

## 5. Rozszerzenie masy GND za pomocą płytki stykowej

Arduino UNO ma ograniczoną liczbę pinów GND, dlatego warto rozprowadzić masę przez płytkę stykową.

1. Podłącz jeden pin **GND** z Arduino do niebieskiej szyny `-` na płytce stykowej.
2. Wszystkie elementy wymagające GND podłączaj do tej samej szyny `-`.
3. Do tej szyny podłącz:
   - GND czytnika RFID,
   - GND serwa,
   - GND buzzera,
   - katodę czerwonej diody LED.

Dzięki temu wszystkie elementy mają wspólną masę, co jest wymagane do poprawnej pracy układu.

---

## 6. Przygotowanie środowiska Arduino IDE

### 6.1. Instalacja Arduino IDE

1. Pobierz i zainstaluj Arduino IDE.
2. Podłącz Arduino UNO do komputera przez USB.
3. W Arduino IDE wybierz:
   - **Tools / Narzędzia → Board / Płytka → Arduino Uno**
   - **Tools / Narzędzia → Port → odpowiedni port COM**

Przykład: jeżeli Arduino jest podłączone jako `COM6`, wybierz `COM6`.

---

### 6.2. Instalacja wymaganych bibliotek

W Arduino IDE wejdź w:

`Sketch → Include Library → Manage Libraries`

Następnie zainstaluj:

- `MFRC522` – biblioteka do czytnika RFID,
- `Servo` – biblioteka do obsługi serwomechanizmu, zwykle dostępna domyślnie,
- `SPI` – biblioteka komunikacji SPI, dostępna domyślnie.

---

## 7. Sprawdzenie UID karty RFID

Przed dodaniem karty do systemu trzeba znać jej UID.

1. Wgraj program testowy do odczytu UID lub użyj swojego aktualnego kodu, który wypisuje UID w Serial Monitorze.
2. Otwórz **Serial Monitor** w Arduino IDE.
3. Ustaw prędkość zgodną z kodem, na przykład `9600` lub `115200`.
4. Przyłóż kartę lub brelok do czytnika RFID.
5. Odczytaj UID z monitora portu szeregowego.
6. Skopiuj UID do listy autoryzowanych kart w kodzie Arduino.

Przykładowy UID może wyglądać tak:

```txt
A1 B2 C3 D4
```

W kodzie Arduino musi być zapisany dokładnie w takim formacie, jaki obsługuje program.

---

## 8. Konfiguracja kodu Arduino

W kodzie Arduino należy sprawdzić najważniejsze ustawienia.

### 8.1. Definicje pinów

Przykładowe definicje mogą wyglądać tak:

```cpp
#define SS_PIN 10
#define RST_PIN 9

const int SERVO_PIN = 6;
const int LED_PIN = 4;
const int BUZZER_PIN = 5;
```

Jeżeli w Twoim kodzie nazwy są inne, najważniejsze jest to, aby numery pinów zgadzały się z fizycznym podłączeniem.

---

### 8.2. Kąty serwomechanizmu

W kodzie powinny znajdować się wartości odpowiadające pozycji zamkniętej i otwartej.

Przykład:

```cpp
const int LOCKED_ANGLE = 0;
const int UNLOCKED_ANGLE = 90;
```

Jeżeli zamek otwiera się w złą stronę, nie zmieniaj mechanicznie całego układu od razu. Najpierw zmień wartość kąta otwarcia.

Przykłady:

```cpp
const int LOCKED_ANGLE = 0;
const int UNLOCKED_ANGLE = 90;
```

albo:

```cpp
const int LOCKED_ANGLE = 0;
const int UNLOCKED_ANGLE = 180;
```

Wybierz tę wartość, przy której zamek otwiera się w poprawną stronę.

---

### 8.3. Lista autoryzowanych kart

W kodzie powinna znajdować się lista poprawnych UID.

Przykładowo:

```cpp
String authorizedCards[] = {
  "A1B2C3D4",
  "11223344"
};
```

Dodaj UID swojej karty i breloka do tej listy.

---

### 8.4. Komunikacja z Processing

Arduino powinno wysyłać komunikaty przez port szeregowy, na przykład:

```txt
STATUS:LOCKED
STATUS:UNLOCKED
ACCESS:GRANTED
ACCESS:DENIED
ALARM:ON
ALARM:OFF
LOG:Przylozono karte RFID
```

Aplikacja Processing odczytuje te komunikaty i na ich podstawie aktualizuje panel Smart Home.

---

## 9. Wgranie kodu do Arduino

1. Otwórz kod Arduino w Arduino IDE.
2. Sprawdź, czy wybrana jest płytka **Arduino UNO**.
3. Sprawdź, czy wybrany jest poprawny port, na przykład `COM6`.
4. Kliknij **Verify / Sprawdź**.
5. Jeżeli kompilacja przejdzie poprawnie, kliknij **Upload / Wgraj**.
6. Po wgraniu programu otwórz Serial Monitor i sprawdź, czy Arduino wysyła komunikaty.

---

## 10. Przygotowanie aplikacji Processing

### 10.1. Instalacja Processing

1. Pobierz i zainstaluj Processing.
2. Otwórz plik aplikacji `.pde`.
3. Sprawdź ustawienia portu szeregowego.

---

### 10.2. Ustawienie portu COM

Jeżeli Arduino działa na porcie `COM6`, w kodzie Processing trzeba ustawić ten sam port.

W kodzie może występować fragment podobny do:

```java
String portName = "COM6";
myPort = new Serial(this, portName, 9600);
```

Jeżeli Twój kod używa listy portów, możesz zobaczyć dostępne porty przez:

```java
println(Serial.list());
```

Następnie wybierz port odpowiadający Arduino.

Przykład dla `COM6`:

```java
myPort = new Serial(this, "COM6", 9600);
```

Prędkość transmisji, na przykład `9600`, musi być taka sama jak w kodzie Arduino:

```cpp
Serial.begin(9600);
```

---

## 11. Uruchomienie całego projektu

1. Podłącz wszystkie elementy do Arduino zgodnie z tabelą pinów.
2. Podłącz Arduino do komputera przez USB.
3. Wgraj kod Arduino.
4. Zamknij Serial Monitor w Arduino IDE, ponieważ port COM może być używany tylko przez jeden program naraz.
5. Otwórz aplikację Processing.
6. Uruchom aplikację Processing przyciskiem **Run**.
7. Sprawdź, czy panel pokazuje status zamka.
8. Przyłóż poprawną kartę RFID.
9. Sprawdź, czy:
   - zamek otwiera się,
   - panel pokazuje dostęp przyznany,
   - po kilku sekundach zamek zamyka się automatycznie.
10. Przyłóż niepoprawną kartę RFID.
11. Sprawdź, czy:
   - panel pokazuje odmowę dostępu,
   - po kilku błędnych próbach uruchamia się alarm,
   - czerwona dioda LED świeci,
   - buzzer wydaje dźwięk.

---

## 12. Testy końcowe

| Test | Oczekiwany rezultat |
|---|---|
| Uruchomienie Arduino | Zamek ustawia się w pozycji zamkniętej |
| Przyłożenie poprawnej karty | Zamek otwiera się |
| Automatyczne zamknięcie | Po kilku sekundach zamek wraca do pozycji zamkniętej |
| Przyłożenie błędnej karty | Dostęp zostaje odrzucony |
| Kilka błędnych prób | Uruchamia się alarm |
| Alarm fizyczny | Świeci czerwona LED i działa buzzer |
| Komunikacja z Processing | Panel pokazuje status, logi i alarmy |
| Ręczne sterowanie z Processing | Przyciski otwierania/zamykania działają, jeżeli są zaimplementowane |

---