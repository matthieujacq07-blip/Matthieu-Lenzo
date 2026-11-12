# Pointeuse RFID ESP32 – COGIP

Système de pointage d employés : un **ESP32 + lecteur RFID RC522** lit les badges et envoie le scan en Wi-Fi à un **serveur FastAPI** (SQLite) qui enregistre automatiquement les entrées / sorties et propose une interface web.

## Architecture

    Badge RFID -> RC522 -> ESP32 --(Wi-Fi, HTTP POST /api/scan)--> Serveur FastAPI -> SQLite
                                                                          |
                                                                  Interface web (/)

## Contenu du dépôt

| Dossier | Rôle |
|---|---|
| firmware/pointeuse_esp32/ | Code Arduino de l ESP32 (lecture RC522 + envoi HTTP) |
| server/ | Serveur FastAPI + page web + base SQLite |
| docs/ | Compte rendu du projet |

## Matériel

ESP32, module RFID RC522, LED (GPIO 2), badges/cartes MIFARE.

Câblage RC522 -> ESP32 (VSPI) :

| RC522 | ESP32 |
|---|---|
| SDA / SS | GPIO 5 |
| SCK | GPIO 18 |
| MOSI | GPIO 23 |
| MISO | GPIO 19 |
| RST | GPIO 4 |
| 3.3V / GND | 3V3 / GND |

## Installation

### 1. Serveur

    cd server
    py -m pip install -r requirements.txt
    py run_server.py

Page web : http://IP_PC:8000/ – Swagger : http://IP_PC:8000/docs. Détails dans server/README_SERVER.md.

### 2. ESP32
1. Arduino IDE : installer la bibliothèque **MFRC522** et le support carte ESP32.
2. Copier firmware/pointeuse_esp32/config.h.example en config.h et renseigner le Wi-Fi et l IP du PC.
3. Ouvrir pointeuse_esp32.ino et téléverser sur l ESP32.

## API

POST /api/scan avec {"badge_id": "ABC123", "device_id": "ESP32-01"} : alterne automatiquement entrée / sortie.
Autres routes : /api/employees, /api/attendance, /api/status, /api/health.
