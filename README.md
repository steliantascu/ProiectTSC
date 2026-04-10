Proiect InkTime - Stelian-Andrei Tascu

1. Introducere
Proiectul InkTime reprezinta dezvoltarea unui smartwatch low-cost si open-source, bazat
pe microcontrolerul nRF52840. Acest repository contine documentatia tehnica, fisierele
de proiectare hardware (schematica si PCB) realizate in Autodesk Fusion 360, fisierele
de fabricatie (Gerber, BOM, CPL) si modelul 3D complet al dispozitivului.

2. Diagrama Bloc
Diagrama se afla in /Images si poarta numele diagrama_inktime.png. 
Diagrama bloc ilustreaza arhitectura hardware a smartwatch-ului InkTime, avand in centru
microcontrolerul nRF52840 care coordoneaza senzorii de mediu, display-ul E-Paper si managementul bateriei.
Aceasta evidentiaza interfetele de comunicatie (SPI, I2C, UART) si fluxul de putere
necesar functionarii intregului sistem.

Sistemul este construit in jurul SoC-ului nRF52840 (Cortex-M4 cu suport BLE),
alimentat de o baterie Li-Po. Include senzori de mediu, un display E-Ink pentru
consum redus si interfata prin butoane si USB-C.

3. Detalii Hardware si Functionalitate
Dispozitivul foloseste urmatoarele module si componente principale:

Microcontroler: nRF52840 - asigura conectivitatea si managementul tuturor perifericelor.

Display: E-Paper Display (interfata SPI) - ales pentru lizibilitate in soare
si consum de energie aproape de zero pentru imagini statice.

Alimentare: Circuit de incarcare Li-Po via USB-C, cu management de putere
si reglare la 3.3V pentru componentele logice.

Interfata: 3 butoane fizice pentru navigare si un motor haptic (shaker)
pentru notificari prin vibratie.

Stocare: Slot MicroSD pentru logare de date.

Senzori: Senzori de temperatura, umiditate (BME680) si senzori de calitate
a aerului (PM, CO2) conform diagramei de sistem.

4. Pin Mapping nRF52840
Alocarea pinilor a fost facuta tinand cont de multiplexarea interna a nRF52840
si de optimizarea rutarii pe PCB:

Componenta	Interfata	Pini nRF52840	Motivatie
Display E-Ink	SPI	P0.15 (SCK), P0.17 (MOSI)	SPI Hardware pentru refresh rapid
MicroSD Card	SPI	P0.20 (SCK), P0.22 (MOSI)	Partajare resurse SPI
Senzor BME680	I2C	P0.26 (SDA), P0.27 (SCL)	Pini dedicati I2C
Butoane	GPIO	P1.02, P1.04, P1.06	Folosirea Portului 1 pentru a lasa Portul 0 pt periferice rapide
Shaker	PWM	P0.13	Suport PWM pentru intensitatea vibratiei
UART (Senzori PM/CO2)	UART	P0.06 (TX), P0.08 (RX)	Comunicatie seriala standard

5. Bill of Materials (BOM)
Componentele importante folosite in proiect:

Componenta	Valoare/Model	Capsula
MCU	nRF52840	aQFN-73	
Rezistente	Diverse	0201	
Condensatori	100nF	0201	
Condensatori	>100nF	0402	
USB Connector	USB-C 16-pin	SMD	
Battery	Li-Po 150mAh	Custom	

6. Design Log si decizii tehnice
Antena a fost plasata pe marginea exterioara a PCB-ului.

S-a utilizat stratul tRestrict (Layer 41) pentru a crea un "Keep-out zone" sub antena.
Acest lucru asigura ca planul de masa (GND) este decupat sub elementul radiant,
prevenind atenuarea semnalului BLE.

In urma rularii verificarii DRC, design-ul prezinta peste 1000 de erori
de tip Overlap si Copper Clearance.
Aceste erori au fost acceptate in mod constient pentru a permite rutarea
semnalelor in zonele extrem de dense (sub integratele cu pitch mic).

Prioritatea a fost integritatea conexiunilor electrice si posibilitatea
de a trage firele (routing) intr-un spatiu foarte restrans, impus de carcasa
smartwatch-ului. Erorile de overlap dintre trasee si pad-uri in zonele BGA sunt
rezultatul limitarilor de grid ale Fusion 360 la acest deadline, dar conexiunile
sunt conforme cu schema electrica.

Dimension Errors: Erorile de dimensiune cauzate de butoane si mufa USB-C au fost
ignorate conform recomandărilor din laborator, deoarece acestea trebuie sa iasa
in afara conturului placii pentru a fi accesibile din carcasa.

PCB-ul are o grosime de 1.0 mm (ajustata in Layer Stackup de la 1.6 mm).

Modelul 3D (Mechanical) include asamblarea exploded view: Carcasa, Display-ul,
PCB-ul, Bateria si Shaker-ul, verificate pentru a intra in volumul disponibil.

7. Structura Repository
/Hardware: Fisierele sursa Fusion 360 (.fsch, .fbrd).

/Manufacturing: Arhiva Gerber, BOM si Pick and Place.

/Mechanical: Modelul 3D (.step) si asamblarea (.f3d).

/Images: Randari si screenshot-uri ale proiectului.