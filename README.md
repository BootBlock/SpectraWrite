# SpectraWrite

**AI-Powered Voice Dictation with Hardware-Level Text Entry**

[![.NET](https://img.shields.io/badge/.NET-10.0-512BD4?logo=dotnet)](https://dotnet.microsoft.com/)
[![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11-0078D4?logo=windows)](https://www.microsoft.com/windows)
[![ESP32](https://img.shields.io/badge/Hardware-ESP32--S3-E7352C?logo=espressif)](https://www.espressif.com/)

---

## What is SpectraWrite?

SpectraWrite is a **local-first, privacy-focused voice dictation system** for Windows that transcribes your speech in real-time using state-of-the-art AI (Whisper). Unlike cloud-based alternatives, your voice data never leaves your machine.

### Why Hardware Text Entry?

SpectraWrite includes an optional **hardware companion device** (ESP32-S3) called **SpectraType** that receives transcribed text and emulates a standard USB keyboard. This architecture provides unique advantages:

| Challenge | SpectraWrite + SpectraType Solution |
|-----------|-------------------------------------|
| **Air-gapped systems** | Type on machines with no network access |
| **Security-locked computers** | No software installation required on target |
| **Cross-platform compatibility** | Works on Windows, Mac, Linux—any USB-capable device |
| **KVM-free multi-PC setups** | Dictate to multiple machines without switching keyboards |
| **Legacy/embedded systems** | Voice-enable any device that accepts USB keyboards |

The target machine sees only a standard USB keyboard—completely undetectable and universally compatible.

[INSERT SCREENSHOT HERE: Main window showing VU meter and transcription area]

---

## ✨ Key Features

### Voice Recognition
- 🎤 **Real-time transcription** powered by OpenAI Whisper (runs 100% locally)
- 🚀 **GPU acceleration** with CUDA support (RTX 2060+ recommended)
- 🗣️ **Voice Activity Detection (VAD)** for hands-free operation
- ⌨️ **Push-to-Talk mode** with configurable global hotkey
- 🌍 **Multi-language support** with auto-detection

### Privacy & Security
- 🔒 **Fully offline** — no cloud, no internet required, no telemetry
- 🔐 **HMAC-SHA256 authentication** for hardware communication
- 🛡️ **End-to-end security** with configurable passkeys

### Hardware Integration (SpectraType)
- 📡 **WiFi & Bluetooth connectivity** — station mode, AP mode, or BLE
- ⌨️ **USB HID keyboard emulation** — universal compatibility
- 🎮 **Three typing modes** — instant, natural (human-like), typewriter
- 📺 **Optional OLED display** — real-time status and progress

### User Experience
- 🎨 **Modern WinUI 3 interface** with Mica/Acrylic effects
- 🔌 **Modular plugin architecture** — extend functionality with modules
- 📊 **Usage statistics dashboard** — track your dictation productivity
- 🗨️ **Voice commands** — punctuation, formatting, custom commands
- 📝 **AI text refinement** — optional Ollama LLM integration

---

## 🖥️ System Requirements

### Windows Application

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **OS** | Windows 10 21H1 (64-bit) | Windows 11 |
| **CPU** | Intel Core i5 / Ryzen 5 | Intel Core i7 / Ryzen 7 |
| **RAM** | 8 GB | 16 GB |
| **Storage** | 4 GB (for models) | SSD recommended |
| **GPU** | — | NVIDIA RTX 2060+ (CUDA) |

### Hardware Device (Optional)

| Component | Specification |
|-----------|---------------|
| **Board** | ESP32-S3-DevKitC-1 (recommended) |
| **Alternatives** | Waveshare ESP32-S3-Zero, Seeed XIAO ESP32S3 |
| **USB** | Native USB OTG (HID capable) |
| **Connectivity** | WiFi 802.11 b/g/n, Bluetooth 5.0 |
| **Cost** | ~$10 USD |

> ⚠️ **Important**: Original ESP32 boards (non-S2/S3) do **not** have native USB support and cannot function as USB HID keyboards.

---

## 🚀 Quick Start

### 1. Install the Windows Application

1. Download the latest release from the [Releases page](https://github.com/BootBlock/SpectraWrite/releases)
2. Run the installer or extract the portable ZIP
3. Launch **SpectraWrite**
4. On first run, select and download a Whisper model (start with `small` for balanced performance)

For detailed installation instructions, see the [Installation Guide](docs/guides/endpoint/spectrawrite-setup.md).

### 2. Start Dictating (Software-Only Mode)

1. Click **Start** or press your Push-to-Talk hotkey (default: `Right Shift`)
2. Speak naturally into your microphone
3. Watch your words appear in real-time
4. Release the hotkey to stop dictation

The transcribed text is automatically typed into the active application.

### 3. Hardware Setup (Optional — For Remote Typing)

To enable typing on a separate machine:

1. **Flash your ESP32-S3** with the SpectraType firmware
2. **Configure WiFi** on the device via the web portal
3. **Add the endpoint** in SpectraWrite Settings → Modules → Remote EndPoints
4. **Connect the ESP32** to your target machine via USB
5. **Start dictating** — text flows: Your PC → WiFi → ESP32 → USB Keyboard → Target Machine

For step-by-step hardware setup, see the [Quick Start Guide](docs/guides/endpoint/quick-start.md).

---

## 📚 Documentation

### Getting Started

| Guide | Description | Time |
|-------|-------------|------|
| [Quick Start](docs/guides/endpoint/quick-start.md) | Get the full system running | 15 min |
| [SpectraWrite Setup](docs/guides/endpoint/spectrawrite-setup.md) | Configure endpoints in the app | 10 min |

### Hardware Guides

| Guide | Description |
|-------|-------------|
| [ESP32 Flashing Guide](docs/guides/endpoint/esp32-flashing.md) | Flash SpectraType firmware to your device |
| [ESP32 Firmware Guide](docs/guides/endpoint/esp32-firmware.md) | Complete firmware documentation |
| [OLED Display Setup](docs/guides/endpoint/oled-display.md) | Add an optional status display |
| [Troubleshooting](docs/guides/endpoint/esp32-troubleshooting.md) | Common issues and solutions |

### Reference

| Document | Description |
|----------|-------------|
| [Protocol Specification](docs/guides/endpoint/protocol-specification.md) | Complete HTTP API reference |
| [Security Guide](docs/guides/endpoint/security-guide.md) | Authentication and best practices |
| [Network Configuration](docs/guides/endpoint/network-configuration.md) | WiFi, AP Mode, and Bluetooth setup |

### Developer Guides

| Document | Description |
|----------|-------------|
| [Module Development Guide](docs/MODULE-DEV-GUIDE.md) | Create custom plugins |
| [Module API Reference](docs/MODULE-API.md) | Complete API documentation |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           YOUR WORKSTATION                                  │
│  ┌─────────────────────────────────────────────────────────────────────┐    │
│  │                      SpectraWrite (Windows)                         │    │
│  │  ┌───────────┐   ┌──────────────┐   ┌──────────────┐                │    │
│  │  │ Microphone│──▶│ Audio Capture│──▶│   Whisper    │               │    │
│  │  │    🎤     │   │   (NAudio)   │   │  (GPU/CPU)   │                │    │
│  │  └───────────┘   └──────────────┘   └──────┬───────┘                │    │
│  │                                            │                        │    │
│  │                                            ▼                        │    │
│  │                                    ┌──────────────┐                 │    │
│  │                                    │ Transcribed  │                 │    │
│  │                                    │    Text      │                 │    │
│  │                                    └──────┬───────┘                 │    │
│  │                                           │                         │    │
│  │              ┌────────────────────────────┼────────────────────┐    │    │
│  │              │                            │                    │    │    │
│  │              ▼                            ▼                    ▼    │    │
│  │   ┌──────────────────┐        ┌────────────────┐    ┌─────────────┐ │    │
│  │   │   Local Typing   │        │  WiFi/HTTP to  │    │ Clipboard/  │ │    │
│  │   │  (This Machine)  │        │  SpectraType   │    │  File Save  │ │    │
│  │   └──────────────────┘        └───────┬────────┘    └─────────────┘ │    │
│  │                                       │                             │    │
│  └───────────────────────────────────────┼─────────────────────────────┘    │
│                                          │                                  │
└──────────────────────────────────────────┼──────────────────────────────────┘
                                           │
                                           │ WiFi / Bluetooth
                                           │ (HTTP/JSON)
                                           ▼
                               ┌───────────────────────┐
                               │   SpectraType Device  │
                               │      (ESP32-S3)       │
                               │  ┌─────────────────┐  │
                               │  │ HTTP Server     │  │
                               │  │ Request Queue   │  │
                               │  │ Typing Engine   │  │
                               │  │ USB HID Driver  │  │
                               │  └─────────────────┘  │
                               └───────────┬───────────┘
                                           │ USB Cable
                                           │ (Appears as Keyboard)
                                           ▼
                               ┌───────────────────────┐
                               │    TARGET MACHINE     │
                               │  • Any OS (Win/Mac/   │
                               │    Linux/Embedded)    │
                               │  • No software needed │
                               │  • Air-gapped OK      │
                               └───────────────────────┘
```

---

## 🔧 Building from Source

### Prerequisites

- **Visual Studio 2022** (17.8+) with:
  - .NET 10 SDK
  - Windows App SDK
  - C++ build tools (for native dependencies)
- **Git**
- **(Optional)** NVIDIA CUDA Toolkit 13+ for GPU acceleration
- **(Optional)** PlatformIO for ESP32 firmware

### Build the Windows Application

```bash
git clone https://github.com/BootBlock/SpectraWrite.git
cd SpectraWrite
dotnet restore
dotnet build -c Release
```

The application builds to `!Distribution/` with a flat structure.

### Build the ESP32 Firmware

```bash
cd tools/esp32-spectrawrite

# Choose your board
pio run -e esp32s3-devkitc -t upload    # DevKitC-1
pio run -e esp32s3-zero -t upload       # ESP32-S3-Zero
pio run -e xiao-esp32s3 -t upload       # XIAO ESP32S3
```

For detailed build instructions, see the [ESP32 Firmware Guide](docs/guides/endpoint/esp32-firmware.md).

---

## 🗂️ Project Structure

```
SpectraWrite/
├── src/                          # Windows application source
│   ├── SpectraWrite.UI/          # WinUI 3 application
│   ├── SpectraWrite.Audio/       # Audio capture (NAudio)
│   ├── SpectraWrite.Transcription/ # Whisper.net backend
│   ├── SpectraWrite.Core/        # Core abstractions
│   └── ...                       # Other libraries
├── modules/                      # Plugin modules
│   ├── Module.EndPoint/          # Remote endpoint support
│   ├── Module.Overlay/           # Floating overlay
│   ├── Module.Dashboard/         # Statistics dashboard
│   └── ...                       # Other modules
├── tools/
│   └── esp32-spectrawrite/       # ESP32 SpectraType firmware
├── docs/
│   └── guides/endpoint/          # Hardware setup documentation
└── tests/                        # Unit and UI tests
```

---

## 🤝 Contributing

Contributions are welcome! Please read our contributing guidelines and submit pull requests to the `main` branch.

### Module Development

SpectraWrite's modular architecture makes it easy to extend. See the [Module Development Guide](docs/MODULE-DEV-GUIDE.md) to create your own plugins.

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgements

- [Whisper.net](https://github.com/sandrohanea/whisper.net) — .NET bindings for OpenAI Whisper
- [NAudio](https://github.com/naudio/NAudio) — Audio capture library
- [Windows App SDK](https://github.com/microsoft/WindowsAppSDK) — Modern Windows UI framework
- [ESP-IDF](https://github.com/espressif/esp-idf) & [Arduino-ESP32](https://github.com/espressif/arduino-esp32) — ESP32 development
- [TinyUSB](https://github.com/hathach/tinyusb) — USB HID implementation

---

<div align="center">

**[Documentation](docs/guides/endpoint/README.md)** · **[Report Bug](https://github.com/BootBlock/SpectraWrite/issues)** · **[Request Feature](https://github.com/BootBlock/SpectraWrite/issues)**

Made with ❤️ for the voice-input community

</div>
