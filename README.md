Proiect InkTime - Smartwatch

Acest repository conține toate fișierele de design necesare pentru InkTime, un smartwatch open source bazat pe microcontrolerul nRF52840.
Sistemul este construit în jurul SoC-ului nRF52840, care gestionează comunicația BLE, procesarea datelor de la senzori și controlul afișajului e-paper.

Conexiuni Principale:
- Alimentare - PWR: Baterie Li-Po 200mAh - Încărcător TI BQ25180 (IC1) -> nRF52840 (3.3V). Am adăugat Test Pad-uri pentru conectarea directă a bateriei.
- Senzori - I2C Common: nRF52840 (SCL/SDA) - Bosch BMA421 (IC3 - accelerometru) și TI DRV2605L (IC2 - driver haptic). Ambele sunt pe layer-ul TOP.
- Afișaj SPI Common: nRF52840 (CLK/MOSI) - Circuit de comandă EPD -> Afișaj E-Paper
- Haptice: DRV2605L - IC2 - Shaker (Motor Vibrație).
- Interfață Utilizator - 3x Push Buttons laterale conectate direct la GPIO-uri nRF.
- Radio: nRF52840 - AN1603-245 (Antenă Ceramică). Zona de sub antenă a fost decupată (keepout) pe toate layerele pentru integritatea semnalului.

InkTime este proiectat pentru un consum minim, prioritizând o viață lungă a bateriei și o integrare mecanică perfectă într-o carcasă printabilă 3D.

Comunicații și Interfețe:
- I2C Bus (Common): Am conectat microcontrolerul nRF52840 (SCL/SDA) la BQ25180, BMA421, MAX17048 și DRV2605L. Am confirmat că toate componentele pot funcționa pe layer-ul TOP.
Am folosit rezistențe de pull-up de 10k
- SPI Bus (Common): nRF52840 (CLK/MOSI) - EPD Driver Circuit.
- GPIO: Am conectat cele 3 push buttons direct la GPIO-urile nRF52840, cu rezistențe de pull-down externe.
- Radio (BLE): Am amplasat antena AN1603-245 spre exteriorul PCB-ului și am decupat planul de masă de sub ea (copper keepout) pe toate layerele pentru integritatea semnalului BLE.

- Layout Componente: Am amplasat toate componentele principale (nRF52840, BMA421, BQ25180, Antenă) pe layer-ul TOP. Layer-ul BOTTOM a fost rezervat pentru planul de masă comun și rute secundare.
- GND Plane & Vias: Am realizat automat planul de masă (GND Plane) pe BOTTOM utilizând GND_Polygon. Am aplicat stitching vias între cele două layere (TOP/BOTTOM) pentru a asigura o referință solidă și a reduce zgomotul, în special în preajma circuitului radio.
- Grosime PCB: Am verificat și confirmat că grosimea PCB-ului este setată la 1.0mm, nu cea implicită de 1.6mm.

Consum de energie (estimare)
nRF52840 --	~5-10mA activ
E-Paper	-- doar la refresh
IMU -- 	~100µA
Fuel Gauge -- ~50µA
Haptic -- peak mare

Explicație pini nRF52840
-- VDD - alimentare principală MCU (3.3V)
-- DEC1, DEC3, DEC4 - pini pentru stabilizarea internă a tensiunii
-- DCC + inductor (L2, L3) folosite pentru DC/DC intern
-- XL1 / XL2 - cristal de 32 MHz - pentru CPU + radio BLE
-- P0.00 / P0.01 (LFXO) - cristal de 32.768 kHz pentru low power + sleep
-- RF (Bluetooth) - ANT pin - transmitere BLE
-- GPIO pinii generali - (input ,output, I2C, SPI, interrupts)
-- P0.06 - SDA  -- P0.07 - SCL - conectate la:
IMU (BMA421)
Fuel Gauge (MAX17048)
Charger (BQ25180)
Haptic (DRV2605)
-- P0.08 - IMU INT1 detectare mișcare
-- P0.10 - Fuel Gauge ALERT baterie low
-- USB D+ / D- conectate la USB-C
-- SWDIO - SWDCLK folosite la debug
-- Butoane P0.1, P0.14, P1.00
