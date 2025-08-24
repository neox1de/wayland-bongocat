# Bongo Cat Wayland Overlay

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.2.4-blue.svg)](https://github.com/saatvik333/wayland-bongocat/releases)

A delightful Wayland overlay that displays an animated bongo cat reacting to your keyboard input! Perfect for streamers, content creators, or anyone who wants to add some fun to their desktop.

![Demo](assets/demo.gif)

## ✨ Features

- **🎯 Real-time Animation** - Bongo cat reacts instantly to keyboard input
- **🔥 Hot-Reload Configuration** - Modify settings without restarting (v1.2.0)
- **🔄 Dynamic Device Detection** - Automatically detects Bluetooth/USB keyboards (v1.2.0)
- **⚡ Performance Optimized** - Adaptive monitoring and batch processing (v1.2.0)
- **🖥️ Screen Detection** - Automatic screen detection for all sizes and orientations (v1.2.2)
- **🎮 Smart Fullscreen Detection** - Automatically hides during fullscreen applications (v1.2.3)
- **🖥️ Multi-Monitor Support** - Choose which monitor to display on in multi-monitor setups (v1.2.4)
- **💾 Lightweight** - Minimal resource usage (~7MB RAM)
- **🎛️ Multi-device Support** - Monitor multiple keyboards simultaneously
- **🏗️ Cross-platform** - Works on x86_64 and ARM64

## 🚀 Installation

### Arch Linux (Recommended)

```bash
# Using yay
yay -S bongocat

# Using paru
paru -S bongocat

# Run immediately
bongocat --watch-config

# Custom config with hot-reload
bongocat --config ~/.config/bongocat.conf --watch-config
```

### Other Distributions

<details>
<summary>Ubuntu/Debian</summary>

```bash
# Install dependencies
sudo apt install libwayland-dev wayland-protocols build-essential

# Build from source
git clone https://github.com/saatvik333/wayland-bongocat.git
cd wayland-bongocat
make

# Run
./build/bongocat
```

</details>

<details>
<summary>Fedora</summary>

```bash
# Install dependencies
sudo dnf install wayland-devel wayland-protocols-devel gcc make

# Build from source
git clone https://github.com/saatvik333/wayland-bongocat.git
cd wayland-bongocat
make

# Run
./build/bongocat
```

</details>

<details>
<summary>NixOS</summary>

```bash
# Quick start with flakes
nix run github:saatvik333/wayland-bongocat -- --watch-config

# Install to user profile
nix profile install github:saatvik333/wayland-bongocat
```

📖 **For comprehensive NixOS setup, see [nix/NIXOS.md](nix/NIXOS.md)**

</details>

## 🎮 Quick Start

### 1. Setup Permissions

```bash
# Add your user to the input group
sudo usermod -a -G input $USER
# Log out and back in for changes to take effect
```

### 2. Find Your Input Devices

```bash
# If installed via AUR
bongocat-find-devices

# If built from source
./scripts/find_input_devices.sh
```

### 3. Run with Hot-Reload

```bash
# AUR installation
bongocat --watch-config

# From source
./build/bongocat --watch-config
```

## ⚙️ Configuration

Bongo Cat uses a simple configuration file format. With hot-reload enabled (`--watch-config`), changes apply instantly without restarting.

### Basic Configuration

Create or edit `bongocat.conf`:

```ini
# Visual settings
cat_height=50                    # Size of bongo cat (16-128)
cat_x_offset=0                   # Horizontal position offset
cat_y_offset=0                   # Vertical position offset
mirror_x=0                       # Flip horizontally (mirror across Y axis)
mirror_y=0                       # Flip vertically (mirror across X axis)
overlay_opacity=150              # Background opacity (0-255)
overlay_position=top             # Position on screen (top/bottom) # Note: hot-reload does not work for this option, requires a restart
visible_in_fullscreen=0          # Keep overlay visible when an app is fullscreen (0=hide, 1=show)

# Animation settings
fps=60                           # Frame rate (1-120)
keypress_duration=100            # Animation duration (ms)
test_animation_interval=3        # Test animation every N seconds (0=off)

# Input devices (add multiple lines for multiple keyboards)
keyboard_device=/dev/input/event4
keyboard_device=/dev/input/event20  # External/Bluetooth keyboard

# Multi-monitor support
monitor=eDP-1                    # Specify which monitor to display on (optional)

# Debug
enable_debug=1                   # Show debug messages
```
### Configuration Reference

| Setting                   | Type    | Range             | Default             | Description                                   |
| ------------------------- | ------- | ----------------- | ------------------- | --------------------------------------------- |
| `cat_height`              | Integer | 16-128            | 50                  | Height of bongo cat in pixels                 |
| `cat_x_offset`            | Integer | -9999 to 9999     | 0                   | Horizontal offset from center                 |
| `cat_y_offset`            | Integer | -9999 to 9999     | 0                   | Vertical offset from center                   |
| `overlay_opacity`         | Integer | 0-255             | 150                 | Background opacity (0=transparent)            |
| `mirror_x`                | Boolean | 0 or 1            | 0                   | Flip cat horizontally (mirror across Y axis)  |
| `mirror_y`                | Boolean | 0 or 1            | 0                   | Flip cat vertically (mirror across X axis)    |
| `visible_in_fullscreen`   | Boolean | 0 or 1            | 0                   | Keep overlay visible during fullscreen        |
| `overlay_position`        | String  | "top" or "bottom" | "top"               | Position of overlay on screen                 |
| `fps`                     | Integer | 1-120             | 60                  | Animation frame rate                          |
| `keypress_duration`       | Integer | 50-5000           | 100                 | Animation duration after keypress (ms)        |
| `test_animation_interval` | Integer | 0-60              | 3                   | Test animation interval (seconds, 0=disabled) |
| `keyboard_device`         | String  | Valid path        | `/dev/input/event4` | Input device path (multiple allowed)          |
| `monitor`                 | String  | Monitor name      | Auto-detect         | Monitor to display on (e.g., "eDP-1", "HDMI-A-1") |
| `enable_debug`            | Boolean | 0 or 1            | 0                   | Enable debug logging                          |

## 🔧 Usage

### Command Line Options

```bash
bongocat [OPTIONS]

Options:
  -h, --help         Show this help message
  -v, --version      Show version information
  -c, --config       Specify config file (default: bongocat.conf)
  -w, --watch-config Watch config file for changes and reload automatically
  --toggle           Toggle bongocat on/off (start if not running, stop if running)
```

### Examples

```bash
# Basic usage
bongocat

# With hot-reload (recommended)
bongocat --watch-config

# Custom config with hot-reload
bongocat --config ~/.config/bongocat.conf --watch-config

# Debug mode
bongocat --watch-config --config bongocat.conf

# Toggle mode
bongocat --toggle
```

## 🛠️ Building from Source

### Prerequisites

**Required:**

- Wayland compositor with layer shell support
- C11 compiler (GCC 4.9+ or Clang 3.4+)
- Make
- libwayland-client
- wayland-protocols
- wayland-scanner
### Build Process

```bash
# Clone repository
git clone https://github.com/saatvik333/wayland-bongocat.git
cd wayland-bongocat

# Build (production)
make

# Build (debug)
make debug

# Clean
make clean
```

The build process automatically:

1. Generates Wayland protocol files
2. Compiles with optimizations and security hardening
3. Embeds assets directly in the binary
4. Links with required libraries

## 🔍 Device Discovery

The `bongocat-find-devices` tool provides professional input device analysis with a clean, user-friendly interface:

```bash
$ bongocat-find-devices

╔══════════════════════════════════════════════════════════════════╗
║ Wayland Bongo Cat - Input Device Discovery v1.2.4                ║
╚══════════════════════════════════════════════════════════════════╝

[SCAN] Scanning for input devices...

[DEVICES] Found Input Devices:
┌─────────────────────────────────────────────────────────────────┐
│ Device: AT Translated Set 2 keyboard                            │
│ Path:   /dev/input/event4                                       │
│ Type:   Keyboard                                                │
│ Status: [OK] Accessible                                         │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│ Device: Logitech MX Keys                                        │
│ Path:   /dev/input/event20                                      │
│ Type:   Keyboard (Bluetooth)                                    │
│ Status: [OK] Accessible                                         │
└─────────────────────────────────────────────────────────────────┘

[CONFIG] Configuration Suggestions:
Add these lines to your bongocat.conf:

keyboard_device=/dev/input/event4   # AT Translated Set 2 keyboard
keyboard_device=/dev/input/event20  # Logitech MX Keys
```

### Advanced Features

```bash
# Show all input devices (including mice, touchpads)
bongocat-find-devices --all

# Generate complete configuration file
bongocat-find-devices --generate-config > bongocat.conf

# Test device responsiveness (requires root)
sudo bongocat-find-devices --test

# Show detailed device information
bongocat-find-devices --verbose

# Get help and usage information
bongocat-find-devices --help
```

### Key Features

- **Smart Detection** - Automatically identifies keyboards vs other input devices
- **Device Classification** - Distinguishes between built-in, Bluetooth, and USB keyboards
- **Permission Checking** - Verifies device accessibility and provides fix suggestions
- **Config Generation** - Creates ready-to-use configuration snippets
- **Device Testing** - Integrated evtest functionality for troubleshooting
- **Professional UI** - Clean, colorized output with status indicators
- **Error Handling** - Comprehensive error messages and troubleshooting guidance

## 📊 Performance

### System Requirements

- **CPU:** Any modern x86_64 or ARM64 processor
- **RAM:** ~7MB runtime usage
- **Storage:** ~0.4MB executable size
- **Compositor:** Wayland with layer shell protocol support

### Performance Metrics (v1.2.4)

- **Input Latency:** <1ms with batch processing
- **CPU Usage:** <1% on modern systems
- **Device Monitoring:** Adaptive 5-30 second intervals
- **Memory:** Optimized with leak detection
- **Fullscreen Detection:** Intelligent hiding with minimal overhead

### Tested Compositors

- ✅ **Hyprland** - Full support
- ✅ **Sway** - Full support
- ✅ **Wayfire** - Compatible
- ✅ **KDE Wayland** - Compatiable
- ❌ **GNOME Wayland** - Support Unknown

## 🐛 Troubleshooting

### Common Issues

<details>
<summary>Permission denied accessing /dev/input/eventX</summary>

**Solution:**

```bash
# Add user to input group (recommended)
sudo usermod -a -G input $USER
# Log out and back in

# Or create udev rule
echo 'KERNEL=="event*", GROUP="input", MODE="0664"' | sudo tee /etc/udev/rules.d/99-input.rules
sudo udevadm control --reload-rules
```

</details>

<details>
<summary>Keyboard input not detected</summary>

**Diagnosis:**

```bash
# Find correct device
bongocat-find-devices

# Test device manually
sudo evtest /dev/input/event4
```

**Solution:** Update `keyboard_device` in `bongocat.conf` with correct path.

</details>

<details>
<summary>Overlay not visible or clickable</summary>

**Check:**

- Ensure compositor supports `wlr-layer-shell-unstable-v1`
- Verify `WAYLAND_DISPLAY` environment variable is set
- Try different `overlay_opacity` values

**Tested compositors:** Hyprland, Sway, Wayfire

</details>

<details>
<summary>Multi-monitor setup issues</summary>

**Finding monitor names:**

```bash
# Using wlr-randr (recommended)
wlr-randr

# Using swaymsg (Sway only)
swaymsg -t get_outputs

# Check bongocat logs for detected monitors
bongocat --watch-config  # Look for "xdg-output name received" messages
```

**Configuration:**

```ini
# Specify exact monitor name
monitor=eDP-1        # Laptop screen
monitor=HDMI-A-1     # External HDMI monitor
monitor=DP-1         # DisplayPort monitor
```

**Troubleshooting:**

- If monitor name is wrong, bongocat falls back to first available monitor
- Monitor names are case-sensitive
- Remove or comment out `monitor=` line to use auto-detection
</details>

<details>
<summary>Build errors</summary>

**Common fixes:**

- Install development packages: `libwayland-dev wayland-protocols`
- Ensure C11 compiler: GCC 4.9+ or Clang 3.4+
- Install `wayland-scanner` package
</details>

### Getting Help

1. Enable debug logging: `bongocat --watch-config` (ensure `enable_debug=1`)
2. Check compositor compatibility
3. Verify all dependencies are installed
4. Test with minimal configuration

## 🏗️ Architecture

### Project Structure

```
wayland-bongocat/
├── src/                 # Source code
│   ├── main.c          # Application entry point
│   ├── config.c        # Configuration management
│   ├── config_watcher.c # Hot-reload system (v1.2.1)
│   ├── input.c         # Input device monitoring
│   ├── wayland.c       # Wayland protocol handling
│   └── ...
├── include/            # Header files
├── scripts/            # Build and utility scripts
├── assets/             # Animation frames
├── protocols/          # Generated Wayland protocols
└── nix/               # NixOS integration
```

### Key Features (v1.2.4)

- **Screen Detection** -> Automatic screen width/orientation detection
- **Fullscreen Detection** -> Smart hiding during fullscreen applications
- **Enhanced Artwork** -> Custom-drawn animations with improved visual quality
- **Multi-Monitor Support** -> Choose specific monitor for display in multi-monitor setups

## 🤝 Contributing

This project follows industry best practices with a modular architecture. Contributions are welcome!

### Development Setup

```bash
git clone https://github.com/saatvik333/wayland-bongocat.git
cd wayland-bongocat
make debug
```

### Code Standards

- C11 standard compliance
- Comprehensive error handling
- Memory safety with leak detection
- Extensive documentation

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

Built with ❤️ for the Wayland community. Special thanks to:

- Redditor: [u/akonzu](https://www.reddit.com/user/akonzu/) for the inspiration
- [@Shreyabardia](https://github.com/Shreyabardia) for the beautiful custom-drawn bongo cat artwork
- All the contributors and users

---

**₍^. .^₎ Wayland Bongo Cat Overlay v1.2.4** - Making desktops more delightful, one keystroke at a time!
