# STAYLM-00 — DE BAAS, ZIJN TWEE HANDEN EN DE AGENT FACTORY
## Mitchell's visie, uitgewerkt tot architectuur — 5 september 2026

MITCHELL'S VISIE (letterlijk):
"Stay4S moet een AI worden die ik kan bedienen vanaf WhatsApp. Zelfs WhatsApp-berichtjes kan laten sturen. En daarna in mn eigen berichtencentrum ook. Het moet een AI zijn die andere AI kan maken en slimmer wordt met de dag. Maar als die AI het zo druk heb, moet ik eigenlijk 3 speciale AI-agenten hebben denk ik. Ik de baas met StayLM-00. Een linker- en rechterhand voor StayLM-00."

---

# DE TRIO-ARCHITECTUUR

## StayLM-00 — DE BAAS (hoofdagent)
- De ENIGE AI die met Mitchell praat via WhatsApp (whitelist: eerst alleen Mitchell)
- Kent Mitchell persoonlijk: geheugen met feiten, voorkeuren, beslissingen, gesprekssamenvattingen
- Ontvangt elk bericht, bepaalt intentie, plant, DELEGEERT aan zijn handen
- IS de Agent Factory: maakt zelf nieuwe specialisten als het werk dat vraagt
- Rapporteert terug: wat gedaan is, wat misging, wat hij voorstelt
- KAN zelf WhatsApp-berichten sturen (via eigen berichtencentrum, Meta Cloud API)
- Spraakberichten verstaan (Whisper), foto's/documenten lezen (Qwen2.5-VL)

## LINKERHAND — DE BOUWER (executor)
- Bouwt en bestuurt: code, builds, repo-werk, CI, infra
- Bestuurt de worker-specialisten (PR-reviewer, repo-auditor, doc-schrijver)
- Monitort systemen: logs, alerts, builds die breken
- Werkt autonoom door als de Baas bezig is — taken komen binnen via de opdrachten-wachtrij
- Mag: GitHub lezen, bouwen, rapporteren. Externe acties (push/merge) alleen na goedkeuring van de Baas

## RECHTERHAND — DE DENKER (analist)
- Onderzoekt: web, documenten, code-reviews van derden, gesprekken samenvatten
- Beheert de InfoVault: categoriseren, taggen, verbanden leggen, verrijken
- Bereidt communicatie voor: weekplanningen, rapporten, concepten
- Leert patronen zien: wat komt terug, wat mist, wat moet de Baas weten
- Mag: alles lezen, niets versturen zonder de Baas

## WAAROM DIT WERKT ALS HET DRUK WORDT
De Baas blijft altijd beschikbaar op WhatsApp want het zware werk ligt bij zijn handen. Zijn handen werken door terwijl hij met Mitchell praat. Loopt een hand vast → escalatie naar de Baas → evt. naar Mitchell. En is er werk dat geen hand kan? Dan maakt de Baas er een nieuwe specialist voor (Agent Factory).

---

# DE GROEILUS: SLIMMER MET DE DAG

Elke dag, automatisch (nightly learning run om 03:00):
1. Dag samenvatten: alle gesprekken, opdrachten, resultaten van die dag
2. Geheugen bijwerken: nieuwe feiten over Mitchell, beslissingen, voorkeuren → user_facts
3. InfoVault verrijken: categoriseren, tags, verbanden, belangrijkheid
4. Qdrant bijvullen: nieuwe kennis wordt doorzoekbaar (RAG)
5. Patroondetectie: welk werk kwam vaker terug? → SUGGESTIE aan de Baas: "we hebben 4x deze week een X-taak gedaan — zal ik een specialist aanmaken?"
6. Groeirapport: 1 korte WhatsApp aan Mitchell: wat het systeem vandaag leerde

Dit is de zelfversterkende cyclus die Mitchell wil: de AI die andere AI maakt én elke dag slimmer wordt.

---

# WHATSAPP-BERICHTEN STUREN — DE EERLIJKE REGELS

Binnen het EIGEN berichtencentrum (Meta WhatsApp Cloud API) KAN StayLM-00 zelf berichten sturen. Maar Meta heeft regels:
1. Binnen 24 uur na het laatste bericht van de gebruiker: vrije antwoorden toegestaan (perfect voor een persoonlijke assistent — Mitchell stuurt iets, StayLM-00 antwoordt wanneer nodig)
2. Buiten dat venster: alleen goedgekeurde template-berichten (bijv. het dagelijkse groeirapport of waarschuwingen)
3. Eerste 1000 gesprekken per maand gratis, daarna betalen per gesprekstype
4. Verificatie vereist (weken — al op de actielijst)

Op de lange termijn, als de walled garden van Meta knelt: eigen berichtencentrum (Matrix/Stay4S Chat) als eerste kanaal, WhatsApp als brug ernaartoe. Dat is al zo ontworpen: StayLM-00 is kanaal-onafhankelijk — zelfde brein, zelfde geheugen, meerdere deuren (WhatsApp, web-chat, later Stay4S Chat).

---

# VAN GEBOWDE FUNDAMENT NAAR TRIO

Wat er al staat (Stay4S-Factory + 7G-7N):
- Gateway met WhatsApp-webhook (HMAC, whitelist) ✓
- Opdrachtensysteem met wachtrij ✓
- Agent Factory met register en beveiliging (FACTORY_TOKEN, allowlist, cap 20) ✓
- Hoofdagent-graph (intentie → geheugen → planning → delegatie → antwoord) ✓
- InfoVault + Qdrant + geheugen-tabellen ✓
- PR-reviewer als eerste worker ✓

Wat het trio toevoegt:
1. StayLM-00 = de bestaande hoofdagent, hernoemd en gepersonaliseerd (eigen naam, eigen geheugen, eigen groeilus)
2. Twee nieuwe factory-templates: linkerhand (bouwer) en rechterhand (denker) — de handen worden door de Baas gespawnd bij eerste startup
3. Nightly learning run (cron in de compose-stack)
4. Groeirapport-template voor WhatsApp
5. Later: eigen berichtencentrum-UI (Command Center) die op dezelfde StayLM-00 API aansluit
