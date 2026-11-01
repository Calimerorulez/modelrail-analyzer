# Veiligheidsanalyse en fysieke verificatie

Status: **O-plan**. **Geen fysieke meting in deze sessie uitgevoerd.**
Geen punt hieronder geldt als geslaagd doordat er een datasheet,
continuïteitspiep, softwaretest of toekomstige ERC/DRC bestaat.

## 1. Veiligheidsgrens

Het ontwerp is bedoeld voor een laagspanningsmodelbaan. De maximale
continue domeinverschillen, transiënten en foutenergie van de eigen
installatie zijn nog onbekend. Functionele galvanische isolatie is het
architectuurdoel. Er wordt geen netspannings-, medische of andere
gecertificeerde beschermingsfunctie geclaimd.

Voor aansluiting eerst revisies, voedingspaden en signaalreferenties
vaststellen. De op 28 oktober gemelde onvolledige isolator is geen
bruikbaar aansluitpunt totdat opbouw en werking afzonderlijk zijn gecontroleerd.
Deze opdracht omvat geen blind bestukken of voeden van die print.

Tijdens inspectie en continuïteitsmetingen: alle betrokken voedingen,
USB, Ethernet-/PoE-bronnen, boosters en andere mogelijke energiebronnen
los; restspanning controleren. Geen weerstand meten op een ingeschakelde
bus en geen jumper onder spanning verzetten.

Bij ingeschakelde metingen uitsluitend geschikt differentieel of
galvanisch geïsoleerd meetgereedschap gebruiken. De aardclip van een
gewone oscilloscoop kan CAN-, S88-, baan- en USB-massa kortsluiten.
De beschermingsaarde van een instrument wordt niet onderbroken om dit
te omzeilen. Meetapparatuur hoort in de domeintekening.

## 2. Voorlopige foutanalyse

H = risico op schade of verstoring van de bestaande installatie;
D = risico op misleidende diagnostiek. Dit is een kwalitatieve analyse;
geen uitvalkansen of kunstmatige RPN-scores invullen zonder gegevens.

