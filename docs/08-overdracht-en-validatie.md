# Overdracht en validatiestatus

Datum: **2026-11-01**. Uitgevoerde scope: documentatie en architectuur.
Installatievrijgave G1: **open**. Geen definitieve elektrische verbinding,
schema, PCB-layout, firmware of productiebestand gemaakt.

## 1. Eerst te sluiten punten

| ID | Open punt | Bewijs voor sluiting | Verantwoordelijke vervolgstap |
| --- | --- | --- | --- |
| G1-01 | Eigen SRSEII-revisie, Gleisbox-artikelnummer en isolatorbestukking | M01, bron-naar-printvergelijking en actuele toestand | Eigenaar inspecteert; documentatie daarna vergelijken |
| G1-02 | Connectororiëntatie, kabels en gebruikte poorten | M02 met volledige contactkaart; geen Mini-DIN-aanname | Eigenaar/uitvoerder meet; adapterkeuze daarna uitwerken |
| G1-03 | Werkelijke galvanische scheiding en voedingspaden | M03; geen onverklaarde massa-/scherm-/debugbrug | Inspectie en gemeten domeinkaart |
| G1-04 | CAN-terminatie en topologie | M04; plaats en toestand van bestaande afsluitingen bekend | Bestaande installatie spanningsloos onderzoeken |
| G1-05 | CAN-/voedingsenvelop | M05/M06; niveaus, common-mode, transiënten, bitrate en aftaklocatie | Geïsoleerde fysieke metingen en passieve capture |
| G1-06 | S88-niveaus, timing en drempelkeuze | M05/M07; afzonderlijk DATA/control; verklaarde LOAD/RESET-/bitrelaties | Eerst werkende isolator/moduleketen vaststellen, daarna meten |
| G1-07 | Haalbare gemeenschappelijke capture | Vastgelegde peripheral-/DMA-/bufferanalyse met begrensde tijdonzekerheid en resourcekeuze | Technisch vervolgonderzoek binnen documentatiefase; nog geen firmware bouwen |
| G1-08 | Ingangsbeveiliging en voedingsgedrag dimensioneerbaar | Bekende fout-/transiëntgrenzen; componentmaxima en alle voedingsstanden doorgerekend | Na installatiegegevens exacte frontend- en voedingskeuze uitwerken |
| G1-09 | Mechanica, meetkabelbudget, opslagduur en bronlicenties | Vastgelegde gebruikseisen en licentiebesluit voor toekomstige hardware | Vóór eigen schema/componenten vrijgeven |

De huidige aflevering maakt de open punten reviewbaar, maar sluit ze
niet. De eerste fysieke actie is inspectie M01, niet het aansluiten van
een generieke CAN- of S88-adapter.

## 2. Wat in deze fase is gecontroleerd

| Onderdeel | Uitkomst |
| --- | --- |
| Bestaande repository | Werkmap was leeg en had geen Git-repository; lokaal `main` geïnitialiseerd |
| Lokale instructies | Geen toepasselijke AGENTS.md gevonden in de werkmap of bovenliggende paden |
| Primaire SRSEII-/isolatorbronnen | Bevroren PDF's visueel onderzocht; bronconflict en revisiebeperking vastgelegd |
| CAN-protocol | Officiële 2.0-PDF gedownload, relevante pagina's bekeken; geen live busmeting |
| S88-protocol | RCD-688 rechtstreeks bij RailCommunity geraadpleegd |
| ESP32-module | v1.8-variant-/pintabellen en PSRAM-/strapbeperkingen gecontroleerd |
| KiCad-tooling | CLI **10.0.7** gevonden op `/Applications/KiCad/KiCad.app/Contents/MacOS/kicad-cli`; `version`, `sch erc --help` en `pcb drc --help` uitgevoerd |
| KiCad-configuratie tijdens toolingcontrole | Tijdelijke `KICAD_CONFIG_HOME` in `tmp/kicad-config` gebruikt; eerste versieaanroep meldde niet-schrijfbare standaardpreferences, herhaling met tijdelijke config zonder foutmelding |
| Documentcontrole | Relatieve links, bron-ID's, fasegrens, rekenvoorbeelden en Git-whitespace bij afronding gecontroleerd |
| Automatische ERC | **Niet uitgevoerd:** eigen schema ontbreekt volgens de fasegrens |
| Automatische DRC | **Niet uitgevoerd:** eigen PCB ontbreekt volgens de fasegrens |
| Firmware-/simulatietests | **Niet uitgevoerd:** softwarefase nog niet gestart |
| Elektrische veiligheid / werkelijke pinouts | **Niet fysiek geverifieerd:** geen toegang tot de hardware |

Een geslaagde CLI-versie-/helpaanroep is geen ERC-/DRC-resultaat.
Er zijn geen groene fictieve checkrapporten of lege proef-PCB's gemaakt.

