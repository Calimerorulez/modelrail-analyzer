# Voorlopige componentkeuze en pinreserveringen

Alle selecties zijn **O**, ook waar een onderdeel volgens een **B**-datasheet
een bepaalde functie ondersteunt. Geen bestel-BOM, footprints of definitieve
elektrische aansluitingen. Bronnen: [register](06-bronnenregister.md).

De **bestaande SRSEII is eigenaar-bevestigd als PCB 1.8.1a** [U02].
Haar oorspronkelijke bronset [S24–S27] noemt SN65HVD230, TPS5430DDA,
LM317 en PIC18F25K80/SSOP-28. Dit is geen BOM voor de analyzer en ook
geen vervanging van de eerder gemelde eigen PIC18F26K80. De
[revisievergelijking](09-srseii-revisievergelijking.md) bewaart deze verschillen.

## 1. Kandidaten

| Functie | Voorkeurskandidaat | Reden en nog te sluiten controle |
| --- | --- | --- |
| Hoofdprocessor | Espressif ESP32-S3-WROOM-1-N8R8 | 8 MB flash/8 MB PSRAM-variant; meer bufferruimte. Exacte module, geheugenvoeding, pinbeschikbaarheid en antennekeepout controleren [S11] |
| Ethernet | WIZnet W5500, LQFP48 | Eigen PCB; 3,3 V, SPI, 10/100 PHY [S14]. Magnetics, RJ45, kristal, PHY-bias en referentieontwerp als geheel selecteren |
| CAN-frontend | NXP TJA1051T/3, SO8 | Silent met actieve ontvanger; aparte VIO; geen MCU-TX-pad [S09]. Belasting, beschermingsnetwerk en deelvoeding toetsen |
| CAN digitale isolatie | TI ISO7720, default HIGH, wide-body DW | Twee kanalen uitsluitend CAN→MAIN voor RXD en VALID_N [S12]. Precieze bestelcode, input/output-voeding, enables en verliesgedrag controleren |
| S88-comparator | TI TLV1704, PW/TSSOP14 | Vier rail-to-rail ingangen en open-collector uitgangen [S10]. Voedingsloze railclamps en slechts typische timing maken kwalificatie noodzakelijk |
| S88 digitale isolatie | TI ISO7740F in DW + een ISO7710F voor VALID | Vier signaalkanalen plus geldigheid uitsluitend S88→MAIN [S13]. Default LOW ondersteunt ongeldige toestand; externe railbewaking nog selecteren |
| Geïsoleerde voeding | Twee afzonderlijke RECOM R05C05TE05S | Eén per veldinterface; 5 V→5 V, 0,5 W [S15]. Min/max bij lage belasting, ripple en inrush toetsen; geen gedeelde secundaire massa |
| Hoofdregelaar | TI TPS62162, 3,3 V / 1 A | Buck voor hoofdrail [S16]. Belastingssprongen, thermiek, spoel en capaciteit moeten nog berekend worden |
| Lokale secundaire 3,3 V | LDO, exact type open | In-/uitgangsbereik, reverse-currentgedrag, stabiliteit en bewaking bepalen na voedingsberekening |
| USB-C CC | TI TUSB320LAI, sink-configuratie | Stroomadvertentie detecteren [S18]. Hardwarerol, dead-batterygedrag, powergate en opstartbudget nog uitwerken |
| USB-C vermogenspad | Stroombegrenzing, OVP, reverse blocking en load gate; type open | Moet aan CC-budget, inrush en foutgeval voldoen. Geen onbekende goedkope USB-C-module als vervanging |
| OLED | Waveshare 1.3inch OLED (B), SKU 10451, SH1106 | 128×64 en 3,3 V-voeding beschikbaar [S19]. I²C-uitvoering vraagt configuratie; PCB/connectororiëntatie, reset en pull-ups controleren |
| Bediening | Twee momentdrukknoppen, type open | MAIN-domein; debounce, ESD en behuizing bepalen. Programmeer-/resetpads zijn geen extra gebruikersknoppen |
| S88-ingangsweerstanden | Gesplitste hoogohmige delerketens, laaglekuitvoering | Definitieve waarden volgen gemeten niveaus; tolerantie, vocht/flux, spanning en foutvermogen meenemen |
| Ingangsbeveiliging | Laaglek-/laagcapaciteitsdelen, type open | Spanning, clampniveau en foutstroom nog onbekend; TVS niet selecteren op nominale busvoeding alleen |
| Connectors en Ethernet-magnetics | Exacte typen open | Oriëntatie, footprint, isolatie, scherm en mechanische verwisselbaarheid zijn selectiecriteria |

De isolatoren uit de bestaande GBert-print worden niet automatisch gekozen
voor de analyzer. Ook een gerepareerde of anders bestukte bestaande isolator
moet afzonderlijk worden geïdentificeerd.

## 2. Datasheetbevindingen met gevolgen

**B [S09]:** TJA1051T/3: VCC 4,5–5,5 V; VIO 2,8–5,5 V. SO8:
1 TXD, 2 GND, 3 VCC, 4 RXD, 5 VIO, 6 CANL, 7 CANH, 8 S.
De `/3`-variant is functioneel relevant; `/E` heeft op pin 5 EN.
**O:** verkeerde variant is een review-/inkoopfout met elektrische gevolgen.