| ID | Fout | Gevolg | Ontwerp-/procesmaatregel | Vereist bewijs en resterend risico |
| --- | --- | --- | --- | --- |
| R01 | CAN, S88 en Ethernet-RJ45 verwisseld | H: voeding op verkeerde ingang | Verschillende analyzerconnectoren; gevalideerde, gelabelde adapters | Contactkaart en mechanische review; geen universele fouttolerantie geclaimd |
| R02 | Spiegelbeeldige pinout of afwijkende kabel | H: kortsluiting/verkeerde polariteit | Contactnummer en kijkrichting vastleggen; alle aders doormeten | M02; aderkleuren zijn onvoldoende |
| R03 | S88-module-GND met centrale- of CAN-GND verbonden | H: bestaande isolatie overbrugd | Drie domeinen; geen gedeelde secundaire rail | M03; later netlist-, PCB- en systeeminspectie |
| R04 | MCU boot, reset of verkeerde TWAI-modus | H: ACK/data/errorframes | Silent lokaal vast, TXD afzonderlijk recessief, alleen heenrichting RX | Later test met foutieve firmware op losse testbus; hardware blijft begrensd |
| R05 | CAN-transceiver, TVS of connector kortgesloten | H: busvervorming of blokkade | Seriestroombegrenzing waar haalbaar; bescherming achter begrenzing | Foutproef uitsluitend op bank; connectorfout vóór begrenzing blijft risico |
| R06 | Extra of ontbrekende CAN-terminatie | H/D: reflecties, hogere belasting | Geen eigen terminatie; bestaande terminatoren identificeren | M04; circa 60 Ω alleen bewijst geen topologie |
| R07 | Lange CAN-/S88-aftakking | H/D: reflecties, vervormde klok | Aftakking kort houden; capaciteit inclusief kabel budgetteren | M06/M07 en later M10; lengte pas na meting vrijgeven |
| R08 | 12 V op ingang die alleen 5 V verdraagt | H: ingangsschade en terugwerking | Hoogohmige delers en juiste protectie; alle vier kanalen kwalificeren | M05/M07 voor echte niveaus; later 5 V/12 V-bankproeven |
| R09 | S88-voeding 12 V maar DATA-HIGH lager | D: gemiste bezetmeldingen | Drempels uit metingen kiezen; eventueel per kanaal | G1-06; geen automatische foutconclusie uit een lege capture |
| R10 | Analyzer uit, ingang aan; clamp voedt rail | H/D: backpower, ongeldige logica | Gecontroleerde railafvoer/ontvangerkeuze; aparte VALID | Later M09 met alle voedingsvolgorden; serieweerstand alleen is onvoldoende |
| R11 | Deler open/kort, bias/lekstroom of vervuilde PCB | H/D: meer belasting of foute drempel | Gesplitste weerstanden, laaglekbescherming, reinigen/inspecteren | Tolerantie- en foutberekening; later M08/M09; diagnostiek merkt niet elke analoge fout |
| R12 | Bestaande isolator mist U3 of heeft verkeerd type | H/D: onveilige of afwezige scheiding | Eerst bestukking, kanalenrichting en bronmatch vaststellen | M01/M03; geen vervanging aannemen uit 2022-BOM |
| R13 | USB/debug/scherm/montageschroef omzeilt barrière | H: massastroom | Scherm- en testpuntenbeleid per domein; isolerende mechanica | Volledig gemonteerde systeeminspectie, ook met programmeerfixture |
| R14 | Converter rimpel/common-mode of secundair te hoog | H/D: storing/overspanning | Aparte voeding, filters, railbewaking; min/max incl. nullast | Worst-case berekening en later M09; componenttestspanning is geen systeembewijs |
| R15 | USB-bron te zwak, inschakelpiek te groot | D/H: resets of verhitting | CC-budget, stroombegrenzing, load gate en brownoutstatus | Later M09 met kabelverlies; geen onbewezen 500 mA/1,5 A-aanname |
| R16 | Bufferoverloop, jitter, kanaalverschuiving | D: foutieve correlatie | Gemeenschappelijke tijdas, verliesmarkering, begrensde captures | G1-07 en latere stress-/simulatietests |
| R17 | Digitale analyzer “signaal gezond” tonen zonder analoge meting | D: schijnzekerheid | Weergeven wat is waargenomen en binnen welk meetbereik | UI-/exportreview; scope blijft nodig voor niveaus en flanken |

## 3. Metingen aan de bestaande installatie — vóór schema

M01–M07 vormen de installatieverificatie voor G1. Uitvoering door de
eigenaar of een bevoegde uitvoerder met passende instrumenten. Leg ook
vast wanneer iets nog ontbreekt of niet meetbaar is; geen waarde raden.

### M01 — identificatie en actuele bestukking

- Noteer artikelnummer/label van de Gleisbox, voedingsadapter en alle boosters.
- Leg SRSEII-silkscreen, printdetails, PIC-markering, jumpers en modificaties vast.
- Leg beide zijden van isolator 2020/08 vast, inclusief werkelijk geplaatst U3,
  CD4504BM-referentie, RJ45's, regulator, bypass en filters.
- Vergelijk relevante koperpaden met de bevroren oorspronkelijke bron.
  Een andere PIC kan een vervanging zijn; dat stelt de printrevisie niet vast.
- Noteer firmware 1.27 en UID als U totdat er een bestaand log of andere
  verificatie is toegevoegd. Geen nieuwe discovery-/programmeeractie nodig.

**Acceptatie:** bij elk interfacekritisch onderdeel een eenduidige match of
een gedocumenteerd verschil waarvan de elektrische gevolgen zijn uitgezocht.
Isolator niet compleet: status open en geen ingeschakelde proef uitvoeren.

### M02 — connector- en kabelkaart, zonder voeding

