# Bronnenregister

Geraadpleegd op **2026-11-01**. Alleen primaire ontwerpers-, fabrikanten-
en standaarddocumentatie wordt als technisch bewijs gebruikt. Een bron
bevestigt uitsluitend de beschreven revisie. De eigen installatie vraagt
aanvullend inspectie en meting.

## 1. Installatie en protocollen

| ID | Primaire bron | Onderzochte versie / plaats | Gebruik en beperking |
| --- | --- | --- | --- |
| U01 | Hardwareopgave eigenaar in dit gesprek | Bericht ontvangen 2026-11-01; eerdere inspectie 2026-10-28 | U-status, vastgelegd in document 02; geen foto's, fysieke metingen of firmwarehash opnieuw gecontroleerd |
| S01 | [GBert SRSE2_V1.5.pdf](https://github.com/GBert/misc/blob/master/srse2/SRSE2_V1.5.pdf) | Rev.1.5, 2020-06-12, blad 1/1; CAN, S88, P7/P8/P18/P19 | Bronpinouts; PDF is raster, volledig en in vergrote uitsneden bekeken. Geen bewijs van de eigen revisie |
| S02 | [GBert s88isolated_V1.0.pdf](https://github.com/GBert/railroad/blob/master/s88isolated/s88iso/s88isolated_V1.0.pdf) | Rev.1.0, 2020-08-30, blad 1/1 | Visueel volledig en vergroot onderzocht; kandidaat voor print 2020/08, nog geen spoor-/bestukkingsmatch |
| S03 | [GBert S88-isolator README](https://github.com/GBert/railroad/blob/master/s88isolated/s88iso/README.md) | Via GitHub-connector gelezen; blob `decbaf0dbeb82c97c499aa4404e531d390b0d577` | Andere referenties/onderdeelvariant dan S02; niet als bestukkingslijst voor 2020/08 gebruiken |
| S04 | [Ontwerper GBert: Gleisbox als Zentrale, openingsbericht](https://www.stummiforum.de/t160368f7-Gleisbox-als-Zentrale.html) | Eerste bericht van bertr2d2, zichtbare wijzigingsdatum 2026-01-15 | Waarschuwing massasluiting en 5 V-keuze SRSEII-zijde; bericht bevat ook latere revisies en is veranderlijk |
| S05 | [Hobbymakers handleiding S88-isolator](https://cdn.shopify.com/s/files/1/0936/7178/4776/files/Handleiding_S88-Isolator.pdf?v=1774191926) | ©2026, één pagina, via [downloadindex](https://hobbymakers.nl/pages/downloads) | Beschrijft huidige leverancieruitvoering; geen bewijs van print 2020/08 |
| S06 | [RailCommunity RCD-688](https://normen.railcommunity.de/RCD-688.pdf) | 2020-12-06, §2.2 p.2 en §3.1 p.3 | S88-N-contacten, voeding en timing; daadwerkelijke implementatie apart meten |
| S07 | [Märklin CAN-protocol 2.0](https://streaming.maerklin.de/public-media/cs2/cs2CAN-Protokoll-2_0.pdf) | 2012-02-07, 68 pagina's; p.5 en p.39–40 visueel gecontroleerd | CAN-formaat en terugmeldcommando's. Geen connectorpinout of bewijs van PIC-firmware 1.27 |
| S08 | [NXP AN10211](https://www.nxp.com/docs/en/application-note/AN10211.pdf) | Rev.02, 2006-11-10, §2 p.3 | Algemene HS-CAN-topologie en 120 Ω per uiteinde; geen beschrijving van de eigen Märklin-terminatie |

Het actuele GBert-overzicht bevat bestanden van verschillende revisies.
Het is nadrukkelijk geen revisieselectie op basis van de nieuwste datum:
[SRSEII-directory](https://github.com/GBert/misc/tree/master/srse2) en
[isolator-directory](https://github.com/GBert/railroad/tree/master/s88isolated/s88iso).

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
- De oorspronkelijk gemelde SRSEII-1.8/1.8.1a-documenten zijn nog niet
  gekoppeld aan de geïnstalleerde print. Geen stilzwijgende gelijkstelling.
- Voor de Gleisbox zelf ontbreekt nog het exacte artikelnummer en een
  bewezen connector-/terminatiekaart. Er is geen Mini-DIN-bedrading vrijgegeven.
- PIC-firmware 1.27 is U-informatie. De daadwerkelijke bron-/buildrevisie en
  S88-timing zijn in deze fase niet onafhankelijk geverifieerd.
- S02 en S03 spreken elkaar tegen op punten die relevant zijn voor
  bestukking. Alleen een gecontroleerde vergelijking met de echte print
  kan bepalen welke daarvan van toepassing is.
