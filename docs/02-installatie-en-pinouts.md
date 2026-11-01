# Installatie en pinoutonderzoek

Datum: 2026-11-01. **Geen aansluithandleiding.** B-tabellen hieronder
beschrijven bronnen, geen vrijgegeven kabel voor de eigen installatie.
Bron-ID's verwijzen naar het [bronnenregister](06-bronnenregister.md).

## 1. Wat over de eigen installatie bekend is

Deze tabel bewaart de informatie van de eigenaar uit dit gesprek [U01].
Er zijn hier geen foto's, meetbestanden of apparaten opnieuw onderzocht.

| Onderdeel | Gegevens | Status |
| --- | --- | --- |
| SRSEII | Gerhard Bertelsmann; Onion Omega2+ | U |
| CAN/S88-controller | PIC18F26K80; device-ID `0x6126`; silicon revision `06` | U; geen afleiding naar PCB-revisie |
| PIC-firmware | 1.27, volgens eerdere verificatie op 2026-10-28 | U; geen broncode/binaire hash aangeleverd |
| SRSEII-print | Eerder documentatie van 1.5, 1.8 en 1.8.1a gebruikt | U; daadwerkelijk gemonteerde printrevisie M |
| Gleisbox | Märklin digitale Gleisbox met MS2, aangesloten op SRSEII | U |
| Gleisbox-artikelnummer | Mogelijk 60113 of 60116; nog niet vastgesteld | M |
| Gleisbox-UID | `0x47459FB0` | U; identificeert geen connectorbezetting of PCB-revisie |
| Aanduiding “1.5” | Eerder gebruikt voor de installatie | U; geen bewezen printrevisie of Gleisbox-type |
| S88-isolator | GBert-zelfbouwprint met opdruk `2020/08` | U |
| Isolatoropschriften | `Zentrale_S88`, `Input_S88`, `U3`, `J1`, `S88` | U |
| CD4504BM | Volgens inspectie aanwezig | U; gemonteerd/los en precieze referentie nog inspecteren |
| Bestukking op 2026-10-28 | Twee RJ45's en U3 nog niet gemonteerd | U; actuele toestand M |
| NSI8141W1 | Bekend van latere uitvoeringen | U; geen bevestiging dat dit het juiste onderdeel is voor 2020/08 |
| S88-kabels, gebruikte poorten en modules | Niet vastgesteld | M |
| Isolator voltooid/aangesloten | Niet vastgesteld | M |
| Actuele S88-voeding | 5 V of 12 V onbekend | M |

De losse melding “CD4504BM aanwezig” vult het ontbrekende U3 niet in.
Onderdeelmarkeringen, referenties en bestukkingsstatus worden afzonderlijk
geregistreerd. Tot die inspectie geldt de isolator niet als een bewezen
werkende scheiding.

## 2. Referentie SRSEII 1.5

**B, uitsluitend [S01]:** schema revisie 1.5, 2020-06-12, blad 1/1.
De titel noemt Gleisbox 60113. Het schema tekent een PIC18F25K80,
MCP2562 en schakelbare R11 van 120 Ω via P7.
De gemelde PIC18F26K80 is dus geen bewijs van gelijkheid.

| Bronconnector | Pennen volgens schemasymbool |
| --- | --- |
| P8 CAN | 1 GND; 2 +5 V; 3 CANL; 4 CANH |
| J2 CAN RJ45 | 1 `br` naar P19.1; 2 `rt` naar P19.2; 3 NC; 4 GND; 5 NC; 6 naar P3.2; 7 CANL; 8 CANH; shield NC |
| J1 S88-N RJ45 | 1 Vdd; 2 DATA; 3/5 GND; 4 CLOCK; 6 LOAD; 7 RESET; 8 niet verbonden |
| P18 spanningskeuze | 2 Vdd; keuze tussen +5 V en uitgang 7812 |

**M:** P19-/P3-bedrading, connectororiëntatie en werkelijke spanningen.
De labels `br`/`rt` zijn geen bewijs van een vaste spanningsrail. J2.6
mag niet zonder controle als GND of voeding worden geïnterpreteerd.
Het lokaal getekende net `+24V` bewijst geen gemeten voedingsspanning.

De 1.8-/1.8.1a-documentatie uit eerder werk is niet als bewijs gebruikt.
Pas na revisie-identificatie wordt het bijbehorende oorspronkelijke ontwerp
geselecteerd. Een nieuwere `master`-netlist mag de bevroren 1.5-PDF niet
stilzwijgend vervangen.

## 3. Referentie isolator van augustus 2020

**B, uitsluitend [S02]:** PDF revisie 1.0, gedateerd 2020-08-30.
Dit is een passende onderzoekskandidaat bij opdruk 2020/08,
geen bewezen match met de eigen print.

| Functie | In deze PDF |
| --- | --- |
| Centralezijde | `Zentrale_S88`; massa GND; voeding +5V |
| Modulezijde | `Input_S88`; massa GNDD; voeding VDD |
| Beide RJ45's | 1 voeding; 2 DATA; 3/5 lokale massa; 4 CLOCK; 6 PSLoad; 7 RESET; 8 NC |
| U2 | 4504, niveauomzetting modulezijde |
| U3 | `Si8641BB-B`, drie besturingskanalen vooruit en DATA terug |
| Modulevoeding | J1, L7805 en J3-keuze tussen ingang en 5 V; J2 heet `5V Bypass` |
| DATA-pad | R4 1k5 en C5 1nF getekend |

**Bronconflict [S03]:** de README noemt U2 `SI8641AB-B-IS1`, U3
`CD4504BM96`, andere R4 en geen C5. Deze README is daarom geen
bestukkingsinstructie voor de onderzochte PDF of de eigen print.

