Raspberry Pi Kiosk Mode (Chromium) — RPi 5 / Bookworm

Turn your Raspberry Pi into a dedicated kiosk that boots directly into Chromium (full-screen), loading one or more URLs automatically. This repo includes scripts and autostart files for both Wayland (labwc/wayfire) and Legacy X11 (LXDE) sessions on Raspberry Pi OS Bookworm.

✅ Works offline/online, supports multiple tabs, optional tab rotation with specifically set timer, and avoids “Chromium didn’t shut down correctly” prompts and other warnings.

✨ Features
Auto-login to desktop and launch Chromium in kiosk (full-screen)
Open multiple URLs in separate tab
Optional tab rotation (extension or script; script requires X11)
Avoids Chromium profile lock and unclean shutdown prompts
Screen-blanking disabled (Wayland: raspi-config, X11: xset)
Clean factory-reset instructions

📦 Repository Structure:
├─ README.md
├─ scripts/
│  ├─ install_kiosk.sh
│  ├─ kiosk-autostart.sh
│  └─ switchtabs_x11.sh
├─ autostart/
│  ├─ kiosk.desktop                 # XDG autostart (works on Bookworm desktop sessions)
│  ├─ labwc_autostart               # Wayland (labwc) autostart file
│  └─ lxde_autostart                # Legacy X11 (LXDE) autostart file
├─ .gitignore
└─ LICENSE

🧰 Requirements:
Raspberry Pi 4 model or later
Raspberry Pi OS Bookworm (Desktop)
Network access (for online URLs)
A display/HDMI and keyboard for first-time setup

🚀 Quick Start (Recommended: Wayland + XDG Autostart)
This uses a modern .desktop autostart that works well across sessions.

**Clone this repo to your Pi:
**git clone <YOUR_REPO_URL> rpi-kiosk
cd rpi-kiosk

Install Chromium and enable desktop autologin:
sudo apt update
sudo apt install -y chromium-browser
sudo raspi-config   # System Options → Boot / Auto Login → Desktop Autologin

Disable screen blanking (Wayland path):
sudo raspi-config  # Display Options → Screen Blanking → Disable
Set your URLs in autostart/kiosk.desktop (the Exec= line). Example:
Exec=/usr/bin/chromium-browser --kiosk 'https://www.popmart.com/us' 'https://us.jellycat.com/' --no-first-run --noerrdialogs --disable-infobars --start-maximized --user-data-dir=/home/%u/.kiosk-profile

Install the autostart:
mkdir -p ~/.config/autostart
cp autostart/kiosk.desktop ~/.config/autostart/

Reboot:
sudo reboot

**🔁 Tab Rotation Options
**Wayland (recommended): install a Chromium extension like Tab Carousel / Revolver Tabs.
Start Chromium once without kiosk: chromium-browser
Install extension & set dwell time (e.g., 10–15 s)
Reboot to kiosk
X11 (script): use scripts/switchtabs_x11.sh to send Ctrl+Page_Down every N seconds.

Factory reset option:
pkill -9 -f chromium
pkill -9 -f chromium-browser
rm -rf ~/.config/chromium ~/.config/chromium-browser ~/.cache/chromium ~/.cache/chromium-browser

