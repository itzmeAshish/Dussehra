<div align="center">

# 🏹 Dussehra — Ravana Dahan on a 0.96" OLED
### Rama's arrow flies. Ravana burns. "Happy Dussehra" lights up. All on a screen the size of a postage stamp.

<img src="https://www.oledanimationmaker.com/github-readmes/Dussehra-preview.gif" alt="Dussehra animation playing on a 128x64 SSD1306 OLED: Rama fires an arrow, Ravana burns, Happy Dussehra appears" width="512" />

**300 frames. One ESP8266. Zero audio. Good over evil, in 1-bit.**

> **Built and tested on ESP8266.** Want it on a different board? [Open OLED Animation Maker](https://www.oledanimationmaker.com/), pick your board and library, import your clip and click **Get the Code**.



![Arduino](https://img.shields.io/badge/Arduino-IDE-00979D?logo=arduino&logoColor=white)
![ESP8266](https://img.shields.io/badge/Board-ESP8266-000000?logo=espressif&logoColor=white)
![OLED](https://img.shields.io/badge/Display-SSD1306%20128x64-111827)
![Frames](https://img.shields.io/badge/Frames-300-F97316)
![Loop](https://img.shields.io/badge/Loop-20.1s-E11D48)
![Library](https://img.shields.io/badge/Library-Adafruit%20SSD1306-orange)
![License](https://img.shields.io/badge/code-free%20to%20use-brightgreen)

[**⬇ Get the code**](#-60-second-start) · [**📖 Full tutorial**](https://www.oledanimationmaker.com/blog/dussehra-ravana-dahan-oled-animation-arduino-ssd1306.html) · [**🛠 Make your own**](https://www.oledanimationmaker.com/) · [**📸 Instagram**](https://www.instagram.com/oled_animator)

</div>

---

## Why this exists

**Dussehra (Vijayadashami) 2026 is on Tuesday, 20 October.** Every year people watch Ravana burn. This repo puts that moment on a **0.96" SSD1306** (128×64).

Rama draws his bow. The arrow crosses the screen. The ten-headed Ravana catches fire. Embers become fireworks, and the loop ends on a bold **Happy Dussehra**, then starts again.

Put it on a festival table, a hostel desk or a maker reel. People already know the story. The screen just has to keep it moving.

> 🔇 Pictures only. The sketch has no speaker. Play your own music and let the OLED do the visuals.

---

## What you get

| | |
|---|---|
| 🎞 **Frames** | 300 × 128×64 mono bitmaps in `PROGMEM` (`frame0` … `frame299`) |
| ⏱ **Speed** | 67 ms per frame (~15 fps), one number to change |
| 🔁 **Loop** | ≈ **20.1 seconds** per pass, then it restarts |
| 📚 **Libraries** | Adafruit SSD1306 + Adafruit GFX |
| 🔌 **I2C** | Address `0x3C` |
| 🧮 **Index** | `uint16_t`, because 300 doesn't fit in 8 bits |

---

## 🔁 Need a different board?

All the code here is made for **ESP8266**. Please don't hand-edit it for another chip. Instead, generate fresh code for your board on our website:

1. Open [oledanimationmaker.com](https://www.oledanimationmaker.com/)
2. Pick your **board** (the tool has a 24-board menu: Arduino Uno/Nano/Mega, many ESP32 types, Raspberry Pi Pico and more) and your **library**
3. Import your clip or GIF
4. Click **Get the Code** and upload it


---

## ⚡ 60-second start

```bash
git clone https://github.com/itzmeAshish/Dussehra.git
```

1. Rename the folder to **`main`** (the file is `main.ino`, and Arduino IDE needs the names to match)
2. Install **Adafruit SSD1306** and **Adafruit GFX** from Library Manager
3. Select your **ESP8266** board (for example *NodeMCU 1.0*) with a **4MB** flash size
4. Upload
5. Watch Ravana burn

Serial baud is **115200**. If you see `SSD1306 allocation failed`, check power, SDA/SCL, or try address `0x3D`.

> **New to Arduino?** The [step-by-step upload guide](https://www.oledanimationmaker.com/blog/upload-oled-animation-from-github-esp8266-arduino-ide.html) covers the board package, wiring diagram, board settings and a troubleshooting table.

---

## Requirements

| Item | Notes |
|------|--------|
| Board | **ESP8266** (NodeMCU or Wemos D1 Mini), 4MB flash |
| OLED | 0.96" SSD1306 I2C 128×64 @ `0x3C` |
| IDE | Arduino IDE 1.8+ or 2.x |
| Wires | 4 Dupont jumpers |

> **Will it run on an Uno?** No. 300 frames × 1024 bytes = **307,200 bytes (300 KB)** of pictures alone.
> Uno has 32 KB of flash and Mega 2560 has 256 KB, so neither can hold it.
> The comment at the top of `main.ino` mentions Uno pins because that's the generator's header. Wire an ESP8266 board.

---

## 🔌 Wiring

### ESP8266 NodeMCU / Wemos D1 Mini (the board this code is built for)

| OLED | Board | GPIO |
|------|--------|------|
| VCC | **3.3V** | — |
| GND | GND | — |
| SDA | **D2** | GPIO4 |
| SCL | **D1** | GPIO5 |

```cpp
Wire.begin(4, 5);  // only if the panel stays dark
```

Set **Tools → Flash Size** to a **4MB** scheme. A 512 KB or 1 MB profile rejects this sketch even when the chip is bigger.

---

## 🎚 Change the speed

In `loop()`:

```cpp
if (millis() - lastMs >= 67) {  // lower = faster
```

| Delay | Approx FPS | Full 300-frame pass |
|------:|-----------:|--------------------:|
| 134 ms | ~7.5 | ~40 s |
| 100 ms | 10 | 30 s |
| **67 ms (stock)** | **~15** | **~20.1 s** |
| 50 ms | 20 | 15 s |

If a stopwatch says ~27 seconds at the stock 67 ms, the I2C bus is the real clock. After the bus starts:

```cpp
Wire.setClock(400000);
```

Leave `numFrames` at **300**, and don't change `frameIdx` to `uint8_t`. It wraps at 255 and cuts the loop short.

---

## 🛠 Make your own festival animation

This was built with the free [OLED Animation Maker](https://www.oledanimationmaker.com/). No install, no signup.

1. Open [oledanimationmaker.com](https://www.oledanimationmaker.com/)
2. Import a short clip you have the right to convert
3. Keep 128×64 and a frame count your board can store
4. Click **Get the Code** → Adafruit SSD1306
5. Paste into the IDE and upload

Long version, with wiring, flash math and filming tips:
https://www.oledanimationmaker.com/blog/dussehra-ravana-dahan-oled-animation-arduino-ssd1306.html

---

## 🧯 Troubleshooting

| Problem | Fix |
|---------|-----|
| IDE says the sketch name doesn't match | Rename the folder to `main` |
| Sketch too big | Use a 4MB ESP8266. Uno and Mega won't link |
| Blank screen | 3.3V, reseat SDA/SCL, try `0x3D` |
| Loop jumps back early | `frameIdx` must stay `uint16_t` |
| Slower than 15 fps | `Wire.setClock(400000)` |
| Compile errors | Install both Adafruit libraries |

Blank-screen checklist: [oledanimationmaker.com/blog/arduino-oled-blank-screen-fix-ssd1306.html](https://www.oledanimationmaker.com/blog/arduino-oled-blank-screen-fix-ssd1306.html)

---

## 🎬 Film it so the reel works

- Fill the frame with the **glass**, not the whole desk
- Kill the light behind the phone or the cover glass mirrors the room
- Record **at least 30 seconds** so one full 20.1 s pass completes
- Post it on Dussehra evening, when the effigies burn

---

## 🔥 More festival & song loops

| Project | Frames | Guide |
|---------|-------:|-------|
| [Navratri Garba & Dandiya](https://github.com/itzmeAshish/NavratriGarba) | 390 | [Blog](https://www.oledanimationmaker.com/blog/navratri-garba-dandiya-oled-animation-arduino-ssd1306.html) |
| [Ganesha Day 7](https://github.com/itzmeAshish/GaneshaDay7) | 150 | [Blog](https://www.oledanimationmaker.com/blog/ganesha-day-7-oled-animation-arduino-ssd1306.html) |
| [Sajan Sajan Teri Dulhan](https://github.com/itzmeAshish/SajanSajan) | 338 | [Blog](https://www.oledanimationmaker.com/blog/sajan-sajan-teri-dulhan-tujhko-pukare-aaja-oled-animation-arduino-ssd1306.html) |
| [Batman x Sajan Sajan](https://github.com/itzmeAshish/SajanSajanBatmanVersion) | 768 | [Blog](https://www.oledanimationmaker.com/blog/sajan-sajan-batman-edit-oled-animation-esp8266-ssd1306.html) |
| [Bad Apple](https://github.com/itzmeAshish/BadApple) | 385 | [Blog](https://www.oledanimationmaker.com/blog/bad-apple-oled-animation-arduino-ssd1306.html) |
| Independence Day | drawn show | [Blog](https://www.oledanimationmaker.com/blog/independence-day-oled-animation-esp8266-ssd1306.html) |

---

## ⭐ Liked it?

- **Star the repo.** It's how the next maker finds it
- **Tag [@oled_animator](https://www.instagram.com/oled_animator)** when you build yours
- **Send it to the friend** who'd love Ravana burning on a tiny screen

---

## 🔎 Keywords

`dussehra oled animation` · `ravana dahan arduino` · `vijayadashami oled` · `happy dussehra arduino` · `rama ravana oled` · `dussehra 2026 maker project` · `festival oled animation` · `esp8266 ssd1306` · `progmem bitmap` · `300 frames` · `0.96 oled` · `adafruit ssd1306`

---

## 🔗 Links

- **Code:** https://github.com/itzmeAshish/Dussehra
- **Tutorial:** https://www.oledanimationmaker.com/blog/dussehra-ravana-dahan-oled-animation-arduino-ssd1306.html
- **Tool:** https://www.oledanimationmaker.com/
- **Author:** [itzmeAshish](https://github.com/itzmeAshish)
- **Instagram:** [oled_animator](https://www.instagram.com/oled_animator)
- **YouTube:** [@oled_animator](https://www.youtube.com/@oled_animator)

---

## License

Free to use for learning, demos, festival tables and personal projects.
A star on the repo helps the next person find it during Dussehra.
Credit: link this repo and [oledanimationmaker.com](https://www.oledanimationmaker.com/).

**Happy Dussehra — and may your I2C address be 0x3C. 🏹**

<!--
GitHub repo settings to paste:
Description: 🏹 Dussehra (Ravana Dahan) as a 300-frame pixel-art loop on a 0.96" SSD1306 OLED. Happy Dussehra on ESP8266. Free code.
Topics: dussehra, vijayadashami, ravana-dahan, arduino, esp8266, oled, ssd1306, oled-animation, pixel-art, progmem, adafruit-gfx, festival, maker, diy, arduino-project
-->
