# PROMPT 14: Stay4S 12m2 AI-Cloud, Stay4LM site, eSIM & Domains

Plak dit in Grok:

---

Je bent een senior infrastructure architect en telecom/product developer die werkt voor Stay4S B.V. Ontwerp de complete commerciële en technische architectuur voor het Stay4S 12m2 datacenter met 5 verdienmodellen. Output: copy-paste ready configs, scripts en docker-compose bestanden.

CONTEXT:
- Bedrijf: Stay4S B.V. — privacy-first, AI-native, Europees alternatief voor Big Tech
- Hardware budget: €33.500 + €10K buffer (totaal €40-45K), bouwperiode oktober-november 2026
- Locatie: 12m2 kamer thuis in Nederland, 3 aparte elektrische groepen (3500W continu), 3,5kW mini-split airco
- Doel: 5 verdienpijlers live krijgen: (1) Stay4LM publieke AI-chatsite, (2) GPU-verhuur, (3) managed hosting voor NL MKB, (4) Stay4S eSIM (Belgische profielen), (5) Stay4S Domains (DNS + reseller)
- Doelgroep: Nederlandse/Belgische consumenten en MKB die van Big Tech af willen; bestaande wachtlijst early adopters
- Bestaand: Hetzner AX102 (wordt vervangen door thuisrack), 12 SovereigntyService plannen (Keycloak, Mailu, Matrix, SearXNG, MinIO+Nextcloud, BTCPay, Pi-hole+Unbound, WireGuard, F-Droid, Ollama), Raspberry Pi 5 edge nodes, Meshmatic LoRa mesh
- Belgische eSIM keuze is bewust: BIPT dwingt open access wholesale af bij Proximus/Orange/Telenet-BASE — makkelijker dan NL. Data-only eSIMs zijn licht gereguleerd (geen nummerportering/112-zware verplichtingen). Volledige MVNO met +32-nummers is latere fase.

SCHRIJF DE VOLGENDE LEVERANCIEREN:

1. FYSIEKE ARCHITECTUUR (12m2)
   - Racklayout 27U: welke units voor wat (switch, firewall, 3x Proxmox nodes, 2x GPU-nodes met 4x RTX 4090, TrueNAS, patchpanel, PDUs, UPS)
   - Stroomberekening: verbruik per device, totaal continu en piek, verdeling over 3 groepen
   - Koeling: benodigde capaciteit, luchtstroom in de kamer, ontvochtiger
   - Netwerktopologie: OPNsense firewall, 10GbE switch, VLAN-plan (VLAN 10 Stay4S-productie, VLAN 20 verhuur/GPU, VLAN 30 gast/SIP, VLAN 40 management) met bijbehorend IP-schema 10.10.x.x
   - Internet: zakelijk glasvezel met vaste IP's + 5G-backup, failover-configuratie
   - Monitoring: rookmelder, CO2-blusser, camera, temperatuur/vochtsensors gekoppeld aan NodeAlarm-structuur (bestaande entity: node_id, severity, threshold, type, value)

2. STAY4LM PUBLIEKE CHATSITE (pijler 1, live in week 1-2 na hardware)
   - Front-end: LibreChat of Open WebUI — docker-compose config met Keycloak OIDC-koppeling (Stay4S Identity)
   - Backend: vLLM of Ollama op 2x RTX 4090, welke modellen (Llama 3.x, Qwen 2.5, klein custom StayLM), quantisatie-advies voor 48GB VRAM totaal
   - Abonnementenlaag: 3 tiers (Free 20 berichten/dag, Pro €9/mnd onbeperkt, Team €29/mnd API-toegang) — koppeling met bestaande Abonnement-entity structuur en BTCPay/Stripe betaling
   - Domein: lm.stay4s.com, nginx reverse proxy, Let's Encrypt, rate limiting, anti-abuse
   - Volledige docker-compose.yml voor de hele Stay4LM stack, klaar om te draaien

3. GPU-VERHUUR (pijler 2)
   - 2x RTX 4090 via vast.ai en/of TensorDock: welke agent-software, VLAN-isolatie van productie, verwacht rendement bij 60-70% bezetting
   - Veiligheid: verhuur-VM's krijgen geen toegang tot Stay4S-netwerk — firewall-rules en netwerkisolatie uitgewerkt
   - Switch-strategie: eigen AI-dienst heeft prioriteit; bij piekgebruik StayLM verhuur-capaciteit bijschuiven en vice versa