Identificeer contact 1 aan de hand van de concrete connectortekening.
Meet ieder contact naar de juiste IC-/voedingsnetten en naar het andere
kabeleinde. Noteer ongebruikte pennen, scherm, adapters en eventuele
kruisverbindingen. Verwacht niet dat een generieke RJ45-splitter alle
contacten afzonderlijk correct doorverbindt.

**Acceptatie:** complete contact-naar-contactkaart met kijkrichtingen,
geen onverklaarde verbindingen. Definitieve analyzeradapter nog niet ontwerpen
zolang deze kaart ontbreekt.

### M03 — referenties en bestaande isolatie, zonder voeding

Onderzoek apart: centrale-S88-GND, module-S88-GND, CAN-referentie,
baanretour, externe isolatorvoeding, USB/debug en kabelschermen.
Controleer op onbedoelde laagohmige paden, ook via voedingen en kabels.
Een weerstandsmeter die “open” aangeeft bewijst geen gecertificeerde isolatie.
Geen megger/hipottest op de onbekende gemonteerde elektronica.

**Acceptatie:** domeinkaart en voedingspad kloppen met de geverifieerde
scheiding; geen onbedoelde overbrugging. De vereiste werkspanning en
transiëntmarge moeten nog uit installatiegegevens worden begrensd.

### M04 — bestaande CAN-topologie en afsluiting, zonder voeding

Leg alle nodes, verdelers, kabels en vermoedelijke eindpunten vast.
Meet CANH–CANL op het geïdentificeerde meetpunt met alle voedingen uit.
Registreer bestaande jumperstanden. Waar nodig, met de installatie uit,
deelverbindingen losnemen om de plaats van terminatoren te bepalen en
daarna de oorspronkelijke toestand herstellen.

**Acceptatie:** verklaarde meetwaarde en bekende terminatielocaties.
120 Ω of circa 60 Ω is een hypothese-indicator, geen opdracht om een
weerstand toe te voegen. Geen live weerstandsmeting en geen active scan.

### M05 — voedingsniveaus en potentiaalverschillen

Pas na M01–M04, met correcte referenties en geïsoleerd meetgereedschap:
meet de gebruikte voedingspinnen en domeinverschillen. Bij S88 apart
centralezijde en modulezijde vastleggen. Meet stationair en tijdens
opstart/stop onder normale bestaande belasting. Jumperopschrift alleen
is geen spanningsmeting.

**Acceptatie:** min/max, polariteit, transiënten en meetreferentie bekend.
De analyzer moet beide beoogde 5 V- en 12 V-gevallen op de bank kunnen
kwalificeren; de echte installatie hoeft daarvoor niet blind te worden omgezet.

### M06 — CAN-golfvorm en passieve protocolwaarneming

Meet CANH/CANL zowel differentieel als t.o.v. de correcte CAN-referentie.
Leg recessief/dominant, common-mode, stijg-/daaltijd, ringing en bitrate vast.
Gebruik een aantoonbaar listen-only meetinstrument of geïsoleerde scope;
een standaard CAN-interface kan automatisch ACK zenden.

Observeer bestaand MS2/SRSEII-verkeer. Vergelijk ontvangen ID/DLC/payload
met Märklins protocolreferentie zonder vragen uit te sturen.

**Acceptatie:** normale busenvelop en relevante fysieke ontvangstmarges
bekend; geschikte aftaklocatie geïdentificeerd. Ruwe captures bewaren.

### M07 — S88-golfvorm en timing aan modulezijde

Alleen aan een aantoonbaar werkende modulezijde, met bestaande modules
en eigen besturing als bron. Leg per kanaal vast: LOW/HIGH-min/max,
overshoot/undershoot, stijg-/daaltijd, klokfrequentie en minimale pulsduur.
Leg LOAD, RESET, DATA en CLOCK gelijktijdig vast over meerdere scans en
een bekende verandering van terugmeldstatus. Noteer modulevolgorde en
instellingen van de centrale.