**M:** spoorvergelijking, geïnstalleerd type/kanalenrichting, voedingskeuze,
filterbestukking, massa's en raildata. Geen component uit de 2022-uitvoering
overnemen zonder deze controle. Het RC-net kan timing veranderen; zijn
werkelijke aanwezigheid en effect moeten worden gemeten.

De huidige Hobbymakers-handleiding [S05] beschrijft een aparte 12 V-voeding
en waarschuwt voor omgewisselde aansluitingen. Ze specificeert niet de
eigen 2020/08-print. De ontwerper noemt voor zijn isolator 5 V aan de
SRSEII-zijde [S04]. Beide zijn redenen voor gerichte revisiecontrole;
ze geven geen toestemming om de onbekende print te voeden.

## 4. S88-N als protocolreferentie

**B [S06], RCD-688, 2020-12-06, §2.2:**

| RJ45-contactnummer | Functie | Normale richting op modulebus |
| --- | --- | --- |
| 1 | +5 V / +12 V voeding | Voedingsbron naar modules; analyzer gebruikt dit alleen als meet-/referentiepunt |
| 2 | DATA | Terugmeldketen naar centrale |
| 3 | GND | Lokale referentie |
| 4 | CLOCK | Centrale naar modules |
| 5 | GND | Lokale referentie |
| 6 | PS / LOAD | Centrale naar modules |
| 7 | RESET | Centrale naar modules |
| 8 | RAILDATA | Gleissignaal; geen 5/12 V-logica veronderstellen |
| Scherm | Kabelscherm | Niet verbinden volgens deze referentie |

RCD-688 beschrijft een cascadeschuifregister zonder CRC/pariteit. §3.1:
CLOCK-cyclus >30 µs, HIGH/LOW >15 µs en stijgtijd >300 ns;
module-uitgangvertraging >1 µs en <14 µs t.o.v. de dalende klokflank.
5 V en 12 V zijn voedingsopties, geen bewijs van de niveaus in deze installatie.

**O/M:** registreer eerst flanken en niveaus. Toets daarna de sampleflank,
eerste bit, bitvolgorde, LOAD/RESET-volgorde en scanlengte tegen de echte
PIC-1.27-uitvoer. Een bitstream is nog geen bewezen adresmapping.

**O:** geen afsluitweerstand aan S88 toevoegen. De analyzer bewaart de
bestaande topologie. Een gestandaardiseerde RJ45-bezetting betekent geen
Ethernet. Bij een meetadapter pin 8 uitsluitend binnen het S88-domein
doorvoeren, zonder meetaftakking in versie 1. Op de bronisolator is pin 8
juist NC; dat verschil moet zichtbaar blijven in de installatiekaart.

## 5. Märklin-CAN als protocolreferentie

**B [S07], versie 2.0, 2012-02-07, p.5 en p.39–40:** 250 kbit/s,
29-bit identifier, maximaal acht databytes. De ID bevat vier prioriteitsbits
(2+2), acht commandobits, één responsebit en zestien hashbits.
S88 Polling gebruikt commando `0x10`; S88 Event `0x11`.

**O, afgeleid van deze veldindeling:**

| Veld | Interpretatie van een ontvangen 29-bit ID |
| --- | --- |
| Prioriteit | `(id >> 25) & 0x0f` |
| Commando | `(id >> 17) & 0xff` |
| Response | `(id >> 16) & 1` |
| Hash | `id & 0xffff` |

`0x22` in het document is de combinatie van commando `0x11` en een
responsebit nul in de betreffende CAN-ID-byte; het is geen los nieuw
commando `0x22`. Ruwe ID's blijven altijd bewaard.

**M:** bevestig bitrate, ontvangen frames en PIC-1.27-gedrag door passief
meten en bestaande captures. Geen ping, polling of statusvraag uitsturen.
Een 16-bit hash mag niet zonder meer tot een unieke UID worden verheven.

**B [S08]:** een gebruikelijke HS-CAN-lijn heeft 120 Ω aan beide uiteinden,
gezamenlijk nominaal 60 Ω. **M:** werkelijke terminatoren in Gleisbox,
SRSEII, verdelers en andere nodes zijn onbekend. Een gemeten 60 Ω is een
aanwijzing, geen bewijs van juiste topologie of pinout. Deze analyzer
krijgt geen eigen terminator, ook niet als het bestaande netwerk afwijkt.

## 6. Connectororiëntatie en kabelvrijgave

De tabellen geven elektrische contactnummers van de bron, geen aanzicht
van een kabelstekker. Een socketsymbool is geen footprinttekening.

Voor iedere werkelijk gebruikte poort is nodig:

1. Foto/tekening met printnaam, revisie, connector-ID en kijkrichting.
2. Fabrikanttekening van de concrete connector en markering van contact 1.
3. Continuïteit van ieder contact via de eigen kabel naar de bronnetten,
   gemeten zonder voeding. Geen bewijs op basis van aderkleur alleen.
4. Tabel met “broncontact → kabelcontact → adaptercontact → meetdomein”.
5. Afzonderlijke controle van voeding, GND, scherm en ongebruikte contacten.

Een Mini-DIN-10-pinout van de Gleisbox is in deze fase **niet geverifieerd**.
Er wordt daarom geen adapter voor die connector vastgelegd. Mogelijk kan
een geverifieerde SRSEII-CAN-aftakking worden gebruikt; ook die keuze is open.

## 7. Nog ontbrekende bewijzen

Geen primaire bron verbindt op dit moment ondubbelzinnig alle gevonden
schema's met de geïnstalleerde hardware. Voor G1 zijn de meetresultaten
M01–M07 uit [document 05](05-veiligheid-en-meetplan.md) vereist,
naast een ondertekende/gedateerde bron-naar-printvergelijking.
