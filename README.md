# 🚦 4-Way Traffic Light System (ATmega32)

A specialized embedded systems project that manages a four-way crossroads intersection using the **ATmega32** microcontroller. This project simulates real-world traffic logic by pairing opposite roads and utilizing a precise countdown sequence.

---

## ⚙️ Logic & Operation
The system controls four traffic light poles. To ensure safe traffic flow, the roads are paired: **Road A (North-South)** and **Road B (East-West)**.

* **Synchronized Pairing:** Two opposite roads are **Green** while the other two are **Red**.
* **The Countdown:** Every phase starts from **40** and counts down to **0**.
* **Transition Phase:** * **From 40 down to 5:** The light stays **Green**.
    * **When the timer hits 4:** The Green light turns off and the **Yellow** light runs.
    * **When the timer hits 0:** The **Red** light turns on.
* **The Swap:** Once one pair completes its cycle, it switches to Red, and the opposite pair immediately starts its Green cycle from 40.

---

## 📊 Phase Sequence Table

| Timer Value | Road A (North/South) | Road B (East/West) |
| :--- | :--- | :--- |
| **40 to 5** | 🟢 Green | 🔴 Red |
| **4 to 1** | 🟡 Yellow | 🔴 Red |
| **0** | 🔴 Red | 🟢 Green (Start 40) |

---

## 🛠️ Features
* **Microcontroller:** ATmega32 (AVR Architecture).
* **Timing:** Precision countdown logic from 40 to 0.
* **Safety Logic:** 4-second Yellow light warning phase to simulate realistic transitions.
* **Opposite Sync:** Automatically mirrors lights for opposite-facing traffic.

## 🚀 How to Use
1. Clone this repository.
2. Open the project in **Microchip Studio** or **Atmel Studio**.
3. Compile the code to generate the `.hex` file.
4. Flash the file to your **ATmega32** or run it in a **Proteus** simulation.
