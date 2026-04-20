# InkTime - Smartwatch cu e-Paper si nRF52840

## Descriere generala

InkTime este un smartwatch open-source bazat pe microcontroller-ul nRF52840 de la Nordic Semiconductor, cu display e-Paper, alimentat de o baterie LiPo. Proiectul include senzor IMU (accelerometru), motor haptic pentru feedback tactil, incarcare prin USB-C si comunicatie Bluetooth Low Energy.

## Diagrama bloc

```
                          +------------------+
                          |    nRF52840      |
                          |   (MCU + BLE)    |
                          +--------+---------+
                                   |
          +------------+-----------+----------+------------+
          |            |           |          |            |
     +----+----+  +----+----+ +---+---+ +----+----+ +-----+-----+
     | BMA421  |  |DRV2605  | |Display| |  SWD    | |   USB-C   |
     |  IMU    |  | Haptic  | |e-Paper| |  Debug  | | Connector |
     | (I2C)   |  | (I2C)   | | (SPI) | |         | |           |
     +---------+  +---------+ +-------+ +---------+ +-----+-----+
                                                           |
                                                     +-----+-----+
                                                     | USBLC6-2  |
                                                     | ESD Prot. |
                                                     +-----------+
          +------------+-----------+
          |            |           |
     +----+----+  +----+----+ +---+------+
     |BQ25180  |  |RT6160   | |MAX17048  |
     | LiPo   |  | DC/DC   | |  Fuel    |
     |Charger  |  |Converter| |  Gauge   |
     | (I2C)   |  | (I2C)   | |  (I2C)   |
     +---------+  +---------+ +----------+
          |            |
     +----+------------+----+
     |      Baterie LiPo    |
     |    3.7V / 250mAh     |
     +----------------------+
```

## BOM (Bill of Materials)

