# QuadFlyController - Flight Controller Hardware

A high-performance, compact, 4-layer flight controller PCB engineered for quadcopters and small UAVs. Built around the STMicroelectronics STM32F411CEU6 MCU, featuring high-rate low-noise motion sensing, barometric altitude tracking, robust power management with dual-source automatic switching, and standard 30.5 mm × 30.5 mm mounting.

---

## 🚀 Key Specifications

| Subsystem | Component | Description |
| :--- | :--- | :--- |
| **Microcontroller (MCU)** | **STM32F411CEU6** | ARM Cortex-M4 @ 100 MHz, FPU, 512 KB Flash, 128 KB SRAM, external 25 MHz HSE oscillator |
| **Inertial Measurement (IMU)** | **ICM-42688-P** | High-precision 6-axis gyroscope & accelerometer on dedicated high-speed SPI bus with interrupt line |
| **Barometer / Altimeter** | **BMP390** | Ultra-low noise barometric pressure sensor on SPI bus for altitude hold |
| **Switching Regulator (Buck)** | **TPS62933** | High-efficiency 3A synchronous step-down converter providing stable +5V rail from battery |
| **Linear Regulator (LDO)** | **AP2112K-3.3** | Ultra-low dropout, low-noise 600 mA 3.3V LDO for clean MCU and analog sensor rails |
| **Power Path Switching** | **TPS2115A** | Seamless automatic power multiplexer between USB VBUS (+5V) and internal Buck converter (+5V) |
| **Transient Protection** | **SMBJ18A** | Bidirectional TVS diode protection on battery input |
| **USB Protection** | **USBLC6-2SC6** | Ultra-low capacitance ESD protection array on USB D+/D- lines |
| **USB Interface** | **USB Type-C (16-pin)** | Firmware flashing (DFU), telemetry, and configuration interface |
| **ESC Interface** | **JST-SH 8-Pin (1.0 mm)** | Standard 4-in-1 ESC connector (VBAT, GND, Motors 1–4, Current sensing) |

---

## 📐 PCB & Mechanical Specifications

- **Dimensions:** 45.0 mm × 45.0 mm with 3.5 mm corner radius
- **Mounting Hole Pattern:** Standard **30.5 mm × 30.5 mm**, 4.3 mm holes (M4, compatible with anti-vibration silicone grommets down to M3 hardware)
- **Layer Stack:** **4-Layer PCB**
  - **F.Cu (Top Layer):** High-speed signals, component breakout, top ground pour
  - **In1.Cu (Inner Layer 1):** Solid ground reference plane (GND)
  - **In2.Cu (Inner Layer 2):** Power distribution (+5V, +3.3V, +BATT) and cross-layer routing
  - **B.Cu (Bottom Layer):** Bottom ground plane and secondary signal routing
- **3D Mechanical Model:** Full 3D STEP model available in [`FlightController/CAD/FlightController.step`](FlightController/CAD/FlightController.step) for frame integration and enclosure design.

---

## 🔌 Pinout & Connector Definitions

### J3: 8-Pin JST-SH 4-in-1 ESC Connector (`SM08B-SRSS-TB`)

| Pin | Net Name | Function / Description |
| :---: | :--- | :--- |
| **1** | `+BATT` | Raw battery voltage input from ESC / PDB |
| **2** | `GND` | Power and signal ground return |
| **3** | `CUR` | Analog current sensing signal from ESC shunt sensor |
| **4** | `M1` | Motor 1 PWM / DShot signal output (STM32 Timer channel) |
| **5** | `M2` | Motor 2 PWM / DShot signal output (STM32 Timer channel) |
| **6** | `M3` | Motor 3 PWM / DShot signal output (STM32 Timer channel) |
| **7** | `M4` | Motor 4 PWM / DShot signal output (STM32 Timer channel) |
| **8** | `GND` | Ground return |

### External Pads & Test Points

- **UART (TX / RX):** External receiver connection (ELRS, Crossfire, SBUS) or GPS telemetry module.
- **SWD (SWDIO / SWCLK):** Hardware debug and in-circuit flashing with ST-Link or J-Link.
- **BOOT0:** Jumper/pad for STM32 internal ROM system bootloader (DFU mode over USB).
- **Status LEDs:**
  - `D1`: Power indicator
  - `D2`: User / Status LED (connected to STM32 GPIO)

---

## 📂 Repository Structure

```text
Electronics/
├── .gitignore                                 # Git ignore configuration
├── LICENSE                                    # License file
├── README.md                                  # Hardware documentation
├── FlightController_Schematic.pdf             # Exported full schematic PDF
│
└── FlightController/                          # KiCad 10 Project Directory
    ├── FlightController.kicad_pro             # KiCad project file
    ├── FlightController.kicad_pcb             # KiCad 4-layer PCB layout
    ├── FlightController.kicad_sch             # Root schematic sheet
    ├── MCU.kicad_sch                          # STM32F411CEU6 microcontroller sheet
    ├── IMU.kicad_sch                          # ICM-42688-P & BMP390 sensor sheet
    ├── PowerManagement.kicad_sch              # Power rails, buck, LDO, & USB sheet
    ├── fp-lib-table                           # Footprint library table
    ├── sym-lib-table                          # Symbol library table
    ├── fabrication-toolkit-options.json       # Production generation settings
    │
    ├── CAD/                                   # 3D Mechanical Models
    │   └── FlightController.step              # Complete 3D STEP assembly of the PCB
    │
    └── production/                            # Manufacturing & Assembly Files
        ├── FlightController.zip               # Gerbers & Drill archive (RS-274X / Excellon)
        ├── bom.csv                            # Bill of Materials (with LCSC part numbers)
        ├── positions.csv                      # Pick-and-Place (CPL) centroid coordinates
        ├── designators.csv                    # Component designator mapping
        └── netlist.ipc                        # IPC-D-356 netlist for bare-board testing
```

---

## 🏭 Manufacturing & Assembly (PCBA)

The files in [`FlightController/production/`](FlightController/production/) are pre-configured and formatted for rapid quoting and ordering with PCB manufacturers (such as JLCPCB, PCBWay, etc.):

1. **PCB Fabrication (Gerber & Drill):**
   - Upload [`FlightController/production/FlightController.zip`](FlightController/production/FlightController.zip).
   - Layers: **4 Layers**
   - Dimensions: **45 mm × 45 mm**
2. **SMT Assembly (PCBA):**
   - **BOM File:** Upload [`FlightController/production/bom.csv`](FlightController/production/bom.csv) (includes pre-matched LCSC part numbers for automated component sourcing).
   - **CPL / Centroid File:** Upload [`FlightController/production/positions.csv`](FlightController/production/positions.csv).
3. **Bare-Board Electrical Test:**
   - [`FlightController/production/netlist.ipc`](FlightController/production/netlist.ipc) is provided for automated flying-probe netlist verification.

---

## 🛠️ Design & Development Environment

- **EDA Software:** [KiCad EDA v10.x](https://www.kicad.org/)
- **To Open the Project:** Launch KiCad and open [`FlightController/FlightController.kicad_pro`](FlightController/FlightController.kicad_pro).
- **Viewing the Schematic without KiCad:** Open [`FlightController_Schematic.pdf`](FlightController_Schematic.pdf).