**B [S10]:** TLV1704 ondersteunt voeding 2,2–36 V; typische tpHL/tpLH
460/560 ns bij de opgegeven testcondities. De tabel geeft hier geen
gegarandeerde bovengrens. Biasstroom maximaal 20 nA over temperatuur.
Ingangen zijn naar voedingsrails geklemd. **O:** het timing- en
uitschakelbudget moet daarom expliciet worden aangetoond.

**B [S15]:** de RECOM-datasheet noemt een semi-geregelde uitgang,
functionele isolatie, 7 pF typische isolatiecapaciteit en een 3 kVDC/1 min
componenttest. **O:** deze combinatie maakt het apparaat geen
gecertificeerde beschermingsscheiding voor netspanning. Controleer bij
lage belasting zowel de uitgang als common-mode verstoring door de converter.

**B [S14]:** W5500-pinnen 32 SCSn, 33 SCLK, 34 MISO, 35 MOSI,
36 INTn, 37 RSTn; 30/31 voor de 25 MHz-kristal/klokaansluiting.
**O:** deze nummers horen bij het IC, niet bij willekeurige W5500-modules.

## 3. Voorlopig vermogensbudget

Onderstaande waarden zijn **O-reserveringen**, geen gemeten of gegarandeerde
maximale componentstromen. Wifi/Bluetooth standaard uit; een reset-/foutpad
mag het USB-budget niet alsnog overschrijden.

| Deel | Gereserveerd vermogen/stroom |
| --- | --- |
| ESP32-S3-module | 3,3 V × 0,50 A |
| W5500 en PHY-omgeving | 3,3 V × 0,20 A |
| OLED | 3,3 V × 0,05 A |
| Overige MAIN-logica en marge | 3,3 V × 0,05 A |
| Hoofdrail totaal | 2,64 W; circa 3,11 W USB-ingang bij aangenomen 85% buckrendement |
| CAN-secundair | 5 V × 0,020 A = 0,10 W |
| S88-secundair | 5 V × 0,035 A = 0,175 W |
| Twee DC/DC's samen | circa 0,79 W ingang bij voorlopig conservatief aangenomen 35% rendement op deze lage belasting |
| CC/bewaking/vermogenspad | 0,10 W reservering |
| USB totaal stationair | circa 4,0 W / 0,80 A bij 5 V; met 20% reserve circa 0,96 A |

Beoogde voeding: 5 V-bron met minimaal 1,5 A Type-C-advertentie.
De 1 A hoofdregelaar heeft bij deze reservering 0,2 A ruimte; dat bewijst
geen voldoende transiëntgedrag. Inrush, kabelverlies, minimum-VBUS,
USB-aansluitcondities en thermiek moeten nog worden berekend en gemeten.
Reserveer in het stand-by-pad alleen de aantoonbaar toegestane stroom
voor CC-detectie en vrijgave; volledige MAIN-belasting niet blind starten.

## 4. GPIO-verkenning

**O:** alleen een resourceverkenning. Geen gerouteerde pinout of netlist.
De gekozen capturemethode kan deze tabel nog wijzigen.

| Signaal | GPIO-kandidaat | Richting t.o.v. MCU |
| --- | --- | --- |
| CAN_RX | 4 | In |
| S88_CLOCK / LOAD / RESET / DATA | 5 / 6 / 7 / 8 | Vier ingangen |
| CAN_VALID_N / S88_VALID | 9 / 10 | Twee ingangen |
| W5500 SCLK / MOSI / MISO | 11 / 12 / 13 | Uit / uit / in, uitsluitend MAIN |
| W5500 CSn / INTn / RSTn | 14 / 15 / 16 | Uit / in / uit, uitsluitend MAIN |
| OLED SDA / SCL | 17 / 18 | I²C, uitsluitend MAIN |
| Knop 1 / knop 2 | 1 / 2 | In |
| OLED reset | 21 | Uit, uitsluitend MAIN |
| USB D− / D+ | 19 / 20 gereserveerd | In v1 voedingsconnector zonder dataverbinding; nog geen USB-programmeerpad |
| UART/debug | 43 / 44 gereserveerd | Alleen MAIN; programmeerfixture en voedingspad apart ontwerpen |
| Parallelcapture/framing/debug | 38–42 gereserveerd | Definitieve functie open; kan botsen met extern JTAG |

**B [S11]:** GPIO0, 3, 45 en 46 zijn strapping-gerelateerd;
bij octal-PSRAM-varianten zijn GPIO35–37 niet vrij beschikbaar.
**O:** deze pins niet aan meetkanalen toewijzen. Modulepadnummers,
geheugenvariant en resettoestanden vóór schema dubbel controleren aan de
exacte moduldatasheet. MCU-bootknop is niet nodig als afzonderlijke
gebruikersknop; programmeerpads kunnen de fixture bedienen.

**B [S20]:** ESP-IDF biedt W5500 MAC-/PHY-drivers. **O:** voorkeur voor
die integratie met ESP-IDF/lwIP en de webserver, in plaats van twee
naast elkaar draaiende TCP/IP-stacks. Versie en API worden pas in de
firmwarefase bevroren. De W5500-hardwaresockets worden niet impliciet
als implementatie gekozen.

## 5. Selectiecriteria vóór een exacte BOM

Een bestelbare variant heeft een passende pinout, behuizing, temperatuur-
en spanningsrange, gegarandeerde maxima waar nodig, bekende verliesstanden,
gedocumenteerde sourcing en correcte KiCad-symbolen/footprints nodig.
Beschikbaarheid of certificaten op componentniveau bewijzen geen
veilige eindschakeling. Prijzen en levertijden zijn hier niet vastgelegd.