| Componenta | Valoare | Package | Cantitate | Datasheet | JLCPCB |
|------------|---------|---------|-----------|-----------|--------|
| nRF52840-QFAA | MCU + BLE | QFN-48 | 1 | [Datasheet](https://infocenter.nordicsemi.com/pdf/nRF52840_PS_v1.1.pdf) | [Link](https://jlcpcb.com/parts) |
| BQ25180YBGR | LiPo Charger | DSBGA-8 | 1 | [Datasheet](https://www.ti.com/lit/ds/symlink/bq25180.pdf) | [Link](https://jlcpcb.com/parts) |
| RT6160AWSC | DC/DC Buck-Boost | WLCSP | 1 | [Datasheet](https://www.richtek.com/assets/product_file/RT6160A/DS6160A-05.pdf) | [Link](https://jlcpcb.com/parts) |
| MAX17048G+T10 | Fuel Gauge | DFN-8 | 1 | [Datasheet](https://datasheets.maximintegrated.com/en/ds/MAX17048-MAX17049.pdf) | [Link](https://jlcpcb.com/parts) |
| BMA421 | Accelerometru/IMU | LGA-12 | 1 | [Datasheet](https://www.bosch-sensortec.com/products/motion-sensors/accelerometers/bma421/) | [Link](https://jlcpcb.com/parts) |
| DRV2605YZFR | Haptic Driver | DSBGA-12 | 1 | [Datasheet](https://www.ti.com/lit/ds/symlink/drv2605.pdf) | [Link](https://jlcpcb.com/parts) |
| USBLC6-2SC6Y | ESD Protection | SOT23-6 | 1 | [Datasheet](https://www.st.com/resource/en/datasheet/usblc6-2.pdf) | [Link](https://jlcpcb.com/parts) |
| DMG2305UX-7 | P-MOSFET | SOT23-3 | 1 | [Datasheet](https://www.diodes.com/assets/Datasheets/DMG2305UX.pdf) | [Link](https://jlcpcb.com/parts) |
| KH-TYPE-C-16P | USB-C Connector | SMD | 1 | - | [Link](https://jlcpcb.com/parts) |
| 2450AT18B100E | Antena 2.4GHz | SMD | 1 | [Datasheet](https://www.johansontechnology.com/datasheets/2450AT18B100/2450AT18B100.pdf) | [Link](https://jlcpcb.com/parts) |
| Waveshare 12561 | Display e-Paper 1.54" | FPC-24 | 1 | [Datasheet](https://www.waveshare.com/w/upload/7/77/1.54inch_e-Paper_Datasheet.pdf) | - |
| LP502030 | Baterie LiPo 250mAh | - | 1 | [Datasheet](https://www.tme.eu/Document/b9e12bf26ad0ba929a22ab5d58f022cd/AKY0106.pdf) | - |
| FIT0774 | Mini Vibration Motor | 10x2.7mm | 1 | [Datasheet](https://www.dfrobot.com/product-2297.html) | - |
| MBR0530 | Dioda Schottky | SOD-323 | 3 | [Datasheet](https://www.onsemi.com/pdf/datasheet/mbr0530-d.pdf) | [Link](https://jlcpcb.com/parts) |
| Cristal 32MHz | CX2016 | 2x1.6mm | 1 | - | [Link](https://jlcpcb.com/parts) |
| Cristal 32.768kHz | CC7V | 3.2x1.5mm | 1 | - | [Link](https://jlcpcb.com/parts) |
| Condensatoare 0201 | 100nF, 1pF, 12pF etc. | 0201 | ~20 | - | [Link](https://jlcpcb.com/parts) |
| Condensatoare 0402 | 1uF, 4.7uF, 10uF | 0402 | ~15 | - | [Link](https://jlcpcb.com/parts) |
| Rezistoare 0201 | diverse valori | 0201 | ~15 | - | [Link](https://jlcpcb.com/parts) |
| EVP-AKE31A | Push Button | SMD | 3 | - | [Link](https://jlcpcb.com/parts) |
| TC2030-IDC | SWD Debug Header | SMD | 1 | - | [Link](https://jlcpcb.com/parts) |

## Functionalitate hardware detaliata

### Microcontroller - nRF52840

nRF52840 este un SoC (System on Chip) care combina un procesor ARM Cortex-M4F la 64MHz cu un transceiver Bluetooth 5.0 / BLE. Are 1MB Flash si 256KB RAM, suficient pentru firmware-ul smartwatch-ului. Include suport nativ pentru USB 2.0, ceea ce permite programarea si comunicatia directa prin USB-C fara convertor suplimentar.

### Alimentare

Lantul de alimentare este urmatorul:
- **USB-C (VBUS 5V)** → **BQ25180** (charger LiPo) → **Baterie 3.7V (VBAT)**
- **VBAT** → **RT6160** (DC/DC buck-boost) → **VREG** → **nRF52840 (VDDH)**
- nRF52840 genereaza intern **3V3** pentru alimentarea celorlalte componente

BQ25180 este un charger LiPo extrem de compact (DSBGA-8), ideal pentru wearables. RT6160 este un DC/DC buck-boost care mentine tensiunea de iesire stabila indiferent de nivelul bateriei. MAX17048 monitorizeaza nivelul bateriei prin I2C si genereaza alerte cand bateria e scazuta.

### Display e-Paper

Display-ul Waveshare 1.54" (200x200 pixeli) comunica prin SPI cu nRF52840. Avantajul principal e consumul ultra-redus — display-ul consuma energie doar cand se actualizeaza continutul, apoi il pastreaza fara alimentare. Circuitul de drive EPD include diode Schottky (MBR0530) si un MOSFET (SI1308EDL) pentru generarea tensiunilor necesare display-ului.

### Senzor IMU - BMA421

BMA421 este un accelerometru triaxial conectat prin I2C. Poate detecta miscari, pasi (pedometer), si gesture-uri. Genereaza intreruperi (IMU_INT1, IMU_INT2) catre nRF52840 pentru a-l trezi din sleep la detectarea miscarii.

### Haptic Driver - DRV2605

DRV2605 controleaza motorul de vibratie (shaker-ul) prin I2C. Are o biblioteca interna de efecte haptice (click, buzz, ramp etc.) si poate genera vibratii personalizate. E conectat la shaker-ul de 10mm prin pinii OUT+ si OUT-.

### ESD Protection - USBLC6-2SC6Y

Protectie ESD pe liniile USB (D+, D-) impotriva descarcarilor electrostatice. Esential pentru orice dispozitiv cu conector USB expus.

### Debug - SWD (TC2030-IDC)

Conector tag-connect pentru programare si debug prin SWD (Serial Wire Debug). Foloseste pinii SWDIO, SWDCLK si optional SWO pentru trace.

## Pinout nRF52840

| Pin nRF52840 | Functie | Componenta | Interfata |
|-------------|---------|------------|-----------|
| P0.00 | XL1 | Cristal 32.768kHz | Oscilator |
| P0.01 | XL2 | Cristal 32.768kHz | Oscilator |
| P0.02-P0.05 | AIN0-AIN3 | Disponibili | ADC |
| P0.09 | NFC1 | Nefolosit | - |
| P0.10 | NFC2 | Nefolosit | - |
| P0.13 | SCK | Display e-Paper | SPI Clock |
| P0.14 | MOSI | Display e-Paper | SPI Data |
| P0.15-P0.17 | EPD_RST/BUSY/DC | Display e-Paper | Control |
| P0.18 | RESET | Reset extern | - |
| P0.19 | EPD_CS | Display e-Paper | SPI Chip Select |
| P0.20 | SCL | I2C Bus | I2C Clock |
| P0.21-P0.24 | SDA, diverse | I2C Bus + altele | I2C Data |
| SWDIO | Debug | TC2030 | SWD |
| SWDCLK | Debug | TC2030 | SWD |
| P1.01 | ALERT | MAX17048 Fuel Gauge | Interrupt |
| P1.02 | PMIC_INT | BQ25180 | Interrupt |
| P1.04 | IMU_INT1 | BMA421 | Interrupt |
| P1.05 | IMU_INT2 | BMA421 | Interrupt |
| P1.06 | HAPTIC_EN | DRV2605 | Enable |
| D+ / D- | USB Data | USB-C Connector | USB 2.0 |
| ANT | RF | Antena 2450AT18B100E | Antena 2.4GHz |

**Nota:** Pinii I2C (SCL, SDA) sunt partajati intre mai multe componente pe acelasi bus: BMA421, DRV2605, MAX17048, BQ25180 si RT6160. Fiecare componenta are adresa I2C unica, deci nu exista conflicte.

## Pasi de implementare

1. **Schematic** — Am pornit de la schema electrica furnizata (inktime_schematic.pdf) si am implementat-o in Fusion 360 Electronics folosind biblioteca InkTime_v5.lbr.

2. **Stackup 4 layere** — Am configurat PCB-ul cu 4 straturi de cupru:
   - Top: componente + semnale + pour GND
   - Inner1 (Route2): plan GND dedicat
   - Inner2 (Route63): power
   - Bottom: semnale + pour GND

3. **Plasarea componentelor** — Toate componentele au fost plasate exclusiv pe layer-ul TOP, respectand layout-ul recomandat din fisierul inktime.fbrd. Butoanele si mufa USB-C au fost aliniate cu deschiderile din carcasa.

4. **Rutarea power** — Am rutat mai intai traseele de alimentare (3V3, VBAT, VBUS, VREG) cu width de 0.3mm pe Top si Route63.

5. **Rutarea semnalelor** — Semnalele de date au fost rutate cu width minim de 0.15mm.

6. **Pour-uri GND** — Am adaugat polygon pour cu net GND pe Top, Inner1 si Bottom.

7. **Via stitching** — Am plasat vias-uri GND pentru a conecta planurile de masa de pe Top, Inner1 si Bottom.

8. **Modele 3D** — Am adaugat modele STEP pentru fiecare componenta din library, editate prin Package Editor. Am desenat de la zero modelele 3D pentru baterie (32.5x21x5.5mm), display (37.32x31.8x1.05mm) si shaker (D10x2.7mm) folosind dimensiunile din datasheet-uri.

9. **Asamblare** — Am importat carcasa (InkTime_Case.f3z) si am asamblat PCB-ul cu bateria, display-ul si shaker-ul in interior. Am verificat cu Section Analysis ca totul incape.

10. **Generare fisiere de fabricatie** — Am exportat Gerber files, BOM si Pick and Place (CPL) conform tutorialelor recomandate.

## Probleme intampinate si decizii luate

### Via-in-pad
Am acceptat erori de via-in-pad in cateva locuri, in special la componentele BGA (BQ25180, nRF52840). Pad-urile acestor componente sunt atat de apropiate incat nu exista spatiu fizic sa scoti un trace din pad fara sa pui un via direct pe el. Este o practica standard pentru BGA si nu afecteaza functionalitatea, dar poate necesita via filling la fabricatie.

### Dimensiunile bateriei ajustate
Modelul 3D al bateriei a fost usor ajustat ca dimensiuni fata de maximul din datasheet (32.5x21x5.5mm) pentru a incapea corect in carcasa. In realitate, dimensiunile din datasheet sunt valori maxime, iar bateria reala este putin mai mica, deci ajustarea este realista.

### Rutarea dificila
Rutarea a fost cea mai consumatoare parte a proiectului. Pe un PCB de dimensiuni de smartwatch, cu atatea componente, gasirea traseelor a fost un proces iterativ. Cateva semnale au fost rutate pe inner layers — am acceptat asta constient, stiind ca fragmenteaza usor planurile.

### Erori de Dimension la butoane si USB
Erorile de DRC cauzate de amplasarea celor trei butoane si a mufei USB au fost ignorate conform recomandarilor din cerinta — sunt erori false, cauzate de componentele care ies partial din board outline.

### Decupaj sub antena
Am asigurat ca zona de sub antena (2450AT18B100E) nu contine cupru pe niciun layer si nu are trasee rutate, conform specificatiilor Nordic Semiconductor. Antena este amplasata la marginea PCB-ului pentru performanta RF optima.

### Condensatoare de decuplare
Condensatoarele de 100nF au fost plasate cat mai aproape de pinii de alimentare ai fiecarui IC. Pinii DEC ai nRF52840 au condensatoare dedicate imediat langa chip, cu trace-uri cat mai scurte.

### Grosimea PCB-ului
PCB-ul a fost configurat cu grosime de 1mm conform cerintei, pentru a incapea in carcasa.
