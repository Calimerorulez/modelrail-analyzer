# Modelrail Analyzer

Open-source diagnosemodule voor Märklin H0, naast een bestaande
SRSEII/Gleisbox en S88-isolator. Beoogd: ESP32-S3, W5500, uitsluitend
ontvangende geïsoleerde CAN-ingang, vier hoogohmige geïsoleerde
S88-meetkanalen, 1,3-inch OLED, twee knoppen en USB-C-voeding.

**Status op 1 november 2026: uitsluitend fase 1, documentatie en
architectuur. Er is geen bouwbaar schema, PCB of firmware. De aansluiting
op de werkelijke installatie is nog niet vrijgegeven.**

De aanduiding “1.5” bewijst geen printrevisie. Het oorspronkelijke
SRSEII-1.5-schema is onderzocht als referentie. De gemelde isolatorprint
2020/08 wordt apart vergeleken met het oorspronkelijke schema van
30 augustus 2020. Andere revisies zijn geen bewijs voor deze installatie.

## Documentatie

| Document | Inhoud |
| --- | --- |
| [Eisen en fasering](docs/01-eisen-en-fasering.md) | Scope, eisenmatrix, acceptatie en fasegrenzen |
| [Installatie en pinouts](docs/02-installatie-en-pinouts.md) | Inspectiegegevens, bronpinouts, revisieverschillen en open vragen |
| [Hardwarearchitectuur](docs/03-hardwarearchitectuur.md) | Elektrische domeinen, ontvangstpaden, voeding en meetgrenzen |
| [Componentkeuze](docs/04-componentkeuze.md) | Voorlopige selectie, alternatieven en GPIO-reserveringen |
| [Veiligheid en meetplan](docs/05-veiligheid-en-meetplan.md) | Foutanalyse, fysieke verificatie en meetlog |
| [Bronnenregister](docs/06-bronnenregister.md) | Primaire bronnen, revisies, vindplaatsen en bronconflicten |
| [Ontwerpbesluiten](docs/07-ontwerpbesluiten.md) | Besluiten, redenen en voorwaarden voor herziening |
| [Overdracht en validatie](docs/08-overdracht-en-validatie.md) | Open punten, vervolgfasen, geplande ERC/DRC en actuele controlestatus |

## Betekenis van statussen

- **U — gebruikerswaarneming:** door de eigenaar gemeld, niet hier opnieuw gemeten.
- **B — bronbevestigd:** aantoonbaar in de genoemde bron en revisie; toepassing op de eigen print blijft apart te bewijzen.
- **O — ontwerpvoorstel:** eigen eis, keuze, berekening of aanname; nog geen geteste eigenschap.
- **M — meten/inspecteren:** nog open; er is geen meetresultaat beschikbaar.

“Passief” betekent hier: geen busbesturing en beperkte elektrische belasting.
Het betekent geen nulbelasting. De ontvangers gebruiken een eigen voeding.
Ook een hardwarematig geblokkeerde CAN-zender sluit niet iedere
componentfout of kabelkortsluiting uit.

## Eerstvolgende stap

Voer de installatie-inspectie en metingen M01–M07 uit uit het
[meetplan](docs/05-veiligheid-en-meetplan.md). Leg printrevisies,
connectororiëntatie, kabelcontinuïteit, voedingsniveaus, isolatie en
CAN-terminatie vast. Pas na sluiting van de overdrachtscriteria volgt
een afzonderlijke KiCad-schemafase.

De automatische KiCad ERC en DRC zijn **nog niet uitgevoerd**: eigen
KiCad-bestanden ontbreken in deze fase. Hun toekomstige uitslagen worden
gescheiden gehouden van de fysieke elektrische beoordeling.

## Open source

De eigen documentatie in deze repository valt onder de [MIT-licentie](LICENSE).
Externe schema's en datasheets behouden hun eigen rechten. Ze worden
gelinkt en niet in deze repository heruitgegeven. Er is nog geen remote
repository aangemaakt of gepubliceerd. De licentie voor toekomstige
hardwarebestanden wordt vóór fase 2 afzonderlijk vastgelegd.

