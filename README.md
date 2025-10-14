# RasPi-Lab
# 🧰 Featuring all of my Raspberry Pi Projects!! (RPi Collection)

A curated collection of my Raspberry Pi builds — each project lives in its own folder with code, setup notes, and gotchas. Built for learning, reuse, and future me 🧠.

[![Made with Python](https://img.shields.io/badge/Python-3.x-informational)]()
[![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-Projects-red)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)]()

---

## 📁 Projects Index
- **[rpi5-hotspot](./rpi5-hotspot)** — Flask “captive portal” + Wi-Fi hotspot router on Raspberry Pi 5 (NAT, IP forwarding, mDNS support).
- *(More coming soon: camera streamer, sensor kits, home automation, etc.)*

---

## 🏗️ Repo Structure
raspberry-pi-projects/
├─ rpi5-hotspot/ # The hotspot portal project (RPi 5)
│ ├─ app.py # Flask app (serves portal on :80)
│ ├─ requirements.txt # Python deps
│ ├─ README.md # Project-specific docs
│ ├─ templates/ # HTML (if used)
│ └─ static/ # CSS/JS/assets (if used)
├─ docs/ # Screenshots, diagrams shared across projects
├─ .gitignore
└─ README.md # (this file)