**Acceptatie:** sampleflank, eerste bit, scanlengte en timingrelaties zijn
herleidbaar; eventuele lagere DATA-HIGH op een 12 V-bus is zichtbaar.
Geen externe klok/load/reset injecteren. Als er geen aangesloten bus is,
is de uitkomst “niet aanwezig / nog niet meetbaar”, niet “0 V-bus”.

## 4. Latere fysieke prototypekwalificatie

Onderstaande stappen zijn gepland voor de hardwaretestfase; ze worden
niet uitgevoerd of als gereed aangemerkt in deze documentatiefase.

| ID | Proef op zelfstandige testopstelling | Acceptatie |
| --- | --- | --- |
| M08 | S88-impedantie, lekstroom, capaciteit en transferfunctie; langzaam én snel aangeboden flanken, per kanaal en met alle kanalen samen | E04–E06 en timingdoelen; meetinstrumentbelasting expliciet verrekenen |
| M09 | Alle voedingscombinaties; boot/reset/brownout; nullast/rimpel; geïsoleerde railfouten; USB-stroomadvertentie en inrush | Geen ongecontroleerde terugvoeding; VALID meldt verlies; geen grensoverschrijding; stroom-/temperatuurmarges gedocumenteerd |
| M10 | Busbelasting en niet-beïnvloeding met analyzer los, gevoed en ongevoed | E03/E12; geen nieuwe CAN/S88-fouten; voldoende analoge en timingmarges in alle toestanden |
| M11 | CAN-ontvangst bij bewuste verkeerde MCU-modus en zendpoging op testbus | Geen door analyzer veroorzaakte dominante bits, ACK, error- of dataframes; beide hardwarebeperkingen afzonderlijk controleren |
| M12 | S88-inputs bij verkeerd ingestelde GPIO-uitgang en lokale captureklok actief | Geen actieve output op de S88-bus; geen galvanisch retourpad via debug of logica |
| M13 | Foutinjectie: geopende/kortgesloten deler, protectie, voedingsrail; kanaal-/ingangsverwisseling binnen vastgelegde proefenvelop | Foutstromen en gevolgen overeen met analyse; proeven uitsluitend stroombegrensd op bank, nooit op bestaande baan |

Voor M11 moet de losse CAN-testbus normale zenders en een andere actieve
ontvanger voor ACK hebben. De analyzer levert die ACK juist niet. Een
enkelvoudige zender met alleen deze analyzer kan daardoor herhalen of
foutmeldingen geven zonder dat de analyzer zendt.

Voor E12 worden dezelfde bedrijfspatronen en meetpunten vóór en na
aansluiting gebruikt. Neem de gekozen adapter, maximale probelengte,
schermen en alle externe verbindingen mee. Eerst bankkwalificatie;
pas daarna een gecontroleerde vergelijking op de echte installatie.

## 5. Meetlogtemplate

Een nieuw resultaat krijgt een eigen bestand en verwijzing vanuit
[document 08](08-overdracht-en-validatie.md). Het template zelf is geen resultaat.

```text
Meet-ID / datum / uitvoerder:
Installatie en relevante printrevisies:
Bronontwerp + hash / gedocumenteerde afwijkingen:
Firmware/configuratie en instellingen:
Schakelsituatie en alle aangesloten voedingen/kabels:
Connector-ID, contactnummer, kijkrichting en referentie:
Instrument, kalibratie-/nauwkeurigheidsinformatie en probe:
Meetmethode, bandbreedte, samplefrequentie en instrumentbelasting:
Voedingsstatus per domein:
Gemeten minimum / maximum / eenheid / onzekerheid:
Originele capture/foto en bestandshash:
Verwachting en acceptatiegrens + herkomst (B of O):
Resultaat: geslaagd / afgekeurd / onbeslist / niet meetbaar
Afwijkingen, vervolgactie en eventueel besluit-ID:
```

Het is onvoldoende om alleen “CAN werkt”, “12 V”, “isolatie OK” of
“ERC groen” te noteren. Bewaar het meetpunt, de meetcondities en de
onderliggende gegevens zodat een ander de conclusie kan beoordelen.
