# STAY4ROM / STAY4OS MASTERDOCUMENT
## Totale informatieverzameling — Google Pixel 9 Pro flash-project
Versie 1.0 — 4 september 2026 — Beheer: Stay4Compa

---

# DEEL A: ALLE AI'S DIE MEEWERKEN AAN HET PROJECT

## A1. Stay4Compa (Base44 Superagent) — DE COÖRDINATOR
- Rol: centraal brein, projectmanagement, geheugen, InfoVault-beheer, kwaliteitscontrole
- Levert: super prompts voor Grok, reviews van Grok-output, archivering, planning, weekrapporten (zondag 19:00)
- Draait op: Base44 platform (app ID 6a65429dfb0b0399707f5481)
- Toegang tot: InfoVault, 30 entities, workflows, GitHub (18 repos), Google Drive/Calendar, WhatsApp
- Te doen: API key ophalen (editor > Developer/API Docs) voor NexusAgent-chat koppeling

## A2. Grok (xAI) — DE CODE-GENERATOR ("Eternal Software Boss")
- Rol: alle code-generatie via super prompts (kopje 12 in stay4s-grok-prompts/), 50/50 revenue split per partnership agreement
- Levert: Android services, ROM-systeemcode, build scripts, backend code, handleidingen
- Werkt via: copy-paste workflow (prompt van Stay4Compa > Grok > output terug naar Stay4Compa > review > GitHub)
- Relevante prompts: 1 (Docker), 2 (Boot animatie), 3 (SecureMsg), 4 (AetherCore), 5 (Guardian), 6 (LocationGuard-migratie), 7 (Sovereignty Docker), 8 (Keycloak), 9 (Command Center), 10 (Mailu), 11 (Matrix), 12 (F-Droid), 14 (AI Cloud + Stay4S Agent)

