# 🔐 Waybar WireGuard VPN Toggle (Rust)

A lightweight, fast, and minimal Rust-based utility for managing WireGuard VPNs via NetworkManager (`nmcli`). Built for integration into Waybar as a `custom` module, it allows users to:

- Toggle VPN on/off with a click
- Rotate between configured VPNs via scroll or right-click
- Display current VPN status in Waybar
- Output proper JSON for color and icon styling

---

## 📦 Features

- 🚀 Written in Rust for speed and safety
- 🧠 Remembers current VPN between reboots (`/tmp/wg-current`)
- 🖱️ Scroll or right-click to cycle through VPN configs
- 🎨 Emits Waybar-compatible JSON for visual theming

---

## 🛠 Dependencies

- [`NetworkManager`](https://wiki.archlinux.org/title/NetworkManager)
- WireGuard support via `nmcli` (import `.conf` files with `nmcli connection import`)
- [`waybar`](https://github.com/Alexays/Waybar) with `custom` module support

---

## 🔧 Installation

### 1. Clone and build

```bash
git clone https://github.com/codemonkey76/wg-toggle
cd wg-toggle
cargo build --release
```

### 2. Copy binary to somehwere in your `$PATH`:

```bash
cp target/release/wg-toggle ~/.local/bin/
```

### Using cargo
```bash
cargo install wg-toggle
```

> In this case, the binary is stored in `~/.cargo/bin/`. So be sure to point the module to `~/.cargo/bin/wg-toggle`.

## 🖼️ Waybar Integration

### Example module config:

```json
"custom/wg": {
  "exec": "$HOME/.cargo/bin/wg-toggle --status",
  "return-type": "json",
  "interval": 5,
  "on-click": "$HOME/.cargo/bin/wg-toggle",
  "on-click-right": "$HOME/.cargo/bin/wg-toggle next",
  "on-scroll-up": "$HOME/.cargo/bin/wg-toggle previous",
  "on-scroll-down": "$HOME/.cargo/bin/wg-toggle next",
  "format": "{icon}",
  "format-icons": {
	   "disconnected": " ",
	   "connected": " "
  },
  "tooltip-format": "{} - {alt}"
},
```

### Example CSS:

```css
#custom-wg.connected {
  background-color: #a6e3a1;
  color: #1e1e2e;
}

#custom-wg.disconnected {
  background-color: #45475a;
  color: #cdd6f4;
}
```

## ⚙️ Behavior

| Action           | Result                        |
|------------------|-------------------------------|
| Left Click       | Toggle current VPN on/off     |
| Scroll Up        | Switch to previous VPN config |
| Scroll Down      | Switch to next VPN config     |
| Right Click      | Switch to next VPN config     |
| Auto-refresh     | Show current VPN status       |

## 📁 VPN Configuration

Use `nmcli` to import `.conf` files:

```bash
nmcli connection import type wireguard file wg-home.conf
nmcli connection import type wireguard file wg-work.conf
```

Names must be valid Linux interface names (`<=15` characters, no spaces).

## 🧪 Debugging

To test the output:

```bash
~/.local/bin/wg-toggle --status
~/.local/bin/wg-toggle
~/.local/bin/wg-toggle next
```

## 🪪 License

MIT

## 🤝 Contributing

Pull requests and suggestions are very welcome!  
If you'd like to improve the script, support more VPN managers, or enhance the Waybar integration, feel free to open an issue or PR.

Please ensure any code contributions:
- Are formatted with `rustfmt`
- Include basic error handling
- Preserve existing CLI behavior