4. MANAGED HOSTING VOOR NL MKB (pijler 3)
   - Productpakket: privacy-first VPS (Proxmox VM templates), managed Nextcloud, managed PostgreSQL — met prijzen per maand
   - Verkoopargument en eerlijke SLA (99% uptime, geen 99,99% beloftes voor thuisrack), backupschema (3-2-1), herstelprocedures
   - Juridisch minimum: algemene voorwaarden-punten, AVG/verwerkersovereenkomst-checklist, wat je NIET mag hosten zonder extra vergunningen
   - Onboarding-script: van betaling tot draaiende klant-VM in 30 minuten

5. STAY4S ESIM MET BELGISCHE PROFIELEN (pijler 4, snelste geld)
   - White-label data-only eSIM via aggregator: vergelijk eSIM Go, 1Global, eSIM Access, BICS — welke heeft Belgische netwerkprofielen (Proximus/BASE/Orange), welke API, welke kosten/deposit
   - Storefront: Stay4S eSIM bestel pagina in de bestaande site — API-integratiecode (Python/FastAPI of Node) voor: plan-uitgifte, eSIM-profiel QR-levering, data-verbruik tracking, verlenging
   - Prijzen: wholesale €1-3/GB vs retail €5-10/GB, abonnementsvormen (5GB/10GB/25GB per maand)
   - SIP/VoIP-laag eroverheen: Asterisk of Kamailio + RTPengine op eigen rack, Stay4S-nummers via SIP-provider (bijv. een NL VoIP-wholesaler), zo wordt het "Stay4S telefonie" op Belgische data
   - Grenzen: wat data-only eSIM wettelijk wel/niet hoeft (112-bereikbaarheid van het device, geen eigen nummers), en wanneer de stap naar echte MVNO/BIPT-notificatie nodig is — geef het beslismoment (aantal actieve SIM's/maand)
   - Stappenplan week 1-6: account aanmaken, API-test, 3 test-SIM's, soft launch aan early adopters

6. STAY4S DOMAINS (pijler 5)
   - Eigen authoritative DNS: PowerDNS of NSD als primary thuis + secondary op kleine VPS (anycast niet nodig), zone-beheer via PowerDNS-Admin
   - Interne .stay4s-zone op Unbound/Pi-hole voor eigen apparaten (gateway.stay4s, vault.stay4s, lm.stay4s)
   - Reseller: koppeling met Realtime Register (Nijmegen) API of OpenSRS — domeinregistratie, DNS-beheer en doorverkoop in eigen dashboard, met marges
   - Wanneer eigen registrar worden: SIDN-deelnemerschap (.nl) en ICANN-accreditatie (.com) — kosten, drempels, break-even punt in aantal domeinen

7. BEVEILIGING & SOEVEREINITEIT SLA
   - Zero-trust basis: WireGuard-toegang voor beheer, geen open poorten behalve reverse proxy, fail2ban, TLS overal
   - Backups: TrueNAS snapshots + externe versleutelde backup (Hetzner Storage Box of familie-Pi), restore-test procedure
   - Wat gebeurt er bij stroomuitval, internetuitval of hardware-failure per pijler — en hoe communiceer je dat naar klanten

8. ROADMAP & FINANCIEN
   - Tijdlijn oktober-november 2026: wat eerst, wat daarna, welke pijler levert wanneer eerste omzet
   - Maandelijkse lasten (stroom, internet, licenses, VPS-secondary) vs verwachte omzet per pijler in maand 1, 3, 6
   - Break-even moment en wat te doen als GPU-verhuur tegenvallt (plannen B per pijler)
   - KPI's om wekelijks te meten (actieve abonnementen, GPU-uren verkocht, uptime, eSIM-activeringen, domeinen beheerd)

Geef alles als copy-paste ready code blocks, docker-compose bestanden, scripts en tabellen. Echte productnamen, echte prijzen 2026, echte API-namen. Waar iets onzeker of afhankelijk van het gesprek met een leverancier is, zeg je dat expliciet. Nederlands in uitleg, Engels in code. Volledig executeerbaar, geen placeholders.

---

# PROMPT 14 ADDENDUM: Multi-Provider eSIM + AI Routing Layer
# (september 2026 — toegevoegd na besluit: 3 providers trialen, wisselbaar op beschikbaarheid)

PLak dit als AANVULLING achter Prompt 14 in Grok:

---

AANVULLING OP LEVERANCIER 5 (Stay4S eSIM): de storefront wordt MULTI-PROVIDER met een wisselbare vendor-laag. BELANGRIJKSTE EIS: providers kunnen op elk moment gewisseld worden op basis van beschikbaarheid, zonder dat de klant of de storefront-code verandert.

TRIAL-PROVIDERS (al geregistreerd in EsimProvider-database):
- BICS (België, Brussel) — Belgische netwerkprofielen (Proximus/BASE/Orange), wholesale carrier
- eSIM Go (VK) — API-first aggregator, sterk EU
- Telnyx (VS) — 650+ netwerken, usage-based, global
- (Later uit te breiden: 1Global, Monty Mobile, eSIM Access, Transatel voor MVNO-stap)

SCHRIJF DE VOLGENDE EXTRA LEVERANCIEREN:

5A. PROVIDER ABSTRACTION LAYER (FastAPI)
- Eén ProviderAdapter-interface (Python ABC) met per provider een adapterklasse: BicsAdapter, EsimGoAdapter, TelnyxAdapter
- Standaard methoden per adapter: list_plans(), provision(plan, customer), get_status(order_id), get_usage(iccid), topup(iccid, gb), deactivate(iccid)
- Normalisatie: alle provider-antwoorden worden gemapt naar één intern schema (Plan, Order, Usage) zodat de storefront niets van de specifieke provider weet
- Config-gestuurd: welke provider actief is per product/pakket staat in een database-tabel (niet in code) — beschikbaarheidsschakelaar
- Nieuwe provider toevoegen = alleen een nieuwe adapterklasse schrijven, nergens anders iets aanpassen

5B. AI ROUTING LAYER (StayLM op eigen GPU's)
- Beslislogica die per bestelling automatisch de beste provider kiest op basis van: beschikbaarheid (real-time API-check), prijs per GB op dat moment, historische prestaties (trial-scores), netwerkdekking van de bestemming
- Eerst als regel-engine (deterministisch, FastAPI) met AI-scores als invoer — de AI beoordeelt en rangschikt, de regels garanderen dat er altijd een fallback is
- Fallback-cascade: primaire provider onbeschikbaar/stored-out → automatisch tweede → derde, met logging en klantwaarschuwing pas als alle drie falen
- Availability monitor: cron-job die elke 15 min alle 3 adapters pingt (list_plans als health-check), resultaat in een ProviderHealth-tabel; provider die 3x achter elkaar faalt wordt automatisch op non-actief gezet
- Alle routeringsbeslissingen worden gelogd (provider, reden, prijsverschil) — dit wordt de dataset waarmee StayLM slimmer wordt

5C. TRIAL-SCORING SYSTEEM (voert data aan onze eigen AI)
- Scoring-model 0-100 per provider, gewogen: activatietijd (15%), snelheid/latency NL+BE (25%), dekking (20%), prijs/GB (20%), API-kwaliteit (10%), support-reactietijd (10%)
- Invoer: handmatige testresultaten (test-SIM's) + automatische metingen (health-checks, provision-tijden uit logs)
- Output: vergelijkingsrapport dat StayLM maandelks genereert: "provider X is dit maand 12% duurder geworden in BE, advies: verhang Stay4S eSIM België naar provider Y"
- Alle trial-data gaat naar Qdrant (vector store) zodat StayLM er vragen over kan beantwoorden ("welke provider is het beste voor Frankrijk?")

5D. OVERSTAP-PLAN (contractueel en technisch)
- Wat er contractueel nodig is om zonder boete te wisselen: geen minimum-afname tekenen in trial-fase, maandelijkse opzegbaarheid als voorwaarde
- Technische overstapchecklist: adapter op non-actief zetten, actieve abonnementen afbouwen (uitputten, niet afkappen), nieuwe orders naar nieuwe provider
- Klantcommunicatie-template bij provider-wissel (e-mail/whatsapp: "je pakket blijft werken, er verandert niets voor je")

Geef ook hier copy-paste ready Python/FastAPI code (ABC-interface, alle 3 adapters met echte API-endpoints van de providers, health-check cron, scoring-berekening) en een ERD van de tabellen (ProviderAdapter-config, ProviderHealth, RoutingLog, TrialScore). Echte endpoints van eSIM Go en Telnyx API's; voor BICS geef je de aanvraagprocedure voor API-toegang expliciet aan, want die vereist een wholesale-overeenkomst. Nederlands in uitleg, Engels in code. Volledig executeerbaar, geen placeholders.

---

# PROMPT 14 ADDENDUM-2: Stay4S Agent — soevereine AI-assistent (doet wat Superagent doet, maar van onszelf)

Plak dit als TWEEDE AANVULLING achter Prompt 14 in Grok:

---

NIEUWE MODULE: STAY4S AGENT (deze is strategisch — dit wordt het consumer-product achter de Stay4LM-site)

DOEL: bouw een soevereine persoonlijke AI-assistent die alles kan wat een gehoste AI-assistent (ChatGPT met tools / een "superagent") kan — maar draaiend op onze eigen hardware, met onze eigen modellen, en UITGEBREIDER: ook acties uitvoeren, herinneren, plannen en met andere Stay4S-diensten praten. Dit is de kern van Stay4S Prime / AetherCore als product: privacy-first alternatief voor ChatGPT-with-tools, geen data verlaat de EU.

CAPABILITEITEN DIE DE AGENT MOET HEBBEN (1-op-1 met wat een superagent kan, plus uitbreiding):
1. Chat en redeneren (basis LLM via vLLM)
2. Tool-gebruik: shell-commando's, bestandsbeheer, git/GitHub, e-mail (IMAP/SMTP), agenda (CalDAV), berichten (Matrix/WhatsApp-bridge), web zoeken (SearXNG), web browsen (headless browser), API-calls
3. Langetermijngeheugen: feiten-database + vectorgeheugen (Qdrant) + automatische gespreks-samenvattingen per gebruiker
4. Geplande automations: cron-taken die de agent zelf aanmaakt en uitvoert ("elke maandag rapport")
5. RAG over de InfoVault-kennisbank (Qdrant + Postgres)
6. Sub-agent delegatie: gespecialiseerde mini-agenten (onderzoeker, codeur, planner) die de hoofdagent kan aansturen
7. Spraak: Whisper (STT) + Kokoro/Piper (TTS) — praten met je assistent
8. Visie/OCR: Qwen2.5-VL voor foto's, documenten, schema's
9. Database-CRUD op eigen entities (bestellingen, abonnees, taken)
10. UITBREIDING: gespreks-oppervlakken — dezelfde agent praat via de Stay4LM-site, Matrix, WhatsApp-bridge en de Stay4S app

ARCHITECTUUR:
- Orchestrator: LangGraph (Python) of OpenHands als basis; agent-loop met tool-calling, planning, reflectie
- Tool-laag via MCP (Model Context Protocol): per tool een MCP-server (filesystem, shell, git, mail, calendar, messaging, search, browser, database)
- Modellen op eigen 4090's via vLLM: redeneer-model (Qwen 2.5 72B AWQ of Llama 3.3 70B gequantiseerd; valt 48GB VRAM + CPU-offload), vision (Qwen2.5-VL 7B/32B), Whisper-large STT, Kokoro TTS
- Geheugen: Postgres (feiten + profiel per gebruiker) + Qdrant (vectorgeheugen) + samenvattings-pijplijn na elk gesprek
- Automations: n8n (self-hosted) voor cron, gekoppeld aan de agent via webhook
- Front-ends: LibreChat/Open WebUI (web), Matrix-bridge (chat), later mobiele app
- Multi-tenant: elke Stay4LM-abonnee krijgt eigen geheugen-ruimte en eigen tool-permissies

VEILIGHEID (cruciaal, leer van hoe superagents dit doen):
- Externe acties (e-mail versturen, berichten, betalingen) alleen na menselijke goedkeuring of expliciete permissie-tier
- Permissie-tiers per gebruiker: read-only / intern / extern-actief
- Volledige audit-log van elke tool-aanroep
- Sandbox voor shell (Docker-container per sessie, geen toegang tot host)
- Rate limits + kosten-plafond per abonnement

SCHRIJF DE VOLGENDE LEVERANCIEREN:
6A. Architectuurdiagram (tekst/mermaid) van de complete agent-stack
6B. docker-compose.yml voor de hele stack (vLLM, orchestrator, MCP-servers, Qdrant, Postgres, n8n, Whisper, TTS, front-end)
6C. Agent-orchestrator skeleton (Python): agent-loop, tool-registry, planning-stappen, geheugen-opslag per gebruiker
6D. MCP-server voorbeeldconfig + 2 uitgewerkte MCP-servers (shell-sandbox + SearXNG-search)
6E. Geheugenschema: SQL-tabellen (user_facts, conversation_summaries, tool_audit_log) + Qdrant-collections
6F. Permissie- en audit-laag: hoe goedkeuringen werken per tier
6G. Roadmap: MVP in 4 weken (chat+tools+geheugen), fase 2 spraak+visie, fase 3 sub-agents en automations
6H. Realiteitscheck: wat kan een lokaal 70B-model NIET wat hosted frontier-modellen wel kunnen — en hoe compenseer je dat (fijnere tool-steps, RAG, fallback naar groter model achter betaalde API als klant daarvoor kiest)

Geef alles als copy-paste ready code en configs. Echte package-namen en versies 2026. Nederlands in uitleg, Engels in code. Volledig executeerbaar, geen placeholders. Wees eerlijk over beperkingen van lokale modellen — geen mooipraterij.

---
