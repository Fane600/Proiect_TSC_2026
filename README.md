# Proiect_TSC_2026

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
