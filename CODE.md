# Arduino Temperature-Controlled Fan with DHT11 and LCD

## Overview
This program uses:

- A DHT11 sensor to measure temperature
- A 16x2 LCD display to show information
- A fan connected to pin 8
- An Arduino board to control everything

The system:

- Reads the temperature from the DHT11 sensor
- Turns the fan ON if the temperature is higher than **28°C**
- Turns the fan OFF if the temperature is lower than or equal to **28°C**
- Displays the temperature and fan status on the LCD

---

# Complete Code Explanation

## 1. Libraries

```cpp
#include <LiquidCrystal.h>
#include <DHT.h>
```

### Explanation
These libraries allow Arduino to communicate with external devices.

- `LiquidCrystal.h`
  - Controls the 16x2 LCD display

- `DHT.h`
  - Controls the DHT11 temperature sensor

---

# 2. LCD Configuration

```cpp
// ---------------- LCD ----------------
// RS, E, D4, D5, D6, D7
LiquidCrystal lcd(12, 11, 5, 4, 3, 6);
```

### Explanation

Creates an LCD object named `lcd`.

The numbers correspond to Arduino pins connected to the LCD:

| LCD Pin | Arduino Pin |
|---|---|
| RS | 12 |
| E | 11 |
| D4 | 5 |
| D5 | 4 |
| D6 | 3 |
| D7 | 6 |

---

# 3. DHT11 Sensor Configuration

```cpp
#define DHTPIN 2
#define DHTTYPE DHT11
```

### Explanation

Defines:

- `DHTPIN`
  - The Arduino pin connected to the DHT11 data pin
  - Here it is pin `2`

- `DHTTYPE`
  - Specifies the sensor model
  - Here it is `DHT11`

---

```cpp
DHT dht(DHTPIN, DHTTYPE);
```

### Explanation

Creates a DHT sensor object called `dht`.

This object will be used to read temperature values.

---

# 4. Fan Configuration

```cpp
// ---------------- Ventilador ----------------
const int fanPin = 8;
```

### Explanation

The fan is connected to Arduino pin `8`.

`const int` means this value will never change.

---

# 5. Temperature Limit

```cpp
// ---------------- Temperatura límite ----------------
float limiteTemp = 28;
```

### Explanation

Stores the temperature limit.

If the temperature is greater than `28°C`, the fan turns ON.

`float` is used because temperatures can contain decimals.

---

# 6. setup() Function

```cpp
void setup()
{
```

### Explanation

`setup()` runs only once when the Arduino starts.

---

## Configure Fan Pin

```cpp
pinMode(fanPin, OUTPUT);
```

### Explanation

Sets pin 8 as an output because it controls the fan.

---

## Turn Fan OFF Initially

```cpp
digitalWrite(fanPin, LOW);
```

### Explanation

Starts with the fan OFF.

- `LOW` = OFF
- `HIGH` = ON

---

## Initialize LCD

```cpp
lcd.begin(16, 2);
```

### Explanation

Starts the LCD.

- 16 columns
- 2 rows

---

## Initialize DHT11

```cpp
dht.begin();
```

### Explanation

Starts communication with the DHT11 sensor.

---

# 7. loop() Function

```cpp
void loop()
{
```

### Explanation

`loop()` repeats forever while the Arduino is powered.

---

# 8. Read Temperature

```cpp
float temperatureC = dht.readTemperature();
```

### Explanation

Reads the temperature in Celsius from the DHT11 sensor.

The value is stored in `temperatureC`.

Example:

```cpp
temperatureC = 29.4;
```

---

# 9. Check Sensor Errors

```cpp
if (isnan(temperatureC))
```

### Explanation

Checks if the reading failed.

`isnan()` means:

> "Is this value NOT a number?"

If the sensor fails, the program enters the `if` block.

---

## Display Error Message

```cpp
lcd.clear();

lcd.setCursor(0, 0);
lcd.print("Error sensor");

delay(1000);

return;
```

### Explanation

- Clears the LCD
- Prints `"Error sensor"`
- Waits 1 second
- Stops the current loop iteration

`return;` exits the loop cycle early.

---

# 10. Fan State Variable

```cpp
bool fanState = false;
```

### Explanation

Creates a Boolean variable to store fan status.

Possible values:

| Value | Meaning |
|---|---|
| `true` | Fan ON |
| `false` | Fan OFF |

Initially, it starts as OFF.

---

# 11. Temperature Comparison

```cpp
if (temperatureC > limiteTemp)
```

### Explanation

Checks whether the temperature is greater than 28°C.

---

## Turn Fan ON

```cpp
digitalWrite(fanPin, HIGH);

fanState = true;
```

### Explanation

- Sends HIGH signal to pin 8
- Turns the fan ON
- Updates the fan status variable

---

## Turn Fan OFF

```cpp
digitalWrite(fanPin, LOW);

fanState = false;
```

### Explanation

If temperature is NOT above the limit:

- Fan turns OFF
- Status becomes false

---

# 12. Clear LCD

```cpp
lcd.clear();
```

### Explanation

Erases previous text from the screen.

---

# 13. Display Temperature

```cpp
lcd.setCursor(0, 0);

lcd.print("Temp: ");

lcd.print(temperatureC);
```

### Explanation

Moves cursor to:

- Column 0
- Row 0

Then prints:

```text
Temp: 29.0
```

---

## Degree Symbol

```cpp
lcd.print((char)223);
```

### Explanation

Prints the `°` symbol on the LCD.

---

## Print Celsius

```cpp
lcd.print("C");
```

### Result on LCD

```text
Temp: 29°C
```

---

# 14. Display Fan Status

```cpp
lcd.setCursor(0, 1);
```

### Explanation

Moves cursor to:

- Column 0
- Row 1

(second row)

---

## If Fan is ON

```cpp
if (fanState)
{
  lcd.print("Vent: ON ");
}
```

### LCD Output

```text
Vent: ON
```

---

## If Fan is OFF

```cpp
else
{
  lcd.print("Vent: OFF");
}
```

### LCD Output

```text
Vent: OFF
```

---

# 15. Delay

```cpp
delay(1000);
```

### Explanation

Waits 1000 milliseconds = 1 second before repeating.

This prevents the LCD from updating too fast.

---

# Program Logic Summary

```text
START
   ↓
Initialize LCD
Initialize DHT11
Turn fan OFF
   ↓
Read temperature
   ↓
Did sensor fail?
   ├── YES → Show error
   └── NO
         ↓
Is temperature > 28°C?
   ├── YES → Turn fan ON
   └── NO  → Turn fan OFF
         ↓
Display temperature
Display fan status
         ↓
Wait 1 second
         ↓
Repeat forever
```

---

# Possible Improvements

You could improve the project by adding:

- Humidity display
- PWM fan speed control
- Temperature history graph
- Alarm buzzer
- Buttons to change temperature limit
- RGB LED indicators
- Automatic brightness control

---

# Example LCD Output

## Fan OFF

```text
Temp: 25°C
Vent: OFF
```

## Fan ON

```text
Temp: 31°C
Vent: ON
```