# Neues Projekt in STM32CubeIDE erstellen

## Voraussetzungen

- **STM32CubeIDE** installiert (Download: https://www.st.com/en/development-tools/stm32cubeide.html)
- STM32-Board oder Mikrocontroller bekannt (z.B. STM32F103C8T6, STM32F411RE, etc.)

---

## Schritt-für-Schritt Anleitung

### 1. STM32CubeIDE starten

- Programm öffnen
- Einen **Workspace** auswählen (Ordner, in dem Projekte gespeichert werden)
- Mit **Launch** bestätigen

### 2. Neues Projekt anlegen

- Menü: **File → New → STM32 Project**
- Es öffnet sich der **Target Selection** Dialog

### 3. Mikrocontroller auswählen

Es gibt zwei Möglichkeiten:

#### Option A: Nach MCU suchen
1. Tab **MCU/MPU Selector** wählen
2. Im Suchfeld oben den Chip-Namen eingeben (z.B. `STM32F103C8`)
3. Den passenden Controller in der Liste anklicken
4. **Next** klicken

#### Option B: Nach Board suchen
1. Tab **Board Selector** wählen
2. Im Suchfeld den Board-Namen eingeben (z.B. `NUCLEO-F411RE`)
3. Das Board in der Liste anklicken
4. **Next** klicken

### 4. Projektname und Einstellungen

- **Project Name**: Einen Namen eingeben (z.B. `MeinErstesProjekt`)
- **Targeted Language**: `C` oder `C++` auswählen
- **Targeted Binary Type**: `Executable` belassen
- **Targeted Project Type**: `STM32Cube` belassen
- Mit **Finish** bestätigen

### 5. Peripherie konfigurieren (CubeMX-Ansicht)

Nach dem Erstellen öffnet sich die **CubeMX-Konfigurationsansicht** (`.ioc`-Datei):

1. **Pinout & Configuration**: Pins und Peripherie konfigurieren
   - Links die Peripherie-Kategorien (GPIO, USART, SPI, I2C, Timer, etc.)
   - Auf dem Chip-Bild Pins anklicken und Funktion zuweisen
2. **Clock Configuration**: Taktfrequenzen einstellen
   - HSE/HSI Quelle wählen
   - PLL konfigurieren
   - Systemtakt (HCLK) einstellen
3. **Project Manager**:
   - Unter **Code Generator** die Option aktivieren:
     - ✅ *Generate peripheral initialization as a pair of .c/.h files per peripheral*

### 6. Code generieren

- **Strg+S** drücken oder **Project → Generate Code**
- CubeIDE generiert automatisch den Initialisierungscode
- Bei der Frage "Do you want to open the C/C++ perspective?" → **Yes**

### 7. Eigenen Code schreiben

- Die Datei `Core/Src/main.c` öffnen
- Eigenen Code **nur zwischen** die `USER CODE`-Kommentare schreiben:

```c
/* USER CODE BEGIN 2 */
// Eigener Initialisierungscode hier
/* USER CODE END 2 */

/* USER CODE BEGIN WHILE */
while (1)
{
    // Eigener Code in der Hauptschleife hier
    HAL_GPIO_TogglePin(GPIOA, GPIO_PIN_5);
    HAL_Delay(500);

    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
}
/* USER CODE END 3 */
```

> **Wichtig:** Code außerhalb der `USER CODE`-Blöcke wird bei erneuter Code-Generierung überschrieben!

### 8. Projekt kompilieren

- **Strg+B** oder Menü: **Project → Build Project**
- Im **Console**-Fenster den Build-Fortschritt prüfen
- Bei Erfolg erscheint: `Build Finished. 0 errors, 0 warnings`

### 9. Auf den Mikrocontroller flashen

1. STM32-Board per USB/ST-Link anschließen
2. **Run → Debug As → STM32 C/C++ Application** (oder Strg+F11)
3. Beim ersten Mal die Debug-Konfiguration bestätigen
4. Das Programm wird auf den Controller übertragen und startet im Debug-Modus
5. Mit **F8 (Resume)** das Programm starten

---

## Nützliche Tastenkürzel

| Kürzel     | Funktion                    |
|------------|-----------------------------|
| Strg+B     | Projekt kompilieren (Build) |
| Strg+S     | Speichern / Code generieren |
| Strg+F11   | Debug starten               |
| F8         | Programm fortsetzen         |
| Strg+F2    | Debug beenden               |

---

## Tipps

- **Peripherie nachträglich ändern**: Doppelklick auf die `.ioc`-Datei im Projektbaum
- **HAL-Dokumentation**: Rechtsklick auf eine HAL-Funktion → **Open Declaration**
- **Beispielprojekte**: Menü **Help → Manage Embedded Software Packages** → Beispiele für dein Board herunterladen
