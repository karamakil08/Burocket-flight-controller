# Power Subsystem

The power delivery network is designed to ensure continuous, clean power to all logic and sensor systems, with fail-safes for ground testing and flight operations.

## Power Sources & Multiplexing

The system operates from two primary power sources:
1.  **Flight Battery:** A 2S LiPo battery (nominally 7.4V - 8.4V).
2.  **USB-C VBUS:** 5V provided via the USB interface for ground configuration and data retrieval.

**Buck Conversion & 5V Bus:**
The 2S battery voltage is stepped down to 5V using a dedicated buck converter. This 5V bus is used to power higher-voltage peripherals, including the LoRa telemetry module.

**Power Muxing (TPS2121RUXR):**
To safely manage power when both the battery and USB are connected, the system uses a **TPS2121RUXR** priority power multiplexer. This IC automatically and seamlessly switches between the 5V buck output and the USB-C VBUS. It prevents back-powering into the USB host and eliminates voltage droops or system resets when plugging or unplugging the USB cable on the launch pad.

## 3.3V Regulation

**LDO Regulator:** TPS7A4701
The 5V multiplexed output is stepped down to 3.3V using the **TPS7A4701** Low-Dropout (LDO) regulator. 
*   **Properties:** The TPS7A4701 is an ultra-low-noise ($4.1 \text{ }\mu V_{RMS}$), high-PSRR linear regulator capable of outputting up to 1A. 
*   **Application:** While highly capable, this is an instrumentation-grade regulator. Its extreme noise rejection ensures that the high-resolution ADCs on the MS5611 barometer and ICM IMUs have a perfectly stable voltage reference, minimizing noise floor in the telemetry data.

## Power Monitoring & Current Sensing

To monitor system health, power consumption, and battery state, a high-precision current sensing architecture is implemented on the main power rails.

**Current Sensor:** INA226
The **INA226** is a bidirectional current and power monitor with an I2C interface. It measures both the shunt voltage drop and the bus supply voltage, allowing the flight computer to log real-time power consumption and detect potential shorts or brown-out conditions before they cause a critical failure.

**Shunt Resistor:** WSLP12065L000FEA
*   **Properties:** $5\text{ m}\Omega$, 1% tolerance, 1W rating in a 1206 package.
*   **Performance:** Using a $5\text{ m}\Omega$ shunt with the INA226 (which has a maximum shunt voltage reading of $81.92\text{ mV}$) allows the system to measure current peaks up to $\sim 16.38\text{ A}$ before the ADC saturates ($I = \frac{81.92\text{ mV}}{5\text{ m}\Omega}$). The 1W power rating ensures the resistor can comfortably handle high current spikes (up to $14.1\text{ A}$ continuous) without thermal degradation or desoldering under load.