## 3. Automatische KiCad-controles in de volgende fasen

De onderstaande commando's zijn een **uitvoeringsplan**, gecontroleerd
tegen de beschikbare CLI-help. De bestandspaden zijn toekomstige
projectnamen en bestaan nu niet. KiCad-versie en bibliotheken bij G2/G3
bevriezen; bij een andere versie de opties opnieuw controleren.

```sh
mkdir -p reports
kicad-cli sch erc --severity-all --exit-code-violations --format json \
  --output reports/erc.json hardware/modelrail-analyzer.kicad_sch
kicad-cli pcb drc --refill-zones --schematic-parity --all-track-errors \
  --severity-all --exit-code-violations --format json \
  --output reports/drc.json hardware/modelrail-analyzer.kicad_pcb
```

Op deze Mac is het absolute CLI-pad hierboven bruikbaar als `kicad-cli`
niet in PATH staat. Alleen in de schemafase de ERC draaien; DRC wordt
verplicht zodra er een eigen PCB is. Het ontbreken van verwachte
ontwerpbestanden laat een latere pipeline falen en levert geen “geslaagd”.

De latere CI moet automatisch draaien bij wijziging van schema, PCB,
libraries of regels. Bewaar exitcodes, JSON-rapporten, ontwerpcommit,
toolversie en hashes als buildartefact. Standaardwarnings en uitsluitingen
zichtbaar houden. Geen fouten wegdrukken met brede ERC-/DRC-uitsluitingen,
onjuiste pinsoorten of ongefundeerde PWR_FLAG-symbolen.

Controleer vervolgens buiten ERC/DRC:

- Datasheetpinout tegenover symbool én footprint, inclusief componentvariant.
- Geen nets over barrières; echte kruip-/luchtwegen, montage en schermpaden.
- Hardwarematige silent/TXD-vastlegging zonder MCU- of jumperpad.
- Alle voedingsvolgorden, clampstromen, dissipatie en transiëntmarges.
- Hoogohmigheid, capaciteit en niet-beïnvloeding op de complete hardware.

Rapporten in `reports/` zijn lokale uitvoer; de uiteindelijke release krijgt
gehashte rapporten als releaseartefact en een getrackt verificatieverslag
met uitslagen en meetbestanden. Externe datasheets worden daarbij niet
zonder rechten heruitgegeven.

## 4. Vooruitblik op software- en testfase

Dit is uitsluitend een acceptatieplan, geen nu gestarte implementatie.

| Testgroep | Later te simuleren of meten |
| --- | --- |
| CAN-decoder | EFF-ID-velden, responsebit, DLC 0–8, verkorte/onbekende payloads, standaard-ID/RTR, bytevolgorde en ruwe-dataretentie |
| S88-decoder | Meerdere scanlengten, eerste bit, beide datatoestanden, ontbrekende/extra klok, LOAD/RESET-afwijking, lange stilte en drempelchatter |
| Tijdgedrag | Gelijktijdige gebeurtenissen, counterwrap/reset, frontendvertraging, callbackscheduling en kanaalalignering |
| Doorvoer | CAN op werkelijke lijnsaturatie plus S88-maximumbereik, Ethernet op 10/100, meerdere clients en verbroken verbinding |
| Logging | CSV-escaping, JSON-validiteit, sessie/volgnummers, tijddefinitie, configuratiemetadata, ongeldigsignaal en verlies-/overloopmarkering |
| Receive-only | Bron-/API-review plus fysieke foutproeven M11/M12; softwaretest alleen bewijst geen elektrisch zwijgen |
| Referentiedata | Synthetische, verklaarde fixtures en later eigen captures met installatie-/meetmetadata; bestaande captures alleen met bekende herkomst |

Simulatie stopt aan de digitale interface. De bankproeven M08–M13 en
de vergelijking met de echte installatie blijven afzonderlijk vereist.

## 5. Git- en releasebeleid

Elke inhoudelijke wijziging krijgt een gerichte commit. Identificeer
bronrevisies en beslissingen in de wijziging. Geen persoonlijke Git-auteur
verzinnen: zolang er geen auteur ingesteld is, wordt voor deze commits
`Codex <codex@localhost>` gebruikt met opdrachtlokale opties, zonder
globale Git-configuratie te veranderen.

Bij G1 opnieuw de actuele Git-status, gesloten/open eisen en eigen
hardwaregegevens vastleggen. Een latere productierelease omvat exacte
schema-/PCB-/firmwarecommits, BOM, bibliotheken, gerbers, boringen,
plaatsingsbestanden, assemblagetekeningen, ERC/DRC, meetverslag en
licenties. Geen productie-export vanuit een onvrijgegeven revisie.
