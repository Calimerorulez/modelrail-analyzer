# Eisen en fasering

Datum: 2026-11-01. Status: **O**, tenzij een bron- of inspectiestatus is vermeld.
Statusdefinities staan in de [README](../README.md).

## Opdracht en begrenzing

Een zelfstandige analyzer naast de bestaande besturing. De SRSEII blijft
CAN/S88 verwerken; de analyzer observeert de fysieke CAN-bus en S88 aan
de **modulezijde** van de bestaande isolator. De analyzer vervangt of
repareert die isolator niet.

Deze fase levert uitsluitend eisen, architectuur, pinoutonderzoek,
componentkandidaten, veiligheidsanalyse en een verificatieplan.
Er worden geen eigen elektrische aansluitingen definitief gemaakt.
Schema, PCB-layout, firmware, geautomatiseerde decoderproeven en
productiegegevens behoren tot latere fasen.

## Functionele eisen

| ID | Eis | Beoogde acceptatie in latere fase |
| --- | --- | --- |
| F01 | ESP32-S3 als hoofdprocessor; ESP-IDF | Onderdeelvariant, toolchain en afhankelijkheden vastgelegd; reproduceerbare build |
| F02 | Ethernet met W5500 | Webdiagnostiek werkt bij 10 en 100 Mbit/s link; herstel na kabelonderbreking zonder de meetbus aan te sturen |
| F03 | Märklin-CAN ontvangen en interpreteren | Ruwe ID, IDE/RTR, DLC, payload, tijd en verliesstatus blijven beschikbaar; onbekende commando's blijven als ruwe data zichtbaar |
| F04 | CAN uitsluitend ontvangen | Geen data-, ACK-, error-, overload-, discovery- of programmeertransmissies, ook bij boot, reset en verkeerde firmwaremodus |
| F05 | S88 CLOCK, PS/LOAD, RESET en DATA observeren | Frequentie, pulsduur, scaninterval, bitreeks en afwijkingen uit dezelfde tijdlijn afleiden |
| F06 | 5 V en 12 V aan modulezijde ondersteunen | Beide voedingsgevallen afzonderlijk kwalificeren; werkelijke logische HIGH/LOW en drempels meten, niet afleiden uit alleen de voedingsjumper |
| F07 | Geen S88-signaalgeneratie | Geen signaaldriver, pull-up naar analyzerrail of terugwaarts isolatiekanaal naar S88-signalen |
| F08 | Galvanisch gescheiden meetinterfaces | CAN, S88 en hoofdelektronica vormen drie aparte lokale domeinen, met aparte geïsoleerde CAN- en S88-voeding |
| F09 | 1,3-inch OLED en twee gebruikersknoppen | Status, geselecteerde diagnose en verlies-/ongeldigheidsmelding lokaal bedienbaar |
| F10 | USB-C-voeding | Alleen sink; geen voeding uit CAN/S88 en geen terugvoeding naar USB of bestaande installatie |
| F11 | Realtime webdiagnostiek | O-doel: maximaal 500 ms weergavevertraging onder vastgelegde belasting; acquisition heeft voorrang, verliezen zijn zichtbaar |
| F12 | Logging en CSV/JSON-export | Ruwe en afgeleide gegevens met sessie-ID, monotone tijd, tijddefinitie, configuratie, firmwareversie, volgnummer en verliesmarkeringen |
| F13 | Automatische tests met gesimuleerde CAN/S88-data | Latere testfase dekt correcte frames, onbekende ID's, te korte payloads, klokfouten, uitval, bufferoverloop en correlatie |
| F14 | Eigen KiCad-PCB en open bronnen | Bewerkbare bronbestanden, bibliotheken, besluiten, licenties, checks en meetverslagen in Git |

Een webinterface ontvangt netwerkverkeer en mag daarop antwoorden. Dat geeft
haar geen mogelijkheid om CAN/S88 aan te sturen. Geen gateway-, replay- of
besturingsfunctie opnemen in versie 1.

## Elektrische en meeteisen

Alle onderstaande getallen zijn **O-doelen** voor dimensionering en latere
kwalificatie. Het zijn geen eigenschappen van bestaande of ontworpen hardware.

