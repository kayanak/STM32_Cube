# Neues Projekt in STM32CubeIDE erstellen (ohne CubeMX)

## Voraussetzungen

- **STM32CubeIDE** installiert (Download: https://www.st.com/en/development-tools/stm32cubeide.html)
- STM32-Board oder Mikrocontroller bekannt (z.B. STM32F103C8T6, STM32F411RE, etc.)

---

## Schritt-für-Schritt Anleitung

### 1. STM32CubeIDE starten

- Programm öffnen
- Einen **Workspace** auswählen (Ordner, in dem Projekte gespeichert werden)
- Mit **Launch** bestätigen

### 2. Neues leeres C/C++-Projekt anlegen

- Menü: **File → New → C/C++ Project**
- Im Dialog **C Managed Build** auswählen und **Next** klicken

### 3. Projektname und Toolchain

- **Project Name**: Einen Namen eingeben (z.B. `MeinErstesProjekt`)
- **Project type**: **Empty Project** wählen
- **Toolchain**: **MCU ARM GCC** auswählen
- **Next** klicken

### 4. Mikrocontroller auswählen

- Im **MCU Settings**-Dialog den Chip-Namen eingeben (z.B. `STM32F103C8`)
- Den passenden Controller in der Liste anklicken
- **Finish** klicken

### 5. Projektstruktur anlegen

Das Projekt enthält zunächst nur einen leeren Ordner. Folgende Dateien manuell anlegen:

#### Ordnerstruktur erstellen
- Rechtsklick auf Projekt → **New → Source Folder** → Name: `Src`
- Rechtsklick auf Projekt → **New → Folder** → Name: `Inc`

#### Startup-Datei einbinden
- Die Startup-Datei (`startup_stm32f103xx.s`) und das Linker-Script (`.ld`) werden automatisch bereitgestellt
- Falls nicht vorhanden: aus dem STM32Cube Firmware-Paket kopieren

#### main.c erstellen
- Rechtsklick auf `Src` → **New → Source File** → Name: `main.c`

### 6. Code schreiben (Bare-Metal / Register-Ebene)

Beispiel für eine LED-Blink-Anwendung mit direktem Registerzugriff:

```c
#include "stm32f1xx.h"  // Header passend zum MCU anpassen

int main(void)
{
    // Takt für GPIOA aktivieren (RCC)
    RCC->APB2ENR |= RCC_APB2ENR_IOPAEN;

    // PA5 als Output Push-Pull konfigurieren (max. 2 MHz)
    GPIOA->CRL &= ~(GPIO_CRL_CNF5 | GPIO_CRL_MODE5);
    GPIOA->CRL |= GPIO_CRL_MODE5_1;  // Output 2 MHz

    while (1)
    {
        GPIOA->ODR ^= GPIO_ODR_ODR5;  // PA5 toggeln

        // Einfache Verzögerung
        for (volatile uint32_t i = 0; i < 200000; i++);
    }
}
```

> **Hinweis:** Die Header-Datei (z.B. `stm32f1xx.h`, `stm32f4xx.h`) richtet sich nach deiner MCU-Familie. Sie stellt alle Register-Definitionen bereit.

### 7. Include-Pfade konfigurieren

Falls der Compiler die Header nicht findet:

1. Rechtsklick auf Projekt → **Properties**
2. **C/C++ Build → Settings → MCU GCC Compiler → Include paths**
3. Pfad zum `Inc`-Ordner und zu den CMSIS-Headern hinzufügen

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
| Strg+S     | Speichern                   |
| Strg+F11   | Debug starten               |
| F8         | Programm fortsetzen         |
| Strg+F2    | Debug beenden               |

---

## Tipps

- **CMSIS-Header**: Stellen Register-Definitionen bereit (z.B. `RCC->APB2ENR`), ohne HAL-Overhead
- **Reference Manual**: Das Reference Manual deines STM32 enthält alle Register-Beschreibungen (von st.com herunterladen)
- **Datasheet**: Enthält die Pin-Belegung und elektrischen Eigenschaften
- **HAL optional nutzen**: Falls du doch einzelne HAL-Treiber verwenden möchtest, kannst du sie manuell aus dem Firmware-Paket einbinden, ohne CubeMX zu benötigen
