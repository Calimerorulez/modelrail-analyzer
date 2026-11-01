# Ontwerpbesluiten

Datum: 2026-11-01. “Vast” betekent een projectgrens, geen bewezen
elektrische implementatie. “Voorlopig” vraagt nog bron-/meetbevestiging.

| ID | Status | Besluit | Reden / voorwaarde voor herziening |
| --- | --- | --- | --- |
| ADR-001 | Vast | Huidige uitvoering blijft uitsluitend documentatie en architectuur | Gebruikersopdracht; fase 2 pas na G1, geen definitieve aansluitingen op onbekende hardware |
| ADR-002 | Vast | Gebruikerswaarneming, bronfeit, ontwerpvoorstel en fysieke verificatie apart labelen als U/B/O/M | Een bronrevisie is geen bewijs van de eigen installatie |
| ADR-003 | Vast | Drie domeinen: MAIN, CAN en modulezijdig S88; twee onafhankelijke geïsoleerde voedingspaden | Bestaande S88-isolator niet via analyzer, USB, Ethernet of debug overbruggen |
| ADR-004 | Vast | Geen CAN-zendpad over de barrière; lokale silent-vastlegging én afzonderlijke recessieve TXD-vastlegging | Firmwarefouten mogen geen actieve zender vrijgeven; MCU listen-only blijft extra beperking |
| ADR-005 | Vast | Geen analyzerterminatie voor CAN of S88, ook geen jumper | Analyzer is meetaftakking; afwijkende bestaande bus eerst onderzoeken |
| ADR-006 | Vast | S88 alleen meten, met eigen lokaal gevoede frontend; geen busvoeding of signaaldriver | Lage belasting en geen centrale-/module-emulatie in v1 |
| ADR-007 | Voorlopig | Hoogohmige comparatoringangen in plaats van direct busgevoede optocouplers | 1 MΩ-doel en timing gezamenlijk halen; type/waarden herzien als uitschakelgedrag of timing faalt |
| ADR-008 | Voorlopig | S88-drempels pas na meting van elk kanaal vastleggen | 5/12 V-voeding bepaalt niet noodzakelijk DATA-HIGH; voedingsevenredige drempel is slechts kandidaat |
| ADR-009 | Vast | Bestaande communicatie loopt niet door actieve analyzerlogica | Uitgeschakelde/verwijderde analyzer mag de verbinding niet onderbreken; passieve adapter afzonderlijk kwalificeren |
| ADR-010 | Vast | Geen RAILDATA-analyse in v1 | Gevraagde vier logische signalen; pin 8 kan andere spanning voeren en blijft buiten logica-ingang |
| ADR-011 | Voorlopig | S3-WROOM-1-N8R8, W5500 met eigen SPI en OLED via I²C | Bufferruimte en gescheiden busbelasting; pinout en acquisition kunnen reserveringen wijzigen |
| ADR-012 | Open | Gemeenschappelijke S88-capturemethode nog kiezen | Vier RMT-kanalen bewijzen geen simultane opname; LCD_CAM vraagt framing-/DMA-analyse; eerst haalbaarheidsbeoordeling |
| ADR-013 | Vast | Capturetijd, frontendvertraging en verliesstatus expliciet maken | Geen softwarecallback als exacte fysieke flank presenteren; ruwe data blijft beschikbaar |
| ADR-014 | Voorlopig | USB-C 5 V sink met stroomadvertentiedetectie en belastingvrijgave | Voorlopig vermogensbudget past niet vanzelf op een standaard USB-bron; definitief pad nog berekenen |
| ADR-015 | Vast | Exacte installatieadapter pas na connector-/kabelkaart | Primaire SRSEII-basis is eigenaar-bevestigde 1.8.1a met S24–S27; 1.5 uitsluitend historisch. De losse isolator 2020/08 houdt haar eigen bron-/meettraject |
| ADR-016 | Vast | Geen NSI8141W1 of andere latere vervanging aannemen voor de bestaande isolator | U3 en refdes verschillen tussen bronnen; fysieke match nodig |
| ADR-017 | Vast | Automatische ERC/DRC verplicht in de bijbehorende latere fasen | Softwarecontrole en fysieke veiligheid hebben afzonderlijke acceptatiecriteria |
| ADR-018 | Vast voor huidige bestanden | Eigen documentatie onder MIT; externe bronnen uitsluitend verwijzen | Open-source start zonder rechten op externe schema's te veronderstellen. Hardwarelicentie vóór eigen KiCad-bestanden bepalen; CERN-OHL-v2 is een te beoordelen optie |
| ADR-019 | Vast, bijgewerkt na publicatie | Lokale commits en push naar bestaande GitHub-repository `Calimerorulez/modelrail-analyzer`, branch main | Origin is ingesteld op de exacte HTTPS-URL; eerdere commits zijn gepubliceerd en remote bevestigd. Nieuwe pushes eveneens remote verifiëren; auteur Codex bij ontbrekende ingestelde identiteit |
| ADR-020 | Vast | Fysieke hardware metingen zijn niet in deze omgeving verricht | Open meetpunten worden niet afgevinkt op basis van aannames of software-uitvoer |
| ADR-021 | Vast | PCB 1.8.1a overnemen als expliciet door eigenaar bevestigde identificatie [U02] | Herroept de eerdere onzekerheid over de eigen SRSEII-revisie; bevestigt geen individuele onderdelen, pinout, kabels, spanningen of terminatie |
| ADR-022 | Vast | Primaire bronset bestaat uit oorspronkelijke 1.81-schema/PCB plus expliciete 1.8.1a-assemblagelijsten op bevroren commit | Geen apart a-schema aangetroffen; bronherkomst, 18/15 µH-verschil en assemblageheaders vastgelegd in document 09. Huidig master 1.82 en legacy .sch 1.7 niet verwisselen met deze set |

Een vervolgwijziging noemt het besluit-ID, de nieuwe bron of meting,
de gevolgen voor eisen/BOM en de relevante Git-commit. Gesloten punten
blijven herleidbaar; oude revisies worden niet achteraf als gelijk voorgesteld.