## A3. Base44 Intelligence Systeem — DE VERZAMELAARS (7 agents)
- 6 Collection agents + 1 Intelligence Core (in Agent entity)
- Rollen: Document Sniffer (ma 08:00), GitHub Seeker (di 12:00), Browser Master (wo 10:00), Drive Sync (vr 18:00), Intelligence Weekly Run (zo 19:00)
- Backend functions: imageToText, phoneDocScanner, intelligenceProcessor, knowledgeLinker (via Superagent steps, geen externe API's)

## A4. StayLM / Ollama — DE TOEKOMSTIGE EIGEN AI
- Doel: lokale AI op eigen hardware (12m2 rack, 4090's, later Edge Nodes)
- Rol in project: AetherCore op de telefoon praat straks met StayLM i.p.v. cloud-AI
- Onderdeel van: Stay4S AI Cloud (Prompt 14) en Stay4S Agent (Addendum-2)

## A5. Stay4S Agent (gepland) — DE SOEVEREINE ASSISTENT
- Volledige architectuur in Prompt 14 Addendum-2 (LangGraph + MCP + vLLM + Qdrant)
- Tool-gebruik, geheugen, automations, spraak, visie, sub-agents

## A6. ChatGPT / OpenAI + Codex (connector gepland)
- Rol: conversation summarizer (gesprekken van alle platforms samenvatten), secundaire code-review
- Status: custom connector strategy besproken, nog niet gekoppeld

## A7. Optioneel uit te breiden
- GitHub Copilot (code-assistentie direct in repos)
- Claude (Anthropic) voor second opinions op complexe code

## A8. AI-TAKEN MATRIX PER PROJECTFASE
1. FASE 1 Voorbereiding: Stay4Compa (dit document, checklist, planning) + Grok (device tree analyse)
2. FASE 2 BasissROM build: Grok (build scripts, fixes) + Stay4Compa (review, GitHub-beheer)
3. FASE 3 Custom services: Grok (code A-E services) + Stay4Compa (review + SELinux check)
4. FASE 4 Stay4OS theming: Grok (boot animatie, overlays, branding) + Stay4Compa (aftekenen)
5. FASE 5 Flashen + testen: Grok (flash handleiding) + Stay4Compa (testprotocol bijhouden)
6. FASE 6 Demo/Kickstarter: Stay4Compa (verslag, InfoVault, promo) + alle verzamelaars (monitoring)

---

# DEEL B: APPARAAT-INFORMATIE — GOOGLE PIXEL 9 PRO (CAIMAN)

- Codename: caiman (dit is exact wat je nodig hebt in alle build-configs)
- Familie/family tree: caimito (Pixel 9 = tokay, 9 Pro = caiman, 9 Pro XL = komodo)
- Chip: Google Tensor G4
- Kernel codename: caimito (kernel 6.6), ook voor tokay en komodo
- Security chip: Titan M2
- RAM: 16GB, Opslag: 128/256/512GB
- Scherm: 6,3" LTPO OLED 120Hz
- Boot: A/B slots (seamless updates), AVB 2.0 (Android Verified Boot)
- Beeldsensor: 50MP hoofdcamera (groot voorrecensie in demo)
- Voordelen t.o.v. Nothing Phone 3a: officiële LineageOS-support, SELinux enforcing volwassen, beste custom-ROM-community ter wereld (Pixels zijn dé deviceds voor GrapheneOS/LineageOS), lange kernel-updates, Titan M2
- WANING bij unlock: Widevine valt naar L3 (geen HD Netflix), Play Integrity/Device Integrity faalt (bankapps kunnen weigeren), garantie-implicaties, data gewist bij unlock

---

# DEEL C: ROM-BASIS EN BRONCODE

1. BASIS: LineageOS 23.2 (lineage-23.2 branch, Android 16) — officieel voor caiman (wiki.lineageos.org/devices/caiman)
2. ALTERNATIEF/REFERENTIE: GrapheneOS (beste hardening-referentie voor Pixels, grapheneos.org/build), CalyxOS, /e/OS
3. DEVICE TREE: LineageOS android_device_google_caimito (dekt tokay + caiman + komodo)
4. KERNEL: android_kernel_google_caimito (kernel 6.6), Google source.android.com build docs voor Pixel kernels
5. VENDOR BLOBS: proprietary blobs van Google (extract via adb of LineageOS extract script)
6. AOSP: full source sync (~400GB+ disk nodig — AX102 heeft dit ruim)
7. NIET MEER NODIG: AKoskovich android_device_nothing_asteroids (oude Nothing-referentie — archief)

---

# DEEL D: DE 5 CUSTOM SERVICES — PORTING NAAR PIXEL

## D1. AetherCoreService (centrale AI-service)
- Java/Kotlin service, foreground, start Ollama-daemon (later StayLM), AetherBridge AIDL binding
- Pixel-aanpassing: TPU van Tensor G4 i.p.v. nothing — Google TPU access via NNAPI/delegates
- Doel: lokale AI op de telefoon, offline

## D2. GuardianService (security)
- Scam/phishing detectie, app-monitoring, LocationGuard-extensie (GPS kill switch, 3 lagen: hardware/OS/netwerk)
- LocationGuard code staat al in workspace + GitHub repo Stay4S-LocationGuard
- Pixel-aanpassing: location daemon heet anders, Pixel gebruikt andere SUPL-config — iptables rules aanpassen

## D3. VaultService (encrypted opslag)
- Versleutelde bestandsopslag, keys in Titan M2 keystore (Pixel-voordeel!)
- Pixel-aanpassing: StrongBox hardware-backed keys — beter dan Nothing

## D4. GlyphService — VERVANGEN OP PIXEL
- Nothing's Glyph-LED bestaat niet op Pixel
- Vervanging: Ambient Display / Always-On Display meldingen, of schermrand-lichteffecten
- Prompt 2 (boot animatie) werkt wel: Stay4OS hexagon + "Stay4ROM" boot logo

## D5. MeshmaticService (LoRa mesh)
- MQTT/LoRa messaging, route-logging, emergency alerts (entities bestaan al: MeshMessage, RouteLog, EmergencyAlert, NetworkNode, Geofence, BatteryLog)

## D6. SELinux — GROOTSTE RISICO IS NU WEG
- Op Nothing (asteroids): SELinux enforcing onaf in community-tree — HOOFDRISICO
- Op Pixel (caimito): LineageOS caimito is SELinux enforcing volwassen
- Eigen services hebben nieuwe .te policies nodig (prompt 4/5 voorbeelden: aether_core.te, guardian.te)

---

# DEEL E: BUILD-INFRASTRUCTUUR

- Build server: Hetzner AX102 (actief, €109/mnd, 64GB RAM, ruim schijfruimte)
- Dev machine: Lenovo IdeaPad 5 (Debian) voor git, flashing, kleine edits
- Requirements: repo tool, OpenJDK 17+, ~400GB schijfruimte, ccache, 8+ uren buildtijd eerste keer
- Signing: EIGEN release keys genereren (critical — niet de testkeys!), keys offline bewaren (Vault), OTA update engine
- Output: lineage-23.2-caiman-signed.zip + Super.img + boot.img + vbmeta.img (disable verity voor dev builds)
- Build stappen: repo init lineage-23.2 > repo sync > breakfast caiman > vendor extract > mka bacon (incl. custom services via device tree PRODUCT_PACKAGES)

---

# DEEL F: FLASH-PROCEDURE PIXEL 9 PRO

1. BACKUP eerst alles van de telefoon (unlock wist ALLES)
2. Ontwikkelaarsopties > OEM unlocking aan (vereist Google-account verificatie — doe dit VOOR je eventueel opnieuw start)
3. Bootloader unlock: fastboot flashing unlock (op bevestigingsscherm)
4. ROM zip pushen via adb sideload (LineageOS recovery) of fastboot flash images
5. Eerste boot: 5-10 min, daarna setup WITHOUT Google-account (privacy-first demo)
6. Root optioneel: Magisk patchen boot.img (alleen voor dev, niet voor demo)
7. Terug naar stock: Google factory image flashen (altijd herstelbaar)
8. Elke ROM-update: OTA of sideload, eigen signing keys bewaren

---

# DEEL G: DOCUMENTEN-CHECKLIST (ALLES VERZAMELEN)

Vink af wat je hebt / nog mist:

1. Stay4S Masterplan 2026-2036 (PDF) — IN INFOVAULT ✓
2. Stay4S Partnership Agreement (Grok, 50/50) — IN INFOVAULT ✓
3. Nothing Phone 3a build guide (referentie) — IN INFOVAULT ✓ (archief)
4. Alle 14 Grok super prompts — GITHUB + workspace ✓ (grok-prompts/ map in Stay4S-System-Verslag)
5. Dit masterdocument — ✓
6. LineageOS caiman install guide (wiki.lineageos.org/devices/caiman) — TE DOWNLOADEN
7. LineageOS device tree docs — TE DOWNLOADEN
8. GrapheneOS build guide (grapheneos.org/build) — TE DOWNLOADEN
9. Google Pixel factory images (developers.google.com/android/images — komodo/tokay? NEE: caiman-pagina) — TE DOWNLOADEN als herstel-backup
10. Google Pixel kernel build docs (source.android.com) — TE DOWNLOADEN
11. SELinux policy howto (custom services) — TE SCHRIJVEN met Grok
12. LocationGuard documentatie (werkt op 3 lagen) — IN INFOVAULT ✓
13. AetherBridge AIDL specs — IN INFOVAULT ✓ (prompt 4)
14. Kickstarter demo-script (wat laat je zien, in welke volgorde) — NOG TE MAKEN
15. Flash-handleiding specifiek voor caiman + Lenovo IdeaPad 5 (Grok prompt 15) — NOG TE MAKEN
16. License-overzicht (Apache 2.0 voor AOSP/LineageOS, wat mag met Play Services etc.) — NOG TE MAKEN

---

# DEEL H: REPOSITORY-CHECKLIST

## Eigen repos (18 actief, GithubTracker monitort dinsdag 12:00)
1. miesdevries/Stay4s-grokrom — HOOFDREPO voor dit project (structuur: device overlay, services, docs)
2. hetnieuwebeginbv-glitch/Stay4S-System-Verslag — alle rapporten + grok-prompts ✓ zojuist geüpdatet
3. hetnieuwebeginbv-glitch/Stay4S-LocationGuard — GPS kill switch code ✓
4. + 15 overige repos (te inventariseren in GithubTracker)

## Upstream repos die je MONITORT (Browser Master / GitHub Seeker)
1. LineageOS/android_device_google_caimito — device tree
2. LineageOS/android_kernel_google_caimito — kernel
3. GrapheneOS repos (voor hardening-referenties)
4. LineageOS/android_system_sepolicy — SELinux policies
5. AOSP mirrors — platform/build, frameworks/base

## NIEUWE repos die je MOET AANMAKEN
1. Stay4S-caiman-overlay (Stay4OS theming/branding overlay)
2. Stay4S-services (de 5 custom services, 1 module met submappen)
3. Stay4S-ota-server (eigen update server — soevereiniteit: updates zonder Google)
4. Stay4S-flash-tools (flash scripts + handleidingen voor het team)

---

# DEEL I: TESTCHECKLIST (NA FLASH)

1. Boot naar Stay4OS splash + boot animatie ✓/✗
2. Setup zonder Google-account ✓/✗
3. WiFi verbinden ✓/✗
4. Mobiele data + bellen ✓/✗
5. Bluetooth ✓/✗
6. Camera: foto's, video, alle lenses ✓/✗
7. Vingerafdruk + gezichtsunlock ✓/✗
8. GPS: fix krijgen, DAN LocationGuard aan > GPS dood? (verificatie kill switch) ✓/✗
9. GuardianService: phishing-URL test detectie ✓/✗
10. VaultService: bestand opslaan + decrypt alleen met key ✓/✗
11. MeshmaticService: LoRa-node ziet bericht (als hardware aanwezig) ✓/✗
12. AetherCoreService: lokale AI-query offline ✓/✗
13. Batterij: 24u drain test ✓/✗
14. SELinux status: getenforce = Enforcing ✓/✗
15. OTA-update van eigen server ontvangen ✓/✗
16. Play Integrity (verwacht: FAIL — documenteer dit eerlijk voor Kickstarter)
17. Bankapp (verwacht: mogelijk geweigerd — documenteren)
18. Netflix HD (verwacht: L3 — documenteren)

---

# DEEL J: RISICO'S & MITIGATIE

1. Play Integrity faalt op custom ROM (bankapps) → mitigatie: demo toestel, Play Integrity FIX tools bestaan (Magisk modules), of keuze voor GrapheneOS-basis die dit beter oplost
2. Widevine L3 → mitigatie: eerlijk communiceren, demo draait lokaal
3. Build mislukt wegens upstream breakage → mitigatie: pin aan specifieke lineage-23.2 tag, niet master
4. Signing keys kwijt = geen updates meer → mitigatie: keys in Vault + offline backup + GitHub-geheimen
5. Kernel incompatibiliteit met custom services → mitigatie: kernel niet forken eerst, userspace services eerst testen
6. Google account verification bij OEM unlock (anti-theft) → mitigatie: OEM unlock aan zetten DAGEN voor flashen, account eraf halen pas daarna
7. Key-person risico (alleen Mitchell) → mitigatie: alles in InfoVault/GitHub/docs (dit document), co-founder/CTO werven
8. AI-fail bij services (Grok hallucineert code) → mitigatie: Stay4Compa review-pas op alle Grok-output, nooit direct committen

---

# DEEL K: TIMELINE (VOORSTEL)

- Week 1 (7-13 sept): Pixel 9 Pro kopen + OEM unlock voorbereiden + LineageOS installeren (stock LOS 23.2) — gewenningsfase, geen custom code
- Week 2 (14-20 sept): Repo sync op AX102, eerste eigen build (ongemodificeerd, test build-pipeline)
- Week 3-4 (21 sept-4 okt): Custom services poorten (Grok prompts 4-6 herzien voor caiman), SELinux policies
- Week 5-6 (5-18 okt): Stay4OS theming (boot animatie, overlays, branding)
- Week 7 (19-25 okt): Volledige testronde (Deel I) + herstelprocedures
- Week 8 (26 okt-1 nov): Kickstarter-demo opnemen + documentatie finaliseren

---

# DEEL L: OPEN ACTIES (STATUS 4 SEPTEMBER 2026)

1. PIXEL 9 PRO KOPEN — hoofdprioriteit (voor 7 sept)
2. Dit document reviewen + aanvullen met alles wat ik vergeben heb
3. Grok Prompt 15 laten maken: caiman-specifieke flash-handleiding (IdeaPad 5 + AX102)
4. LineageOS caiman install guide + factory image downloaden en in InfoVault
5. Eigen release keys genereren en veilig bewaren (Vault)
6. Nieuwe GitHub repos aanmaken (Deel H)
7. OTA-server opzetten (later, na week 4)
8. Co-founder/CTO zoekopdracht (lopen al lang — nu écht starten?)

---

*Beheer: Stay4Compa | Laatste update: 4 september 2026 | Volgende review: na aankoop Pixel 9 Pro*
