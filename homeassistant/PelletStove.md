# ESPHome Project Pinouts

## ðŸ›  **Wemos D1 Mini Pin Assignments**

### **ðŸ”´ RGB LED (Common Cathode)**
| **LED Pin** | **ESP8266 GPIO** | **D1 Mini Pin** |
|------------|----------------|----------------|
| **Red (R)** | `GPIO14` | `D5` |
| **Green (G)** | `GPIO12` | `D6` |
| **Blue (B)** | `GPIO13` | `D7` |
| **Ground (GND)** | `GND` | `GND` |

---

### **ðŸ”µ HC-SR04 Ultrasonic Sensor**
| **HC-SR04 Pin** | **ESP8266 GPIO** | **D1 Mini Pin** |
|---------------|----------------|----------------|
| **VCC (5V)** | `VBUS` | `VBUS (5V from USB)` |
| **Trig** | `GPIO4` | `D2` |
| **Echo** | `GPIO5` | `D1` (âš ï¸ Use 1kÎ©/2kÎ© voltage divider) |
| **GND** | `GND` | `GND` |

---

### **ðŸŸ¢ 10kÎ© Thermistor Temperature Sensor**
| **Thermistor Pin** | **ESP8266 GPIO** | **D1 Mini Pin** |
|------------------|----------------|----------------|
| **One Side** | `A0` | `A0 (Analog Input)` |
| **Other Side** | `3.3V` | `3.3V` |
| **10kÎ© Resistor** | Between `A0` and `GND` | |

---

## **Wiring Notes**
- **HC-SR04 Echo Pin Protection**: Use a **1kÎ©/2kÎ© voltage divider** to step down **5V â†’ 3.3V** before connecting to GPIO5 (D1).
- **Use 22 AWG Wire**: Helps prevent voltage drop over longer distances (up to ~6ft).
- **Capacitor for HC-SR04 Power**: Add a **10ÂµF - 100ÂµF capacitor** across **VCC and GND** near the sensor if stability issues arise.
- **Twisted Pair for Long Runs**: If running longer wires, use **Cat5/Cat6 twisted pairs** to reduce noise.

---

## **ESPHome YAML Configuration**
```yaml
output:
  - platform: esp8266_pwm
    id: red_led
    pin: GPIO14
  - platform: esp8266_pwm
    id: green_led
    pin: GPIO12
  - platform: esp8266_pwm
    id: blue_led
    pin: GPIO13

light:
  - platform: rgb
    name: "RGB LED"
    red: red_led
    green: green_led
    blue: blue_led
    default_transition_length: 1s

sensor:
  - platform: ultrasonic
    trigger_pin: GPIO4
    echo_pin: GPIO5
    name: "Distance Sensor"
    update_interval: 1s

  - platform: ntc
    name: "Thermistor Temperature"
    sensor: resistance_sensor
    calibration:
      b_constant: 3950
      reference_temperature: 25Â°C
      reference_resistance: 10kOhm

  - platform: resistance
    id: resistance_sensor
    sensor: analog_sensor
    configuration: UPSTREAM
    resistor: 10kOhm

  - platform: adc
    id: analog_sensor
    pin: A0
    update_interval: 5s
```
