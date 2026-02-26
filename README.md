# Secure Wireless Communication System
### ESP32 + nRF24L01 + AES-128 Encryption

A secure wireless communication system built using two ESP32 microcontrollers and nRF24L01 radio modules. Messages are encrypted with AES-128 before transmission and displayed on a 16x2 LCD at the receiver. A web interface allows sending messages from any browser on the same WiFi network.

---

## Components Required

| Component | Quantity |
|---|---|
| ESP32 Dev Module | 2 |
| nRF24L01+ Module | 2 |
| nRF24L01 3.3V Adapter | 2 |
| 16x2 I2C LCD Display | 1 |
| Jumper Wires | Several |
| USB Cables for ESP32 | 2 |

---

## Wiring

### nRF24L01 Adapter → ESP32 (Both Transmitter & Receiver)

| nRF24L01 Adapter | ESP32 Board Pin |
|---|---|
| VCC | 3.3V |
| GND | GND |
| CE | D4 |
| CSN | D5 |
| SCK | D18 |
| MOSI | D23 |
| MISO | D19 |

### LCD 16x2 I2C → ESP32 (Receiver Only)

| LCD Pin | ESP32 Board Pin |
|---|---|
| VCC | VIN (5V) |
| GND | GND |
| SDA | D21 |
| SCL | D22 |

---

## Libraries Required

Install these via **Arduino IDE → Sketch → Include Library → Manage Libraries:**

| Library | Author |
|---|---|
| RF24 | TMRh20 |
| LiquidCrystal I2C | Frank de Brabander |
| AESLib | Matej Sychra |

---

## How It Works

```
[Laptop Browser]
      |
      | WiFi (HTTP)
      |
[Transmitter ESP32]
      |  Type message in web UI
      |  AES-128 encrypt message
      |  Send over nRF24L01 (2.4GHz RF)
      |
[Receiver ESP32]
      |  Receive encrypted packet
      |  AES-128 decrypt message
      |  Display on 16x2 LCD
```

### Security
- Messages are encrypted using **AES-128-CBC** before being sent over the air
- Anyone intercepting the RF signal only sees encrypted bytes
- Only the receiver with the matching secret key can decrypt the message

---

## Transmitter

The transmitter ESP32:
- Connects to your WiFi network
- Hosts a web server at its local IP address
- You open the web page in any browser on the same network
- Type a message (max 16 characters) and click Send
- The message is AES-128 encrypted and transmitted via nRF24L01

---

## Receiver

The receiver ESP32:
- Listens for incoming RF packets on channel 76
- Decrypts each received packet using AES-128
- Displays the decrypted message on the 16x2 LCD
- Shows scrolling text for messages longer than 16 characters

---

## Radio Configuration

Both transmitter and receiver must have matching radio settings:

| Setting | Value |
|---|---|
| Channel | 76 |
| Data Rate | RF24_1MBPS |
| PA Level | RF24_PA_MIN |
| CRC | RF24_CRC_16 |
| Address | "SECR1" |

---

## Important Notes

- Keep both ESP32s within range during testing (start within 1 meter)
- Always power nRF24L01 through the 3.3V adapter, never directly from 5V
- Laptop and transmitter ESP32 must be on the same WiFi network
- Message length is limited to 16 characters (one AES block)
