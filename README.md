Pin out for ESP32C3
LCD RST → IO9 (Pin labeled 9)

LCD CE/CS → IO8 (Pin labeled 8)

LCD DC → IO7 (Pin labeled 7)

LCD DIN/MOSI → IO6 (Pin labeled 6)

LCD CLK/SCK → IO4 (Pin labeled 4)

LCD VCC → 3V3

LCD GND → GND

LCD BL (Backlight) → 3V3 via your 47Ω resistor

A compact, real-time audio visualizer built on the ESP32-C3 and a classic Nokia 5510 LCD (84×48 monochrome, PCD8544). Ambient sound is captured by an analog electret microphone, processed into a smooth envelope, and rendered as a mesmerizing dual mirrored sine wave that breathes and scrolls with the music.

Features
Dual Mirrored Waves: Two sine waves bounce symmetrically from the center, expanding toward the top and bottom of the screen as volume increases.

Sound-Reactive Everything: Amplitude, wave frequency, and scroll speed all dynamically respond to the audio envelope.

Integer Math Engine: Uses a 256-entry sine lookup table and uint8_t phase wrapping for buttery-smooth animation without floating-point drift or visual glitches.

Auto-Calibrating Baseline: Measures the microphone's resting DC bias at startup so silence produces a calm, flat line instead of a constantly maxed-out display.

Beat Flash & LED: A dedicated LED (and optional full-screen invert) pulses on heavy bass hits.

ESP32-C3 Power: Upgraded from the ESP8266 for faster processing, better SPI headroom, and a 12-bit ADC.

Hardware
Component	Connection
Nokia 5510 LCD RST / CE / DC / DIN / CLK	IO9 / IO8 / IO7 / IO6 / IO4
LCD VCC / GND	3.3V / GND
LCD BL (Backlight)	3.3V (via 47Ω resistor)
Electret Mic (KE0034) OUT	IO1 (ADC)
Mic VCC / GND	3.3V / GND
LED	IO10 (via 220Ω)
Software
Libraries: Adafruit_PCD8544, Adafruit_GFX

Core: Arduino ESP32 Core (ESP32-C3 Dev Module)

Key Tuning Values:

MAP_MAX_RAW: Sets the sensitivity (adjust based on your mic's peak output).

LCD_CONTRAST: Tuned to 100 for this specific clone module.

AMP_MIN/MAX & FREQ_MIN/MAX: Controls how wild the wave gets at high volume.

How it Works
Sampling: The ESP32 reads the analog mic 30 times a second.

Envelope Following: The raw signal is cleaned (subtracting the baseline), clamped, and smoothed to create a "level" variable that moves fast on attacks but decays slowly.

Mapping: This level is mapped to amplitude (height), frequency (density of the waves), and phase increment (scroll speed).

Rendering: A pre-computed sine table is indexed using a rolling phase counter. The wave is drawn pixel-by-pixel, and a second inverted copy is drawn from the bottom up to create the "helix" effect.
