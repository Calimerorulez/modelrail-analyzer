# SRSEII 1.8.1a versus historische 1.5

Datum: 2026-11-01. Dit is fase-1-brononderzoek, geen eigen schema of
aansluithandleiding. U/B/O/M hebben de betekenis uit de [README](../README.md).
Bron-ID's staan in het [register](06-bronnenregister.md).

## 1. Identificatie en oorspronkelijke bronset

**U [U02]: de eigenaar bevestigt dat de gebruikte GBert-SRSEII-print
PCB-revisie 1.8.1a is. Revisie 1.5 is niet de eigen hardware.**
Die bevestiging wordt als hardware-identificatie overgenomen. Connector-
en kabelcontinuïteit, jumperstanden, componentvervangingen en werkelijke
spanningen blijven afzonderlijke fysieke verificaties.

Er is geen apart bestand `SRSE2_V1.8.1a.pdf` aangetroffen in de onderzochte
GBert-directory en relevante bestandsgeschiedenis. Er is wel deze bronset:

| Onderdeel | Oorspronkelijk bestand / bewijs | Betekenis |
| --- | --- | --- |
| Native schema | `SRSE2.kicad_sch` op commit `4562014c9548e4880c6216b4975b004596e937e1` [S24] | Titelrevisie **1.81**, datum 2022-12-08; primaire elektrische bron binnen de 1.8.1a-bronset |
| Native PCB | `SRSE2.kicad_pcb` op dezelfde commit [S24] | Titelrevisie **1.81**; silkscreentekst `V1.8`; pads/netten van relevante aansluitingen onderzocht |
| Expliciete a-variant | `SRSE2_BOM_V1_8.1a.csv` en `SRSE2_CPL_V1_8.1a.csv` [S26] | Assemblagelijsten voor 1.8.1a, inclusief P00/P01 |
| Herkomst a-lijsten | Commit `b2ad6cd00db5ced726d1335ca48bb7aa2e657b3f`, 2023-09-18 [S27] | Voegt deze twee bestanden toe met omschrijving “add production file with 2mm pin header”; geen nieuw schema of PCB in die commit |
| Schema-PDF | `SRSE2_V1.8.1.pdf` [S25] | Titelrevisie 1.81; visuele familiereferentie, niet onder een verzonnen a-bestandsnaam citeren |
| Bestaande productiearchive | `SRSE2_V1.8.1_JLCPCB_Assembly_service.ZIP` [S26] | Bevat 1.8.1-BOM/CPL en een Gerber-ZIP met koper, maskers, silkscreens, contour en boringen; geen afzonderlijke a-Gerberrelease vastgesteld |

**B [S27]:** P00/P01 zijn toegevoegd als 2 mm pinheader-/socket-
assemblageregels. **O-conclusie:** de a-aanduiding is in deze bronnen
herleidbaar als assemblagevariant binnen de 1.81-familie; er is geen
afzonderlijk gewijzigd a-schema aangetoond. Dit is geen bewijs dat elk
gebouwd exemplaar exact de gekozen BOM volgt.

De huidige native bestanden op onderzochte `master`-commit
`fafa332b76237878faeb6a7a11081ed7df4d46aa` hebben titelrevisie **1.82**.
Het nog aanwezige legacybestand `SRSE2.sch` draagt **1.7**. Beide worden
daarom niet gebruikt als ongemarkeerde vervanging van de primaire bronset.
De oorspronkelijke 1.5 is vastgelegd op commit
`81b6241cb9d47778daf0326e21210120ba24c2a5` [S01].

## 2. Elektrische vergelijking

De kolommen hieronder beschrijven **bronontwerpen**, niet nieuwe metingen
aan de eigen installatie. 1.8.1a verwijst hier naar de hierboven beschreven
combinatie van 1.81-schema/PCB en a-assemblagelijsten.

