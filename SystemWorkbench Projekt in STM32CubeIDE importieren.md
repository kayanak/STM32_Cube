# SystemWorkbench (SW4STM32) Projekt in STM32CubeIDE importieren

## Voraussetzungen

- **STM32CubeIDE** installiert
- Bestehendes **SystemWorkbench (SW4STM32)** Projekt vorhanden
- Projekt-Ordner zugänglich (lokal oder auf einem Laufwerk)

---

## Schritt-für-Schritt Anleitung

### 1. STM32CubeIDE öffnen

- Einen Workspace auswählen
- **Wichtig:** Nicht den gleichen Workspace wie in SystemWorkbench verwenden

### 2. Import starten

- Menü: **File → Import...**
- Im Dialog den Ordner aufklappen: **General → Existing Projects into Workspace**
  - Alternativ: **General → Import ac6 System Workbench for STM32 Project**
  - (Falls diese Option verfügbar ist, bevorzugt diese wählen)
- **Next** klicken

### 3. Projekt auswählen

1. **Select root directory** aktivieren
2. **Browse...** klicken und den Ordner des SW4STM32-Projekts auswählen
3. Das Projekt erscheint in der Liste unter **Projects**
4. Haken setzen beim gewünschten Projekt
5. **Copy projects into workspace** aktivieren (empfohlen als Sicherheitskopie)
6. **Finish** klicken

### 4. Projekt konvertieren

Nach dem Import erscheint möglicherweise ein Dialog:

- **"Convert to STM32CubeIDE project?"** → **Yes** klicken
- CubeIDE passt die Projektstruktur und Build-Konfiguration automatisch an

Falls der Dialog nicht erscheint:
1. Rechtsklick auf das Projekt im **Project Explorer**
2. **Convert → Convert to STM32CubeIDE project**

### 5. Build-Einstellungen prüfen

Nach der Konvertierung prüfen:

1. Rechtsklick auf Projekt → **Properties**
2. **C/C++ Build → Settings** öffnen
3. Kontrollieren:
   - **MCU Settings**: Richtiger Mikrocontroller ausgewählt?
   - **Include paths**: Alle Header-Pfade vorhanden?
   - **Source locations**: Alle Quellcode-Ordner eingebunden?
   - **Libraries**: Benötigte Bibliotheken verlinkt?

### 6. Häufige Anpassungen nach dem Import

#### Compiler-Fehler bei Include-Pfaden
- Pfade, die auf den alten SW4STM32-Ordner zeigen, manuell anpassen
- **Properties → C/C++ Build → Settings → MCU GCC Compiler → Include paths**

#### Linker-Script prüfen
- **Properties → C/C++ Build → Settings → MCU GCC Linker → General**
- Sicherstellen, dass das richtige `.ld`-Linker-Script eingetragen ist

#### Startup-Datei
- Prüfen ob die `startup_stm32xxxx.s` Datei im Projekt vorhanden ist
- Falls doppelt vorhanden: eine Version entfernen

#### Defines/Symbole übernehmen
- **Properties → C/C++ Build → Settings → MCU GCC Compiler → Preprocessor**
- Alle Defines aus dem alten Projekt übernehmen (z.B. `STM32F103xB`, `USE_HAL_DRIVER`)

### 7. Projekt kompilieren

- **Strg+B** drücken
- Fehler im **Console**- und **Problems**-Fenster prüfen und beheben

### 8. Debug-Konfiguration einrichten

1. **Run → Debug Configurations...**
2. Doppelklick auf **STM32 C/C++ Application**
3. Prüfen:
   - **Debugger**: ST-Link oder J-Link korrekt ausgewählt
   - **Main → C/C++ Application**: Pfad zur `.elf`-Datei korrekt
4. **Apply** → **Debug**

---

## Häufige Probleme und Lösungen

| Problem | Lösung |
|---------|--------|
| `undefined reference` Fehler | Fehlende Quellcode-Dateien zum Build hinzufügen |
| Header nicht gefunden | Include-Pfade in den Compiler-Settings anpassen |
| Linker-Fehler | Linker-Script und Library-Pfade prüfen |
| Doppelte Symbole | Doppelte Startup- oder Quell-Dateien entfernen |
| Debug startet nicht | Debug-Konfiguration und ST-Link Treiber prüfen |

---

## Tipps

- **Backup erstellen**: Vor dem Import das Original-Projekt sichern
- **Clean Build**: Nach dem Import **Project → Clean** und dann neu bauen
- **Alte Projekt-Dateien**: Die Dateien `.cproject` und `.project` werden beim Konvertieren aktualisiert
- **SW4STM32 nicht mehr nötig**: Nach erfolgreichem Import kann ausschließlich in CubeIDE weitergearbeitet werden
