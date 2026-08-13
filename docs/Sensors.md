# Sensors Subsystem

The sensor suite on this flight controller is designed to provide high-resolution, high-speed telemetry for orientation, altitude detection, and recovery operations. 

## Barometric Pressure & Temperature

**Sensor:** MS5611
The MS5611 is an industry-standard, high-reliability altimeter utilized heavily in aerospace and drone applications. It features a 24-bit ADC providing a resolution of up to 10 cm, which is critical for accurate apogee detection and precise pyrotechnic deployment. It was selected due to its proven track record in flight computers, overall abundance in the supply chain, and ease of integration.

## Inertial Measurement Units (IMU)

**Primary IMU:** ICM-42688-P
The ICM-42688-P is a premium 6-axis MEMS motion tracking device that combines a 3-axis gyroscope and a 3-axis accelerometer. It features exceptionally low noise and high sensitivity, providing the real-time orientation and angular velocity data required for flight stabilization and trajectory tracking. 

**High-G Accelerometer:** ADXL375
Standard IMUs typically saturate at around **16g**, which is easily exceeded during the boost phase or ejection shock of a high-powered model rocket. The ADXL375 is an additional 3-axis accelerometer capable of measuring up to **200g**. This ensures continuous data logging and flight profiling even when the ICM-42688-P's accelerometer is saturated.

**Design Consideration & Alternative:**
Using both the ICM-42688-P and ADXL375 introduces a physical challenge: if the sensor axes are not perfectly aligned on the PCB, a rotation matrix must be applied in software to normalize the data vectors. While solvable in firmware, this adds computational overhead and complexity. 
* *Alternative:* Future revisions may consider the **ICM-45686**, which features a **32g** acceleration limit. Depending on the motor profile, this higher limit might prevent saturation entirely, allowing the removal of the ADXL375 and bypassing the axes alignment complications entirely.

## GPS / GNSS Receiver

**Sensor:** MAX-M10N
The MAX-M10N by u-blox is a highly sensitive, ultra-low-power GNSS receiver module. It tracks multiple constellations concurrently (GPS, Galileo, GLONASS, and BeiDou), providing rapid lock times and high positional accuracy. This is the primary system utilized for post-flight ground tracking and rocket recovery.

## SPI Bus Signal Integrity & Safety

Communication with the primary sensors is handled via the SPI bus, which has been designed with strict signal integrity and fail-safes in mind:

* **Series Termination Resistors:** High-speed SPI lines are susceptible to ringing and voltage overshoots. Series resistors are placed on the bus lines to match the impedance of the traces. To function correctly, these resistors are placed as physically close to the driving device as possible (near the MCU for MOSI and SCK; near the sensors for MISO).
* **Slave Select (SS) Pull-Ups:** Dedicated pull-up resistors are integrated on all SS lines. This hardware-level safeguard keeps the pins at a logical HIGH state during system startup, resets, or when the MCU is unconfigured. This guarantees that the peripheral chips do not accidentally activate, listen to floating lines, or corrupt the shared bus before the firmware actively takes control.
