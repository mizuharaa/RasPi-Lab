Host a Wi-Fi Hotspot with a Raspberry Pi

This guide turns a Raspberry Pi into a private Wi-Fi hotspot (AP) that you carry to hotels/guest Wi-Fi. The Pi connects to the venue’s Wi-Fi once, then shares that connection over its own hotspot so all your devices join your Pi’s network automatically. Based on Raspberry Pi’s official tutorial. 
Raspberry Pi

Hardware & OS

Raspberry Pi with built-in Wi-Fi (e.g., Pi 4/5) plus a USB Wi-Fi adapter (you need two Wi-Fi interfaces: one for upstream, one for the AP). If your Pi has no built-in Wi-Fi, you’ll need two USB adapters; at least one must support AP mode. 
Raspberry Pi

Raspberry Pi OS Lite (headless is fine). 
Raspberry Pi

In Raspberry Pi Imager, set a hostname (e.g., pi-hotspot), create username/password, enable Configure wireless LAN, and enable SSH with password auth. 
Raspberry Pi

1) First boot & connect via SSH

After imaging and first boot (with your USB Wi-Fi adapter inserted):

# From your laptop:
ssh <username>@pi-hotspot.local
# Enter the password you set in Raspberry Pi Imager


If .local doesn’t resolve, use the Pi’s IP address. 
Raspberry Pi

2) Identify Wi-Fi devices

You’ll see two Wi-Fi interfaces (names may vary):

nmcli device


Look for something like:

DEVICE   TYPE  STATE          CONNECTION
wlan1    wifi  connected      Example Wi-Fi   # USB adapter (upstream)
wlan0    wifi  disconnected   --              # built-in (will become hotspot)


Typically, wlan0 is the built-in Wi-Fi; wlan1 is your USB adapter. 
Raspberry Pi

3) Create the hotspot on wlan0

Create and start the hotspot (replace placeholders):

sudo nmcli device wifi hotspot ssid <hotspot name> password <hotspot password> ifname wlan0


This brings up an AP immediately on wlan0. Connect your laptop/phone to the new SSID using the password you chose. 
Raspberry Pi

Check saved connections:

nmcli connection
# Look for a connection named "Hotspot" on wlan0


Raspberry Pi

4) Auto-start hotspot at boot (priority)

Make the hotspot auto-start and take priority over other saved Wi-Fi profiles:

# Get the Hotspot connection UUID
nmcli connection

# Show properties (replace with your hotspot UUID)
nmcli connection show <hotspot UUID>

# Enable autoconnect and set a high priority (e.g., 100)
sudo nmcli connection modify <hotspot UUID> connection.autoconnect yes connection.autoconnect-priority 100

# Verify
nmcli connection show <hotspot UUID>
# Expect:
# connection.autoconnect: yes
# connection.autoconnect-priority: 100


Raspberry Pi

5) Install the browser-based Wi-Fi portal (Flask)

This tiny web app lets you pick the upstream Wi-Fi (hotel/guest SSID) from a dropdown and connect wlan1 to it.

sudo apt update
sudo apt install -y python3-flask
mkdir -p ~/wifi-portal
cd ~/wifi-portal
sudo nano app.py


Paste the official app content (uses wlan1 as the upstream interface) and save. 
Raspberry Pi

Run it for a quick test:

sudo python3 app.py
# Access from a device on your hotspot: http://pi-hotspot.local
# Stop with Ctrl+C when done testing


To start it automatically at boot using cron:

crontab -e
# Choose nano, then add the line below (replace <username>):
@reboot sudo python3 /home/<username>/wifi-portal/app.py
sudo reboot


Raspberry Pi

6) Using the hotspot (at a hotel / guest Wi-Fi)

Plug in your Pi.

Connect your device to the Pi’s hotspot SSID.

On that device, open http://pi-hotspot.local and pick the hotel Wi-Fi SSID (enter password if needed) — this connects wlan1 to the venue network.

If the venue uses a captive portal, open http://captive.apple.com and complete the login. (You may need to repeat this every 12–48 hours depending on the venue.) 
Raspberry Pi

Commands Reference (copy-paste)
# SSH into the Pi
ssh <username>@pi-hotspot.local

# List devices
nmcli device

# Create hotspot on wlan0
sudo nmcli device wifi hotspot ssid <hotspot name> password <hotspot password> ifname wlan0

# Show all connections
nmcli connection

# Inspect hotspot properties
nmcli connection show <hotspot UUID>

# Autostart hotspot with high priority
sudo nmcli connection modify <hotspot UUID> connection.autoconnect yes connection.autoconnect-priority 100

# Install Flask & create portal
sudo apt update
sudo apt install -y python3-flask
mkdir -p ~/wifi-portal
cd ~/wifi-portal
sudo nano app.py   # paste code from the tutorial

# Run the portal manually
sudo python3 app.py

# Autostart the portal (cron @reboot)
crontab -e
# add:
# @reboot sudo python3 /home/<username>/wifi-portal/app.py
sudo reboot

Notes & Tips

Interface names (wlan0/wlan1) may differ depending on your adapters; adjust the commands accordingly. 
Raspberry Pi

At least one Wi-Fi interface must support AP mode for the hotspot. 
Raspberry Pi

Keep your Pi updated (sudo apt update && sudo apt full-upgrade -y).
