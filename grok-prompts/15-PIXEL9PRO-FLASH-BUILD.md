# PROMPT 15: Pixel 9 Pro (caiman) — Flash & Build Handleiding Stay4ROM/Stay4OS

Plak dit in Grok:

---

Je bent een senior Android ROM engineer die werkt voor Stay4S B.V. Schrijf de COMPLETE, copy-paste ready handleiding voor het flashen van een Google Pixel 9 Pro (codename CAIMAN, familie CAIMITO) met Stay4ROM gebaseerd op LineageOS 23.2 (Android 16), en daarna het bouwen van Stay4ROM met onze custom services. De handleiding is voor een solo developer met een Lenovo IdeaPad 5 (Debian Linux) als werk-machine en een Hetzner AX102 (64GB RAM, ruime schijfruimte) als build-server.

APPARAAT-INFO (geverifieerd):
- Pixel 9 Pro = codename caiman; familie/family-tree = caimito (Pixel 9 = tokay, 9 Pro XL = komodo)
- Chip: Google Tensor G4; kernel codename: caimito (kernel 6.6); security chip: Titan M2
- Boot: A/B slots, AVB 2.0 (Android Verified Boot), rollback protection
- LineageOS 23.2 heeft OFFICIELE support voor caiman (wiki.lineageos.org/devices/caiman)

CONTEXT:
- Stay4ROM = LineageOS 23.2 fork met 5 custom services: AetherCoreService (lokale AI via Ollama/StayLM, AetherBridge AIDL), GuardianService (scam/phishing detectie + LocationGuard GPS kill switch in 3 lagen: hardware/sysfs, OS daemon+AppOps, netwerk/SUPL via iptables), VaultService (encrypted opslag, Titan M2 StrongBox keys), GlyphService is VERVANGEN door Ambient Display notificaties (Pixel heeft geen Glyph-LEDs), MeshmaticService (LoRa mesh via MQTT)
- LocationGuard-code bestaat al (repo Stay4S-LocationGuard) — hergebruiken, niet herschrijven
- Build server: Hetzner AX102 met Debian/Ubuntu, SSH-toegang vanaf de IdeaPad
- Bestaande device-kennis: Nothing Phone 3a (asteroids) tree is ARCHIEF, geen hergebruik behalve services-code
- Doel: Kickstarter-demo eind oktober 2026, daarna OTA-updates via eigen server

SCHRIJF DE VOLGENDE LEVERANCIEREN (allemaal copy-paste ready):

1. VOORBEREIDING OP DE TELEFOON (dag ervoor — volgorde is kritiek)
- Wat te backuppen, OEM unlocking aanzetten (Instellingen > Ontwikkelaarsopties) en WAAROM dit dagen vooraf moet (Google account verification / anti-theft), Google-account eraf halen pas NA OEM unlock aanzetten
- Opsomming van wat er breekt na unlock: Widevine L3 (geen HD streaming), Play Integrity faalt (bankapps kunnen weigeren), garantie-implicaties — eerlijk en compleet

2. VOORBEREIDING OP DE IDEAPAD 5 (Debian)
- Exacte apt-commando's: adb, fastboot, platform-tools, python3, git, ssh
- USB-driver / udev rules voor Pixel op Debian
- Verify: adb devices moet het toestel tonen

3. STAP 1: STOCK LINEAGEOS 23.2 FLASHEN (eerst romig wennen, geen custom code)
- Download-links procedure: offcieële LineageOS recovery + latest caiman build (wiki-pagina)
- Bootloader unlock: exacte fastboot commando's (fastboot flashing unlock)
- LineageOS recovery flashen via fastboot, dan adb sideload van de ROM-zip
- Eerste boot: wat is normaal (5-10 min), setup ZONDER Google-account
- Herstelprocedure naar stock: Google factory image (caiman) — downloadlocatie developers.google.com/android/images, exacte flash-all stappen

