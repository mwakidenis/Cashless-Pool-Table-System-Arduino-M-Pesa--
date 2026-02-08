# 🎱 Cashless Pool Table System (Arduino + M-Pesa)

A fully automated **cashless pool table system** that replaces traditional coin mechanisms with **M-Pesa payments**.  
Each successful payment of **KES 20** unlocks the pool table for **one game only**, after which the table locks automatically.

No coins. No manual handling. Fully automated.

---

## 💰 Pricing Model

| Amount | Access |
|------|--------|
| **KES 20** | **1 Pool Game** |

- Only payments of exactly **KES 20** are accepted
- Each payment unlocks the table **once**
- A new payment is required for every new game

---

## 🧠 System Architecture
Player │ ▼ M-Pesa STK Push (KES 20) │ ▼ Backend Server (Daraja API) │ ▼ ESP32 (Wi-Fi) │ ▼ Arduino │ ▼ Relay → Solenoid / Motor Lock → Pool Table
Copy code

---

## 🔁 Operational Flow

1. Player initiates payment (button, keypad, or QR code)
2. Backend sends an **M-Pesa STK Push** for KES 20
3. Player enters M-Pesa PIN
4. Safaricom sends payment confirmation callback
5. Backend validates the transaction
6. Backend sends unlock approval to ESP32
7. ESP32 sends unlock command to Arduino
8. Arduino unlocks the pool table
9. After one game, the table locks again

---

## 🧩 Hardware Components

| Component | Description |
|--------|------------|
| Arduino Uno / Nano | Controls lock and game logic |
| ESP32 | Wi-Fi communication with backend |
| Relay Module (5V) | Switches power to locking mechanism |
| Solenoid / DC Motor Lock | Physical table lock |
| Button / Keypad | Payment initiation |
| LCD / OLED Display | Status messages |
| Power Supply | 12V (lock) + 5V (logic) |

---

## 🖥 Software Stack

### Microcontrollers
- Arduino (C/C++)
- ESP32 Wi-Fi firmware

### Backend
- Node.js (Express) or Python (Flask/FastAPI)
- Safaricom **Daraja API**
- HTTPS-enabled server

---

## 📡 M-Pesa Integration

### Payment Method
- **Lipa Na M-Pesa Online (STK Push)**

### Credentials Used
- Consumer Key
- Consumer Secret
- Shortcode
- Passkey
- Callback URL

M-Pesa credentials are stored and used **only on the backend server**.

---

## 🗂 Backend Responsibilities

- Initiate STK Push for **KES 20**
- Receive and process Safaricom callbacks
- Validate transaction amount and status
- Ensure each transaction is used once
- Send unlock authorization to ESP32

---

## 📡 ESP32 Responsibilities

- Connect to configured Wi-Fi network
- Communicate with backend server
- Receive unlock authorization
- Send unlock command to Arduino via serial communication

### ESP32 Logic (Concept)

```cpp
if (unlockApproved) {
  Serial.println("UNLOCK");
}
🔌 Arduino Responsibilities
Listen for serial commands from ESP32
Control relay and locking mechanism
Unlock table for one game
Lock table after game completion
Arduino Logic (Concept)
Copy code
Cpp
if (command == "UNLOCK") {
  unlockTable();
  waitForGameEnd();
  lockTable();
}
⏱ Game End Detection
A game can be considered complete using one of the following:
Fixed timer duration
Ball return sensor
Manual reset button
Inactivity timeout

🔐 Security Model
The backend server is the single authority for unlocking the pool table
The ESP32 cannot unlock the table independently
Each M-Pesa transaction ID is validated and used once
All backend communication is performed over HTTPS
The ESP32 accepts commands only from the trusted backend
🛠 Installation Steps

Install solenoid or motor lock under the pool table
Connect relay module to Arduino
Connect ESP32 to Arduino using UART
Deploy backend server and configure Daraja credentials
Flash firmware to ESP32 and Arduino
Test payment, unlock, and automatic relock
📊 Optional Enhancements

QR code payment initiation
Game counter display
Admin dashboard
Usage analytics
Multiple pool table support
SMS payment confirmation
✅ Advantages

Eliminates coin theft
Fixed and transparent pricing
Fully automated operation
Low maintenance
Scalable to multiple tables
