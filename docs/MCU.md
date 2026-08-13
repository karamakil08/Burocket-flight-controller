# MCU Subsystem

## Clock Configuration

The system is driven by a 25 MHz High-Speed External (HSE) oscillator. A series damping resistor is often recommended to limit the crystal drive level, but it was deemed non-mandatory and excluded from this design.

The load capacitors for the oscillator were calculated using the standard formula:

$$C_1 = C_2 = 2 \times (C_L - C_S)$$

Where:
*   **$C_L$ (Load Capacitance):** 10 pF (based on the [ECS-250-10-36Q-ES-TR](https://www.mouser.com/ProductDetail/ECS/ECS-250-10-36Q-ES-TR?qs=%2FnKYMpMO73OnPa%252BAGH8PiA%3D%3D&srsltid=AfmBOoo1AqcuuS_I_0B5soML1Nrf6dDIe3bqTjITI6zJSijk9Nni3Xhj) specification)
*   **$C_S$ (Stray Capacitance):** Estimated at 5 pF for this PCB layout

$$C_1 = C_2 = 2 \times (10\text{ pF} - 5\text{ pF}) = 10\text{ pF}$$

Based on this calculation, 10 pF capacitors were selected for both $C_1$ and $C_2$.

## User Interface & Controls

*   **Status Indicators:** The board features two indicator LEDs (one Red, one Blue) for system state feedback.
*   **Hardware Reset:** A dedicated push button is routed to the NRST pin to allow manual system resets.
*   **Boot Configuration:** An SPDT (Single Pole Double Throw) switch is connected to the BOOT pin, allowing the user to physically toggle between boot modes.

## Power & Debugging

*   **Decoupling:** Power delivery is stabilized across the MCU utilizing X7R dielectric capacitors, selected for their high performance and temperature stability in flight environments.
*   **Programming Interface:** A Serial Wire Debug (SWD) interface is exposed for programming and debugging. This interface includes a Serial Wire Output (SWO) pin connection to enable real-time trace logging.