4. STAP 2: BUILD-OMGEVING OP DE AX102
- Alle benodigde packages: OpenJDK 17, repo tool, git config, build-essential, ccache setup
- Schijfruimte-check (~400GB), swap-advies bij 64GB RAM
- repo init op lineage-23.2 branch + repo sync (met -j naar cores), tijdsindicatie
- breakfast caiman + proprietary blobs extracteren (adb vanaf de telefoon of extract-files vanaf stock image)

5. STAP 3: EERST EIGEN BUILD (ONGEWIJZIGD) — pipeline-test
- mka bacon met ccache, tijdsindicatie eerste build (6-10 uur)
- Eigen release keys genereren (subject-config, openssl commando's) — en hoe je ze VEILIG bewaart (offline kopie + Vault)
- Build ondertekenen met eigen keys, verschil dev-keys vs release-keys uitleggen
- De gemaakte zip + images flashen — exacte procedure, A/B slot-gedrag uitleggen

6. STAP 4: STAY4ROM AANPASSEN — DEVICE TREE STRUCTUUR
- Eigen fork-structuur: waar komen de 5 services (PRODUCT_PACKAGES in device tree), overlay voor Stay4OS branding (boot animatie hexagon bestaat al — prompt 2 output), ambient-display vervanging voor Glyph
- Per service: welke map, welke .mk/.bp regels, welke SELinux .te policies nodig (aether_core.te, guardian.te, vault.te, meshmatic.te) — geef voorbeeldpolicies die enforcing passeren
- Stay4OS theming: RRO overlay structuur voor kleuren/fonts/boot logo

7. STAP 5: FLASHEN VAN EIGEN BUILD + TESTPROTOCOL
- Flash-procedure van eigen zip (zelfde als stap 1.3 maar met eigen bestanden)
- Testprotocol in volgorde (18 punten): boot splash+animatie, setup zonder Google, WiFi, mobiel+bellen, BT, camera alle lenses, vingerafdruk, GPS fix > DAN LocationGuard aan > GPS dood (kill switch verificatie), Guardian phishing-URL test, Vault bestand+decrypt, Meshmatic LoRa (indien hardware), AetherCore lokale AI query offline, 24u batterij, SELinux status (getenforce = Enforcing), OTA van eigen server, en eerlijk documenteren: Play Integrity (FAIL verwacht), bankapps, Netflix HD (L3 verwacht)
- Wat te doen bij bootloop: recovery, fastboot, slot terugzetten (A/B voordeel)

8. STAP 6: OTA-SERVER (soevereiniteit — updates zonder Google)
- Eigen OTA-server op Hetzner: update_engine config, signing van OTA zips, simpele nginx+json setup
- Hoe de telefoon bijwerkt vanaf eigen server

9. DOCUMENTATIE-STANDAARD (Mitchell wil hier beter in worden — dit is verplicht onderdeel)
- Elke repo krijgt /docs met: README.md, BUILD.md (deze handleiding), DECISIONS.md (waarom elke keuze), CHANGELOG.md
- Elke build-sessie eindigt met 10 min documentatie: wat werkte, wat brak, hoe gefixt
- Formaat van BUILD.md zodat een nieuwe teamgenoot (of toekomstige CTO) van nul tot flashen kan met alleen de docs

10. TROUBLESHOOTING-HOOFDSTUK
- Veelvoorkende fouten: device not found (udev/USB), bootloop, SELinux denials (audit2allow juist gebruiken — met waarschuwing dat het geen silver bullet is), vendor mismatch, AVB errors (wat betekent rode staat), rollback protection
- Wanneer hulp zoeken: LineageOS caiman XDA thread, LineageOS wiki, GrapheneOS build guide als referentie

Geef alles als copy-paste ready commando's, code blocks en bestandsstructuren. Echte URLs, echte packagenamen, echte fastboot-commando's. Nederlands in uitleg, Engels in code/commands. Volledig executeerbaar, geen placeholders. Waar iets afhankelijk is van de actuele LineageOS-caiman build, zeg je expliciet waar dat te controleren valt.

---