| Onderwerp | Historische 1.5 [S01] | Primaire 1.8.1a-bronset [S24–S27] | Betekenis voor analyzer / nog open |
| --- | --- | --- | --- |
| CAN-transceiver U3 | Waarde MCP2562; VDD op +5 V, VIO op VCC | SN65HVD230; voeding op VCC; Vref NC; Rs naar GND | Ander IC en voedingsconcept; busniveaus/common-mode fysiek meten. VCC is als 3,3 V ontworpen |
| CAN-functie bestaande SRSEII | PIC CANTX/CANRX verbonden met transceiver | Eveneens CANTX/CANRX; Rs laag | Bestaande SRSEII blijft een actieve busdeelnemer. De analyzer krijgt zijn eigen geïsoleerde, hardwarematig geblokkeerde zendpad |
| CAN P8 | 1 GND; 2 +5 V; 3 CANL; 4 CANH | Dezelfde netbezetting en JST-PH-footprintnaam | De +5 V-pin wordt niet automatisch 3,3 V doordat U3 verandert; geen voeding voor analyzer aftappen |
| CAN RJ45 J2 | 1 br; 2 rt; 3/5 NC; 4 GND; 6 naar P3.2; 7 CANL; 8 CANH; scherm NC | Dezelfde netbezetting | br/rt en P3.2 vragen kabelcontrole; geen voedingsrail aan een aderkleur toekennen |
| CAN-terminatie | R11 120 Ω, via P7 over CANH–CANL schakelbaar | Dezelfde schakeling en waarde | Jumperstand en andere busafsluitingen blijven M04; analyzer krijgt geen terminatie |
| S88 RJ45 J1 | 1 Vdd; 2 DATA; 3/5 GND; 4 CLOCK; 6 LOAD; 7 RESET; 8 naar P15; scherm NC | Dezelfde netbezetting | Pin 8 is een los test-/aansluitpad naar P15, geen aangetoonde RAILDATA-generator |
| S88-controlniveau | U8 4504 met Vdd en VCC; CLOCK/LOAD/RESET vanaf PIC | U8 CD4504 met dezelfde hoofdkanalen en voedingstoewijzing | Niveauomzetting is geen galvanische scheiding; modulezijdige analyzer blijft achter de aparte 2020/08-isolator |
| S88 DATA | R24 1k5 serie; R25 18k naar GND | R24 1k5; R25 **47k** | Minder sterke pulldown. Ontvangstmarge en timing niet zonder meting gelijkstellen; DATA-HIGH niet uit busvoeding afleiden |
| Overige 4504-kanalen | Naar uitbreidingsheaders P14/P16 | Niet aangesloten; P14/P16 vervallen in deze bron | Geen oude uitbreidingsheader als bestaand meetpunt aannemen |
| S88 12 V-regelaar | U9 7812 | U9 **LM317**, met R26 240 Ω, R27 2k2 en R28 39k | Als circa 12 V ontworpen; werkelijke uitgang en voedingsmarge meten |
| S88-keuze P18 | 1 regelaaruitgang; 2 Vdd; 3 +5 V | Dezelfde keuze, nu tussen LM317-uitgang en +5 V | 1–2 versus 2–3 is een bronpinout, geen vastgesteld aanzicht/jumperstand van de eigen print |
| Logicarail VCC | U10 OKI-78SR-module, uitgang als 3V3 aangeduid | Geïntegreerde **U7 TPS5430DDA**-buck naar VCC | Andere voeding en storingseigenschappen; geen externe analyzerrail hierop aansluiten |
| Gleisboxvoeding P13 | 1 +24V-netnaam; 2 GND; 3 +5 V | Dezelfde netbezetting; PCB-opdruk onder meer `18V` | Netnaam en silkscreen zijn geen gemeten spanning; M05 blijft open |
| PIC U4 | PIC18F25K80; footprint SOIC-28; kristal 16 MHz | PIC18F25K80; footprint **SSOP-28**; kristal 16 MHz | Broncomponent blijft F25; eerder gemelde eigen F26 blijft U-informatie en apart bestukkingsverschil |

R21 is in beide ontwerpen de afzonderlijke RS485-afsluitweerstand en
is geen tweede CAN-terminator. Een BOM-regel met R11 én R21 van 120 Ω
bewijst dus geen 60 Ω CAN-belasting op één print.

**B [S28]:** SN65HVD230 is een 3,3 V-HS-CAN-transceiver. Rs aan GND
selecteert de actieve high-speedmodus; alleen het bestaan van een
stand-byfunctie maakt de oorspronkelijke SRSEII-aansluiting niet receive-only.
**O:** de eigen TJA1051T/3-kandidaat hoeft niet gelijk te zijn aan de
SRSEII-transceiver. Compatibiliteit volgt uit de gemeten busenvelop en
de ontvangstmarges, niet uit een gelijke onderdeelnaam.

## 3. Varianten en verschillen binnen de bronset

De schema-PDF van december 2022 tekent L100 als **18 µH**. De native
schema-/PCB- en a-lijsten op de gekozen commit van 2023-11-22 noemen
**15 µH**. De Git-geschiedenis [S27] documenteert de wijziging wegens
onderdeelbeschikbaarheid. Welke spoel in de eigen print zit, is niet
door alleen de revisiebevestiging vastgesteld.

P00/P01 staan als plaatsingsregels in de a-lijsten, maar niet als
zelfstandige footprintreferenties in het onderzochte native PCB-bestand.
De 1.8.1-archive bevat lijsten zonder deze a-toevoeging. Dit wordt
zichtbaar gehouden; er wordt geen volledig identieke, afzonderlijk
vrijgegeven a-productieset geclaimd.

De eerdere tekst “J1.8 niet verbonden” voor de SRSEII was te grof:
beide bronrevisies verbinden dit contact met P15. Het is niet verbonden
met een spoor-signaalbron in het onderzochte schema. De **losse
S88-isolator** heeft volgens haar eigen schema wel pin 8 NC; dat is een
ander apparaat en mag niet worden samengevoegd met de SRSEII-pinout.

## 4. Onderzoeksmethode en geldigheidsgrens

De bevroren 1.5- en 1.81-PDF's zijn visueel vergeleken. Van het
oorspronkelijke 1.81-schema is alleen een lokale weergave en een
leesnetlist gemaakt. Connectornetten zijn vergeleken met de padnetten
in het oorspronkelijke PCB-bestand. Automatisch benoemde netten zijn
op hun betrokken contacten vergeleken; bijvoorbeeld J2.6–P3.2 en
J1.8–P15 blijven dezelfde verbinding ondanks gewijzigde netnamen in
een export. Dit is geen controle van iedere fysieke koperbaan.

De eigen PCB-revisie is hiermee **U-bevestigd**, de bronpinout **B-bevestigd**,
en de fysieke pinout-/kabel-/spanningsverificatie blijft **M-open**.
De PIC-bron/bestukkingsafwijking en de spoelvariant horen bij M01;
kijkrichting en kabel bij M02; scheiding bij M03; P7/terminatoren bij M04;
rails en busniveaus bij M05–M07.

Alle oorspronkelijke CAD-/productiebestanden blijven onderzoekskopieën
onder `tmp/`, buiten Git. Er is geen eigen analyzerschema, PCB of
productie-export gemaakt en geen ERC/DRC-resultaat als veiligheidsoordeel
gebruikt. De fasegrens en overdrachtscriteria blijven van kracht.
