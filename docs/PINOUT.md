# Pinout

Pinout fuer den Sendspin Gartenhaus ESPHome Player.

## ESP32-S3

| Funktion | GPIO | Hinweis |
| --- | ---: | --- |
| Display Backlight PWM | GPIO15 | `display_backlight_pwm` |
| WS2812 Daten | GPIO14 | 16 LEDs in ESPHome, zwei 16er-Ringe parallel |
| Verstaerker Enable | GPIO21 | `amplifier_enable` |
| Rotary Encoder A | GPIO1 | Pullup aktiv |
| Rotary Encoder B | GPIO2 | Pullup aktiv |
| Taste Next | GPIO17 | invertiert, Pullup |
| Taste Previous | GPIO16 | invertiert, Pullup |
| Taste INC | GPIO4 | invertiert, Pullup |
| Taste DEC | GPIO18 | invertiert, Pullup |
| Taste OK / Encoder-Taste | GPIO19 | invertiert, Pullup, Kurz/Langdruck |

## I2S Audio

| Signal | GPIO | Hinweis |
| --- | ---: | --- |
| BCLK | GPIO7 | I2S Bit Clock |
| LRCLK / WS | GPIO5 | I2S Word Select |
| DOUT | GPIO6 | I2S Data Out zum externen DAC |

Konfiguration:

- Stereo
- 48 kHz
- 16 Bit
- externer DAC
- Buffer: 100 ms

## TFT Display ILI9341

| Signal | GPIO | Hinweis |
| --- | ---: | --- |
| SPI CLK | GPIO12 | `display_spi` |
| SPI MOSI | GPIO11 | `display_spi` |
| CS | GPIO10 | Chip Select |
| DC | GPIO13 | Data/Command |
| RESET | GPIO9 | Reset |
| Backlight | GPIO15 | PWM |

Konfiguration:

- Modell: ILI9341
- SPI: `spi2`
- SPI Mode: MODE2
- Datenrate: 20 MHz
- Rotation: 270 Grad
- Pixel Mode: 18 Bit

## LED-Ring

| Wert | Einstellung |
| --- | --- |
| LED-Typ | WS2812 |
| Datenpin | GPIO14 |
| Farbfolge | GRB |
| Anzahl in ESPHome | 16 |
| Hardware-Aufbau | 2 x 16 LEDs parallel |
| Effekt | `Sendspin Sound-to-Light` |
| Update-Intervall | 35 ms |

Die zwei Ringe sind parallel angeschlossen. Dadurch zeigen beide Ringe dasselbe
16-LED-Muster.

## Hinweise zur Verdrahtung

- Gemeinsame Masse zwischen ESP32, DAC, Display, LED-Ring und Verstaerker ist erforderlich.
- WS2812-Datenleitung kurz halten oder mit Pegelanpassung betreiben, falls die LEDs mit 5 V laufen.
- Fuer WS2812 eine passende externe 5-V-Versorgung verwenden, wenn die Ringe mehr Strom ziehen.
- GPIO19 kann mit USB-Serial-JTAG kollidieren. Wenn USB-Logging oder Flashen ueber USB Probleme macht, OK-Taste testweise abklemmen oder auf einen anderen freien GPIO legen.