| ID | Voorstel | Verificatie |
| --- | --- | --- |
| E01 | Geen CAN-afsluitweerstand, split termination of inschakelbare terminatie in analyzer of meetadapter | Schema-/netlistreview; onbelaste adapterweerstand; vergelijking bus vóór/na |
| E02 | CAN-zendpad fysiek ontbreekt tussen MCU en transceiver | TXD lokaal recessief vastgelegd; silent-ingang lokaal vastgelegd; geen ombouwjumper of TX-testconnector |
| E03 | CAN-ontvangstbelasting inclusief bescherming/kabel expliciet berekend | Rdiff, capaciteit, common-mode, voedingsloze lekstroom en stubgedrag toetsen aan echte bus |
| E04 | S88 per signaalkanaal minimaal 1 MΩ DC-ingangsweerstand in normaal bedrijf én zonder analyzervoeding | Volledige ingang meten, inclusief beveiliging; niet alleen weerstandswaarde op BOM |
| E05 | S88 per kanaal maximaal 15 pF toegevoegde capaciteit aan de aftakking, inclusief gekozen probe/adapter | Impedantiemeting en flankenvergelijking; bronimpedantie en kabel meerekenen |
| E06 | Extra S88-signaalstroom maximaal 15 µA per kanaal bij 12 V; voedingspin alleen een gedocumenteerde hoogohmige meting/referentie | Alle in-/uitgeschakelde toestanden; totaalbelasting van vier kanalen plus voedingsdetectie registreren |
| E07 | Geen ongecontroleerde terugvoeding bij deelvoeding | USB aan/uit, CAN/S88 aan/uit en spanningsopbouw/-afbouw in alle combinaties testen |
| E08 | Voorlopig 8 mm vrijhoudgebied tussen elektrische domeinen op alle koperlagen, te toetsen aan componenten en werkspanning | Exacte isolatie-, kruip-/luchtweg- en fabricageregels pas in fase 2 vastleggen; geen veiligheidsclassificatie hieruit afleiden |
| E09 | CAN/S88-afscherming en montage brengen geen ongewenste massaverbinding tot stand | Inspectie met USB, Ethernet, behuizing en debugverbindingen aanwezig |
| E10 | Geen S88-eindterminatie toegevoegd | Geen 120 Ω op S88; bestaande serieweerstanden of filters blijven ongewijzigd |
| E11 | CAN/S88 worden niet door een actief deel van de analyzer doorgelust | Bestaande communicatie blijft werken bij verwijderen of uitschakelen van het hoofdapparaat; eventuele meetadapter heeft uitsluitend gecontroleerde directe doorverbindingen |
| E12 | Probe aangesloten: geen functionele verandering en geen betekenisvolle afname van gemeten marge | Zelfde bedrijfspatroon, gemeten LOW/HIGH, flanken, samplemarge en fouttellingen vóór/na; voorlopig amplitudeverschil ≤2%, flank-/timingverschil ≤5%, én alle oorspronkelijke marges blijven geldig |

E12 is pas bruikbaar met vastgelegde meetonzekerheid. Een resultaat binnen
deze percentages kan alsnog onacceptabel zijn bij een al marginale bus.
Geen ontvangstfouten zien bewijst geen elektrische veiligheid.

## Tijdgedrag en capaciteit

- **O:** S88-ontwerpenvelop: maximaal 50 kHz CLOCK, HIGH en LOW elk minimaal
  10 µs. Dit is ontwerpreserve; het is geen bewering over de SRSEII-firmware.
- **B:** RCD-688 §3.1 vraagt CLOCK-cyclus >30 µs en HIGH/LOW elk >15 µs;
  het protocolonderzoek staat in [document 02](02-installatie-en-pinouts.md).
- **O:** Gezamenlijke sampletijd voor de vier S88-kanalen, beoogd 2 MS/s;
  digitale tijdraster 0,5 µs. Frontendvertraging en kanaalverschil komen daar
  nog bij. Voorlopig frontendoel: vertraging ≤3 µs en kanaalverschil ≤0,5 µs,
  te bewijzen bij de werkelijk gemeten flanken en niveaus.
- **O:** CAN-tijdstempel aanvankelijk definiëren als ontvangstcallback/ISR-tijd
  na frameontvangst. Geen SOF- of hardwaretijdstempel beloven voordat de
  gekozen ESP-IDF-versie en S3-hardware dat aantoonbaar ondersteunen.
- **O:** CAN/S88-correlatie bevat onzekerheidsmarge; isolatorfilters en
  SRSEII-PIC-firmware kunnen extra vertraging geven. Niet elke CAN-melding
  correspondeert rechtstreeks met één S88-flank.
- **O:** Logging gebruikt begrensde buffers en expliciete verliesmarkeringen.
  Bij 2 MS/s en één byte per sample ontstaat 2 MB/s; 4 MiB buffer bevat
  ongeveer 2,10 s. Ethernetlink en SPI-doorvoer garanderen geen onbeperkte
  verliesvrije uitvoer. Langdurige logging gebruikt samenvattingen en events;
  ruwe captures zijn begrensd. Permanente opslagcapaciteit blijft een open eis.

## Fasen en overdrachtscriteria

| Fase | Resultaat | Voorwaarde voor volgende fase |
| --- | --- | --- |
| 1 — nu | Documentatie, brononderzoek, architectuur en meetplan | G1: echte interfaces, voeding, isolatie, terminatie en meetenvelop bekend; open punten met impact op schema gesloten |
| 2 | KiCad-schema, eigen symbolen, berekeningen en exacte BOM | G2: peer-/elektrische review, automatische ERC met bewaarde rapportage, geen onopgeloste relevante overtredingen |
| 3 | PCB-layout | G3: automatische DRC, schema-pariteit, isolatie-/mechanicareview en productiecontrole; prototype mag nog niet op de baan |
| 4 | ESP-IDF-firmware en lokale testsoftware | G4: receive-only configuratie en bounded acquisition aangetoond op testopstelling; geen echte baan als eerste test |
| 5 | Simulatietests en hardwarekwalificatie | G5: automatische tests plus fysieke veiligheids-, fout- en niet-beïnvloedingsproeven geslaagd |
| 6 | Productiegegevens en release | Reproduceerbare revisie, gehashte outputs, bronlicenties, ERC/DRC-rapporten en hardwaremeetverslag aanwezig |

Een nieuwe fase wordt niet automatisch gestart omdat de tekst van een
vorige fase gereed is. G1 is nu **open**. De fysieke inspectie en metingen
zijn vanuit deze omgeving niet uitgevoerd.

## Nog te bepalen gebruikseisen

Meetbereik voor spanningstransiënten; kabellengte/probelengte;
aantal terugmeldbits en scantijd; bewaartijd en opslagmedium; omgevingstemperatuur;
behuizing; EMC-doelen; toegestane tijdfout van correlatie; gewenste
webclientaantallen. Gebruik hiervoor de echte installatie en metingen.
