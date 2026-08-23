# Bill of Materials

Bill of Materials fuer den Sendspin Gartenhaus ESPHome Player.

## Bauteile

| Menge | Bauteil | Technische Daten | Verwendung | Quelle |
| ---: | --- | --- | --- | --- |
| 1 | ESP32-S3 Entwicklungsboard N16R8 | ESP32-S3, 2.4 GHz WLAN, USB-C, 44-Pin, 16 MB Flash, 8 MB PSRAM, Arduino/ESP-IDF geeignet | Hauptcontroller fuer ESPHome, Sendspin, Display, Audio, LED-Visualizer und Bedienung | [AliExpress](https://de.aliexpress.com/item/1005009962649997.html) |
| 1 | TFT Display ILI9341 SPI | 240 x 320 Pixel, SPI, ILI9341 Controller, Display-Backlight separat angesteuert | Anzeige von Album-Art, Titel, Interpret, Album, Laufzeit und Playerstatus | [AliExpress](https://de.aliexpress.com/item/1005006315533240.html) |
| 1 | Rotary Encoder mit Drucktaster | Drehencoder mit A/B-Signal und integriertem Taster | Lautstaerkeregelung per Drehung, Encoder-Taster auf GPIO35 | [AliExpress](https://de.aliexpress.com/item/1005001453647644.html) |
| 1 | 5 Key Matrix Array | 5 Tasten, einzelne GPIO-Eingaenge mit Pullup | Lokale Medientasten fuer Next, Previous, INC, DEC und OK | [AliExpress](https://de.aliexpress.com/item/1005006245093356.html) |
| 1 | DC-DC Step-Down / Buck-Wandler | Eingang 12 V, Ausgang auf 5,0 V einstellen | Spannungsversorgung von 12 V auf 5 V fuer ESP32/LED/Peripherie | [AliExpress](https://de.aliexpress.com/item/1005009690612836.html) |
| 1 | Steckernetzteil | 12 V DC, 2 A | Hauptversorgung fuer den Player | vom Benutzer angegeben |
| 2 | I2S-Mono-Verstaerker | I2S-Eingang, L/R-Kanalauswahl am Modul, Betrieb an 5 V pruefen | Stereo-Verstaerkung mit je einem Modul fuer links und rechts | [AliExpress](https://de.aliexpress.com/item/1005007003802663.html) |
| 2 | WS2812 LED-Ring 16 LEDs | adressierbarer RGB-LED-Ring, 16 LEDs, GRB-Farbfolge in ESPHome | Sound-to-Light / VU-Visualizer | [AliExpress](https://de.aliexpress.com/item/1005006140650184.html) |
| 2 | Lautsprecher | Impedanz und Leistung am gelieferten Modell pruefen | Stereo-Audioausgabe des Players | [AliExpress](https://de.aliexpress.com/item/1005007733110506.html) |

## Controller

| Merkmal | Wert |
| --- | --- |
| Modul | ESP32-S3 Entwicklungsboard |
| Variante | N16R8 |
| Flash | 16 MB |
| PSRAM | 8 MB |
| USB | Typ-C |
| Pinleiste | 44-Pin |
| Funk | 2.4 GHz WLAN |
| Framework | ESP-IDF ueber ESPHome |

Die YAML verwendet `esp32-s3-devkitc-1`, `ESP32S3`, 16 MB Flash und Octal-PSRAM
mit 80 MHz.

## Encoder-Anschluss

| Encoder-Signal | ESP32-S3 GPIO | Hinweis |
| --- | ---: | --- |
| A / CLK | GPIO1 | Rotary Encoder Eingang, Pullup aktiv |
| B / DT | GPIO2 | Rotary Encoder Eingang, Pullup aktiv |
| SW / Encoder Button | GPIO35 | Drucktaster, invertiert, Pullup aktiv |
| VCC | 3.3 V | Je nach Encoder-Modul pruefen |
| GND | GND | Gemeinsame Masse |

Der Encoder steuert in der aktuellen YAML die Lautstaerke. Der integrierte
Drucktaster liegt auf GPIO35 und ist getrennt von der OK-Taste des 5-Key-Arrays.

## 5-Key-Array-Anschluss

| Taste | ESP32-S3 GPIO | Funktion in der YAML |
| --- | ---: | --- |
| Next | GPIO17 | Naechster Track |
| Previous | GPIO16 | Vorheriger Track |
| INC | GPIO4 | Vorheriger Track |
| DEC | GPIO18 | Naechster Track |
| OK | GPIO19 | Play/Pause, langer Druck fuer Player Power Ein/Aus |
| Common / GND | GND | Gemeinsame Masse |

Die Tasten sind in ESPHome als invertierte GPIO-Eingaenge mit internem Pullup
konfiguriert. Beim Tastendruck wird der jeweilige Eingang gegen GND gezogen.

## Stromversorgung

| Bauteil | Anschluss | Hinweis |
| --- | --- | --- |
| Steckernetzteil | 12 V DC / 2 A | Eingang fuer das Geraet |
| Step-Down-Wandler Eingang | 12 V und GND | Vom Steckernetzteil |
| Step-Down-Wandler Ausgang | 5,0 V und GND | Versorgung fuer 5-V-Schiene |
| ESP32-S3 | 5 V/VIN und GND | Vom Step-Down-Ausgang, je nach Board-Beschriftung |
| WS2812 LED-Ringe | 5 V und GND | Externe 5-V-Schiene empfohlen |
| I2S-Verstaerker | 5 V und GND | Je nach Modul-Beschriftung anschliessen |

Vor dem Anschluss an ESP32, Display oder LEDs den Ausgang des Step-Down-Wandlers
mit einem Multimeter auf 5,0 V einstellen. Alle GND-Leitungen muessen verbunden
sein, damit Audio, Display, LED-Daten und Tasten sauber funktionieren.

## LED-Ringe

| LED-Signal | ESP32-S3 / Versorgung | Hinweis |
| --- | --- | --- |
| DIN | GPIO14 | Datenleitung fuer WS2812 |
| 5 V | 5-V-Schiene vom Step-Down | Externe 5-V-Versorgung empfohlen |
| GND | GND | Gemeinsame Masse mit ESP32 |

Verwendet werden zwei 16er LED-Ringe. Beide Ringe sind parallel angeschlossen,
deshalb ist in ESPHome `num_leds: 16` konfiguriert. Beide Ringe zeigen dadurch
das gleiche 16-LED-Muster.

## Audio-Ausgang

| Bauteil | Anschluss | Hinweis |
| --- | --- | --- |
| I2S-Verstaerker links | BCLK GPIO7, LRCLK GPIO5, DIN GPIO6 | L/R-Select am Modul auf linken Kanal setzen |
| I2S-Verstaerker rechts | BCLK GPIO7, LRCLK GPIO5, DIN GPIO6 | L/R-Select am Modul auf rechten Kanal setzen |
| I2S-Verstaerker links | 5 V und GND | Versorgung von der 5-V-Schiene |
| I2S-Verstaerker rechts | 5 V und GND | Versorgung von der 5-V-Schiene |
| Lautsprecher links | Ausgang des linken Verstaerkers | Polung beachten |
| Lautsprecher rechts | Ausgang des rechten Verstaerkers | Polung beachten |

Die Lautsprecher werden nicht direkt vom ESP32-S3 angetrieben, sondern ueber den
I2S-Verstaerker. Fuer Stereo werden zwei gleiche I2S-Mono-Verstaerker verwendet.
BCLK, LRCLK/WS und DIN werden parallel auf beide Module gefuehrt. Das linke
Modul wird per L/R-Select auf links gesetzt, das rechte Modul per L/R-Select auf
rechts. Impedanz und maximale Leistung der Lautsprecher muessen zum verwendeten
Verstaerker-Modul passen.

## Display-Anschluss

| Display-Signal | ESP32-S3 GPIO | Hinweis |
| --- | ---: | --- |
| CLK / SCK | GPIO12 | SPI Clock |
| MOSI / SDI | GPIO11 | SPI Data |
| CS | GPIO10 | Chip Select |
| DC | GPIO13 | Data/Command |
| RESET | GPIO9 | Display Reset |
| LED / BL | GPIO15 | Backlight per PWM |
| VCC | 3.3 V | Je nach Modul pruefen |
| GND | GND | Gemeinsame Masse |

## Hinweise

- Touch-Pins des Display-Moduls werden in der aktuellen ESPHome-Konfiguration nicht genutzt.
- In ESPHome ist das Display als `ili9xxx` mit Modell `ILI9341`, Rotation `270`, SPI Mode `MODE2` und 20 MHz Datenrate konfiguriert.
- Die genaue Display-Groesse bitte am gelieferten Modul gegenpruefen, da AliExpress-Angebote haeufig mehrere Varianten unter einem Artikel fuehren.
