# Sendspin Gartenhaus ESPHome Player

ESPHome-Konfiguration fuer einen ESP32-S3 Player mit Sendspin,
I2S-Audio, ILI9341-Display, WS2812 Sound-to-Light und lokaler Bedienung ueber
Drehencoder und Tasten.

Der aktuelle stabile Stand ist:

`versions/sendspin-media-gartenhaus_2026-08-23_subpixel-vu-default150-nodebug.yaml`

Die aktive Datei zum Flashen ist:

`sendspin_gartenhaus_player.yaml`

## Funktionen

- Sendspin Medienplayer fuer Home Assistant
- lokaler `speaker_source` Player fuer Audioausgabe ueber I2S-DAC
- Album-Art auf dem TFT-Display
- Titel, Interpret und Album als Laufschrift
- Fortschrittsbalken mit Laufzeit und Gesamtdauer
- Power-Schalter fuer Home Assistant und lokale OK-Langdruck-Bedienung
- WS2812 Sound-to-Light als analog wirkender VU-Ring
- Helligkeit und Empfindlichkeit in Home Assistant einstellbar
- Standard-Empfindlichkeit: 150%
- oberer Empfindlichkeitsbereich wird intern sanfter skaliert, damit 250% nicht sofort voll uebersteuert
- Rotary Encoder fuer Lautstaerke
- lokale Tasten fuer Tracksteuerung

## Hardware

- Board: ESP32-S3 DevKitC-1
- Flash: 16 MB
- PSRAM: Octal, 80 MHz
- Audio: externer I2S-DAC
- Display: ILI9341 ueber SPI
- LED: WS2812 an GPIO14
- LED-Anzahl in ESPHome: 16
- Aufbau: zwei 16er-LED-Ringe parallel, deshalb wird nur ein 16er-Ring angesteuert

Details stehen in [docs/PINOUT.md](docs/PINOUT.md).

## Home Assistant Entitaeten

Wichtige Entitaeten:

- `media_player.sendspin_gartenhaus_sendspin_gruppe`
- `media_player.sendspin_gartenhaus_player`
- `switch.sendspin_gartenhaus_player_power`
- `switch.sendspin_gartenhaus_sound_to_light`
- `number.sendspin_gartenhaus_sound_to_light_helligkeit`
- `number.sendspin_gartenhaus_sound_to_light_empfindlichkeit`
- `light.sendspin_gartenhaus_ws2812_leds`
- `light.sendspin_gartenhaus_display_backlight`

Der Power-Schalter schaltet lokal Display, LEDs und Verstaerker ab. Die
Media-Player `turn_on`/`turn_off` Aktionen sind mit diesem Power-Schalter
gekoppelt, damit Home Assistant, Mushroom Card und Voice Assistant den Player
ein- und ausschalten koennen.

## Bedienung

| Bedienelement | Funktion |
| --- | --- |
| Encoder drehen | Lautstaerke |
| OK kurz | Play/Pause |
| OK lang ca. 1,2 s | Player Power Ein/Aus |
| Next | Naechster Track |
| Previous | Vorheriger Track |
| INC | Vorheriger Track |
| DEC | Naechster Track |

## Sound-to-Light

Der LED-Effekt nutzt die Audiodaten aus der Sendspin-Audioquelle:

- RMS-Pegel ueber `get_audio_level()`
- Peak-Impuls ueber `consume_audio_peak()`
- LED-Update alle 35 ms
- subpixelartige Teilhelligkeit fuer weichere Uebergaenge
- VU-Anzeige von unten nach oben, symmetrisch ueber den Ring

Die Diagnoseausgabe `vu_debug` wurde im aktuellen Stand entfernt, damit der
Live-Log ruhig bleibt und keine unnoetige Last erzeugt.

## Installation

1. ESPHome 2026.8.0 oder neuer verwenden.
2. Dieses Projekt in den ESPHome-Konfigurationsordner kopieren.
3. `secrets.example.yaml` als Vorlage fuer `secrets.yaml` verwenden.
4. WLAN-Daten in `secrets.yaml` eintragen.
5. In ESPHome kompilieren und per OTA oder USB flashen.

Beispiel:

```bash
esphome compile sendspin_gartenhaus_player.yaml
esphome upload sendspin_gartenhaus_player.yaml --device sendspin-gartenhaus.local
```

## Projektstruktur

```text
sendspin_gartenhaus_player.yaml
components/
  sendspin/
  speaker_source/
docs/
  PINOUT.md
versions/
  README.md
  sendspin-media-gartenhaus_2026-08-23_subpixel-vu-default150-nodebug.yaml
secrets.example.yaml
```

Die Ordner `components/sendspin` und `components/speaker_source` gehoeren zum
Projekt, weil die YAML sie als lokale `external_components` nutzt.

## Hinweise

- `secrets.yaml` darf nicht nach GitHub hochgeladen werden.
- `.esphome/` enthaelt Build-Dateien und gehoert ebenfalls nicht ins Repository.
- GPIO19 wird fuer die OK-Taste genutzt. ESPHome warnt, dass GPIO19 mit
  USB-Serial-JTAG kollidieren kann. In diesem Aufbau wurde diese Warnung beim
  Kompilieren akzeptiert.

## Letzter getesteter Stand

- ESPHome: 2026.8.0
- Build: erfolgreich
- OTA-Flash: erfolgreich
- Stand: 2026-08-23, `subpixel-vu-default150-nodebug`
