# Bronnenregister

Geraadpleegd op **2026-11-01**. Alleen primaire ontwerpers-, fabrikanten-
en standaarddocumentatie wordt als technisch bewijs gebruikt. Een bron
bevestigt uitsluitend de beschreven revisie. De eigen installatie vraagt
aanvullend inspectie en meting.

## 1. Installatie en protocollen

| ID | Primaire bron | Onderzochte versie / plaats | Gebruik en beperking |
| --- | --- | --- | --- |
| U01 | Hardwareopgave eigenaar in dit gesprek | Bericht ontvangen 2026-11-01; eerdere inspectie 2026-10-28 | U-status, vastgelegd in document 02; geen foto's, fysieke metingen of firmwarehash opnieuw gecontroleerd |
| U02 | Expliciete hardwarecorrectie eigenaar in dit gesprek | Ontvangen 2026-11-01: daadwerkelijk gebruikte SRSEII PCB-revisie **1.8.1a**, 1.5 niet de eigen hardware | U-bevestigde identificatie; sluit fysieke pinout-, kabel-, bestukkings- of spanningscontrole niet af |
| S01 | [GBert SRSE2_V1.5.pdf](https://github.com/GBert/misc/blob/81b6241cb9d47778daf0326e21210120ba24c2a5/srse2/SRSE2_V1.5.pdf), [oorspronkelijk 1.5-schema](https://github.com/GBert/misc/blob/81b6241cb9d47778daf0326e21210120ba24c2a5/srse2/SRSE2.sch) | Rev.1.5, 2020-06-12; oorspronkelijke commit `81b6241cb9d47778daf0326e21210120ba24c2a5` | **Uitsluitend historische vergelijking**; raster-PDF visueel onderzocht; native bron voor onder meer PIC-footprint |
| S02 | [GBert s88isolated_V1.0.pdf](https://github.com/GBert/railroad/blob/master/s88isolated/s88iso/s88isolated_V1.0.pdf) | Rev.1.0, 2020-08-30, blad 1/1 | Visueel volledig en vergroot onderzocht; kandidaat voor print 2020/08, nog geen spoor-/bestukkingsmatch |
| S03 | [GBert S88-isolator README](https://github.com/GBert/railroad/blob/master/s88isolated/s88iso/README.md) | Via GitHub-connector gelezen; blob `decbaf0dbeb82c97c499aa4404e531d390b0d577` | Andere referenties/onderdeelvariant dan S02; niet als bestukkingslijst voor 2020/08 gebruiken |
| S04 | [Ontwerper GBert: Gleisbox als Zentrale, openingsbericht](https://www.stummiforum.de/t160368f7-Gleisbox-als-Zentrale.html) | Eerste bericht van bertr2d2, zichtbare wijzigingsdatum 2026-01-15 | Waarschuwing massasluiting en 5 V-keuze SRSEII-zijde; bericht bevat ook latere revisies en is veranderlijk |
| S05 | [Hobbymakers handleiding S88-isolator](https://cdn.shopify.com/s/files/1/0936/7178/4776/files/Handleiding_S88-Isolator.pdf?v=1774191926) | ©2026, één pagina, via [downloadindex](https://hobbymakers.nl/pages/downloads) | Beschrijft huidige leverancieruitvoering; geen bewijs van print 2020/08 |
| S06 | [RailCommunity RCD-688](https://normen.railcommunity.de/RCD-688.pdf) | 2020-12-06, §2.2 p.2 en §3.1 p.3 | S88-N-contacten, voeding en timing; daadwerkelijke implementatie apart meten |
| S07 | [Märklin CAN-protocol 2.0](https://streaming.maerklin.de/public-media/cs2/cs2CAN-Protokoll-2_0.pdf) | 2012-02-07, 68 pagina's; p.5 en p.39–40 visueel gecontroleerd | CAN-formaat en terugmeldcommando's. Geen connectorpinout of bewijs van PIC-firmware 1.27 |
| S08 | [NXP AN10211](https://www.nxp.com/docs/en/application-note/AN10211.pdf) | Rev.02, 2006-11-10, §2 p.3 | Algemene HS-CAN-topologie en 120 Ω per uiteinde; geen beschrijving van de eigen Märklin-terminatie |
| S24 | [GBert native schema](https://github.com/GBert/misc/blob/4562014c9548e4880c6216b4975b004596e937e1/srse2/SRSE2.kicad_sch), [native PCB](https://github.com/GBert/misc/blob/4562014c9548e4880c6216b4975b004596e937e1/srse2/SRSE2.kicad_pcb) | Commit `4562014c9548e4880c6216b4975b004596e937e1`, 2023-11-22; titelrevisie **1.81**, datum 2022-12-08 | **Primaire elektrische bron binnen de 1.8.1a-bronset**; schema visueel en connector-/padnetten onderzocht; fysieke toepassing blijft M |
| S25 | [GBert SRSE2_V1.8.1.pdf](https://github.com/GBert/misc/blob/4562014c9548e4880c6216b4975b004596e937e1/srse2/SRSE2_V1.8.1.pdf) | Titelrevisie 1.81, 2022-12-08, blad 1/1 | Oorspronkelijke visuele familiereferentie; tekent nog L100 18 µH, geen afzonderlijke a-schema-PDF |
| S26 | [BOM 1.8.1a](https://github.com/GBert/misc/blob/4562014c9548e4880c6216b4975b004596e937e1/srse2/SRSE2_BOM_V1_8.1a.csv), [CPL 1.8.1a](https://github.com/GBert/misc/blob/4562014c9548e4880c6216b4975b004596e937e1/srse2/SRSE2_CPL_V1_8.1a.csv), [bestaande 1.8.1-archive](https://github.com/GBert/misc/blob/4562014c9548e4880c6216b4975b004596e937e1/srse2/SRSE2_V1.8.1_JLCPCB_Assembly_service.ZIP) | Dezelfde bevroren commit als S24 | Expliciete a-assemblagelijsten met P00/P01; archive heeft 1.8.1-lijsten en Gerbers, geen apart bewezen a-productiearchief |
| S27 | [Toevoeging a-lijsten](https://github.com/GBert/misc/commit/b2ad6cd00db5ced726d1335ca48bb7aa2e657b3f), [spoelbeschikbaarheid](https://github.com/GBert/misc/commit/2a8f674d560dec63c486839b868f3a21de276e4c), [latere bron-/BOM-update](https://github.com/GBert/misc/commit/4562014c9548e4880c6216b4975b004596e937e1) | 2023-09-18 en 2023-11-22 | Herkomst van a-lijsten en 18/15 µH-variant; geen bewijs van eigen bestukking |

Het actuele GBert-overzicht bevat bestanden van verschillende revisies.
De primaire bronset is geselecteerd op de eigenaar-bevestigde 1.8.1a
en bevroren in S24–S27, niet op de nieuwste datum. Overzichtspunten:
[SRSEII-directory](https://github.com/GBert/misc/tree/master/srse2) en
[isolator-directory](https://github.com/GBert/railroad/tree/master/s88isolated/s88iso).
Het onderzochte huidige SRSEII-`master` heeft native titelrevisie 1.82;
het legacybestand `.sch` draagt 1.7. Die bestanden zijn geen vervanging
van S24. Zie [document 09](09-srseii-revisievergelijking.md) voor de
afzonderlijke rollen van schema-/PCB-revisie, a-assemblage en fysieke print.

## 2. Componenten en ontwikkelomgeving

| ID | Primaire bron | Relevante versie / sectie |
| --- | --- | --- |
| S09 | [NXP TJA1051](https://www.nxp.com/docs/en/data-sheet/TJA1051.pdf) | Rev.9, 2017-11-28; §6 pinning, §7.1.2 silent, §7.2 voeding/failsafe, tabel 7 belasting en lekstroom |
| S10 | [TI TLV170x](https://www.ti.com/lit/ds/symlink/tlv1704.pdf) | SBOS589D, juni 2015; §6, §7.1 en §7.6–7.7; typische versus gegarandeerde waarden apart lezen |
| S11 | [Espressif ESP32-S3-WROOM-1/1U](https://documentation.espressif.com/esp32-s3-wroom-1_wroom-1u_datasheet_en.pdf) | v1.8, 2026-03-02; tabel 1-1 variant, tabel 3-1 p.11–12 visueel bekeken, §4 straps |
| S12 | [TI ISO772x](https://www.ti.com/lit/ds/symlink/iso7720.pdf) | Rev.G; richting, voedingsbereik en default HIGH/LOW afhankelijk van suffix controleren |
| S13 | [TI ISO774x](https://www.ti.com/lit/ds/symlink/iso7740.pdf), [TI ISO7710](https://www.ti.com/product/ISO7710) | ISO774x Rev.K, augustus 2026; ISO7710 product-/datasheetindex Rev.E; 4+1 voorwaartse kanalen als O-keuze |
| S14 | [WIZnet W5500 datasheet](https://docs.wiznet.io/assets/files/W5500_ds_v110e-226ffec190c588b69f88d629789585e1.pdf), [officiële referentieontwerpen](https://docs.wiznet.io/Product/Chip/Ethernet/W5500) | v1.1.0; p.8–10 pinfuncties; PHY-/magneticsreferentie nog volledig te dimensioneren in fase 2 |
| S15 | [RECOM RxxC05TExxS](https://recom-power.com/pdf/Econoline/RxxC05TExxS.pdf) | Rev.4/2024; ECO-1 t/m ECO-3, isolatiegraad en lagebelastinggedrag |
| S16 | [TI TPS6216x](https://www.ti.com/lit/ds/symlink/tps62162.pdf) | SLVSAM2E, mei 2017; regelaarfamilie, fixed-outputopties en belastingsbereik |
| S17 | [Espressif SoC-capabilities ESP32-S3](https://github.com/espressif/esp-idf/blob/v5.5.3/components/soc/esp32s3/include/soc/soc_caps.h), [TWAI-documentatie](https://docs.espressif.com/projects/esp-idf/en/v5.5.3/esp32s3/api-reference/peripherals/twai.html) | Versietag v5.5.3; RMT-RX/LCD_CAM-resources en listen-only; geen bewijs van gezamenlijke continue acquisition |
| S18 | [TI TUSB320LAI](https://www.ti.com/product/TUSB320LAI) | Fabrikantpagina/datasheetindex Rev.D; sinkrol en aangeboden stroom detecteren; definitief vermogenspad nog open |
| S19 | [Waveshare 1.3inch OLED (B)](https://www.waveshare.com/product/ai/displays/oled/1.3inch-oled-b.htm) | SKU10451, SH1106; productpagina noemt 128×64, 3,3/5 V en interfaceconfiguratie |
| S20 | [ESP-IDF Ethernet](https://docs.espressif.com/projects/esp-idf/en/v5.5.3/esp32s3/api-reference/network/esp_eth.html), [W5500-bronmap](https://github.com/espressif/esp-idf/tree/v5.5.3/components/esp_eth/src/spi/w5500) | Versietag v5.5.3; integratiemogelijkheid, geen nu gemaakte firmwarekeuze/build |
| S21 | [KiCad CLI-documentatie](https://docs.kicad.org/9.0/en/cli/cli.html) | 9.0.9-handleiding als referentie; de hier aanwezige CLI is 10.0.7 en zijn eigen ERC-/DRC-help is apart gecontroleerd |
| S22 | [Espressif ESP32-S3 camera low-level driver](https://github.com/espressif/esp32-camera/blob/v2.1.3/target/esp32s3/ll_cam.c), [camera HAL](https://github.com/espressif/esp32-camera/blob/v2.1.3/driver/cam_hal.c) | Versietag v2.1.3; pinrouting, start/stop, VSYNC en DMA-bytebegrenzing als onderbouwing voor vervolgonderzoek, niet als analyzerimplementatie |
| S23 | [ESP-IDF RMT](https://docs.espressif.com/projects/esp-idf/en/v5.5.3/esp32s3/api-reference/peripherals/rmt.html) | v5.5.3; symboolopbouw, RX-geheugen en transactionele ontvangst |
| S28 | [TI SN65HVD23x](https://www.ti.com/lit/ds/symlink/sn65hvd230.pdf) | SLOS346O, april 2018; 3,3 V-voeding, pinfuncties en Rs-modus. Gebruikt voor de bestaande SRSEII-component, niet als nieuwe analyzerselectie |

Deze links zijn opzoekpunten. Waar een latere versie wordt opgehaald,
revisie en relevante tabellen opnieuw vergelijken vóór gebruik. Een
“ACTIVE”-label of productpagina vervangt geen review van de exacte bestelcode.

## 3. Identiteit van lokaal onderzochte bestanden

Lokale onderzoekskopieën staan onder `tmp/research/` en zijn niet in Git
opgenomen. Ook renderafbeeldingen en de lokale PDF-leesomgeving worden
niet heruitgegeven. Onderstaande SHA-256 identificeert de werkelijk gelezen
bytes; `master`-links kunnen in de toekomst wijzigen.

| Bron / lokaal bestand | SHA-256 |
| --- | --- |
| S01 `SRSE2_V1.5.pdf` | `318e67097f7f87173f4c128c14c6b4606eeb239a6eb11efbff23d027c42bacfa` |
| S02 `s88isolated_V1.0.pdf` | `ec240e5eaaa8259fbd324c6e10694749add82ddbfdbdb18693fcf47865d0cdc4` |
| S05 `Handleiding_S88-Isolator.pdf` | `890673482ba33e825da26ce6441820d771eed2dc2c3bee4c82c406aa2eb11c5f` |
| S07 `cs2CAN-Protokoll-2_0.pdf` | `98b3d6cb884cb74cfd33a5e2e6a11430b431f7743a49db73d577274c8f46bc7a` |
| S11 `esp32-s3-wroom-1.pdf` | `27d71971da07c280c6068d08c74720d1a25b8f20cf8494dc1765bdd28d40d435` |
| S25 `SRSE2_V1.8.1.pdf` | `3a30fd8efd0a2c340ef2b1dc21940f100507899f5349724bf3d10572c586701f` |
| S24 `srse2-181a/SRSE2.kicad_sch` | `46d440e723ef04c6f1327388ddf3d06cc30d2e310eb9ca17fdddbc05514251c1` |
| S24 `srse2-181a/SRSE2.kicad_pcb` | `abd34d6fb5bfd9a12cc285f4f48fe86c04c6b07d049cfa88873aa63441c1dd4a` |
| S26 `srse2-181a/SRSE2_BOM_V1_8.1a.csv` | `64caa710917fb3bf1d8676c9d164a8c05dfc36ac86d30d301176287b8057b20f` |
| S26 `srse2-181a/SRSE2_CPL_V1_8.1a.csv` | `cae11373d6c784fc07f7e9d9433a252b55b73ebd66c2417c5c24b256087c6c41` |
| S26 `srse2-181a/SRSE2_V1.8.1_JLCPCB_Assembly_service.ZIP` | `e3ed58004eb2a0eae2ba074cb4f053ce96a6509f8e7c8232dfe72c3002924bde` |

GitHub rapporteerde voor S01 blob `809e1a114d084a3cc7648be493c0048d5956a667`
en voor S02 blob `f19e8cabad31dcdb6381b5e04b673e23ec04045c`.
Dit zijn **file-blob-ID's, geen commit-ID's**. Ze identificeren een bestand,
geen complete bronrevisie van de eigen print.

## 4. Bereikbaarheid en onopgeloste bronvragen

- `s88-n.eu` kon via de webtool niet betrouwbaar worden opgehaald.
  RCD-688 is rechtstreeks bij RailCommunity gelezen; de oorspronkelijke
  website is niet als gelezen bewijs opgevoerd.
- De Märklin-2.0-PDF en beide GBert-schema's waren via de webtool niet
  leesbaar. Ze zijn rechtstreeks van de primaire host gedownload en lokaal
  onderzocht. De GitHub-connector ondersteunt alleen tekst voor deze functie.
- De eigenaar bevestigt nu PCB 1.8.1a [U02]; S24–S27 vormen de primaire
  bronset. Een apart a-schema of a-Gerberarchief is niet aangetroffen.
  De fysieke match van contacten, kabels en bestukking blijft afzonderlijk open.
- Grote/binaire CAD- en archivebestanden zijn rechtstreeks van de primaire
  host op de bevroren commit gedownload waar de GitHub-connector ze niet
  kon verwerken. Alleen eigen documentatie wordt gecommit.
- Voor de Gleisbox zelf ontbreekt nog het exacte artikelnummer en een
  bewezen connector-/terminatiekaart. Er is geen Mini-DIN-bedrading vrijgegeven.
- PIC-firmware 1.27 is U-informatie. De daadwerkelijke bron-/buildrevisie en
  S88-timing zijn in deze fase niet onafhankelijk geverifieerd.
- S02 en S03 spreken elkaar tegen op punten die relevant zijn voor
  bestukking. Alleen een gecontroleerde vergelijking met de echte print
  kan bepalen welke daarvan van toepassing is.
