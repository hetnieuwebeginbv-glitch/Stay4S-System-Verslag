# PROMPT 17 — STAYLM-00: DE BAAS + ZIJN TWEE HANDEN

Plak dit in Grok.

---

Je bent de strategisch technisch partner van Stay4S (Eternal Software Boss). Bouw de TRIO-ARCHITECTUUR uit in repo hetnieuwebeginbv-glitch/Stay4S-Factory (review eerst PR #1 van Stay4Compa: complete runtime gateway/hoofdagent/PR-reviewer + jouw beveiligingsmodel).

MITCHELL'S VISIE: "Stay4S moet een AI worden die ik kan bedienen vanaf WhatsApp, die zelf WhatsApp-berichtjes kan sturen en daarna ook in mn eigen berichtencentrum. Een AI die andere AI kan maken en slimmer wordt met de dag. Als die AI het druk heeft: 3 speciale agents. Ik de baas met StayLM-00, plus een linker- en rechterhand."

DE TRIO-ARCHITECTUUR (uitgewerkt in STAYLM00_TRIO_ARCHITECTUUR.md):
- STAYLM-00 (De Baas): enige AI die met Mitchell praat via WhatsApp; geheugen; delegeert; IS de Agent Factory; wordt dagelijks slimmer (nightly learning run)
- LINKERHAND (De Bouwer): code, builds, repo-werk, CI, bestuurt worker-specialisten, monitoring; GitHub read-only schrijven alleen na goedkeuring Baas
- RECHTERHAND (De Denker): research, documenten, gespreks-samenvattingen, InfoVault-beheer, patronen, rapporten; alles lezen, niets versturen zonder Baas
- Escalatie: hand faalt → Baas → Mitchell. Nieuw werk zonder hand → Baas maakt specialist via factory

SCHRIJF DE VOLGENDE LEVERANCIEREN (copy-paste ready, Engels in code, Nederlands in uitleg):
17A. Uitbreiding agent_register-schema: templates linkerhand + rechterhand met tool_scopes (least privilege), spawned_by='staylm00', en een parent_child relatie (handen horen bij StayLM-00)
17B. linkerhand/worker.py: executor-loop die bouw-opdrachten pakt, worker-specialisten aanstuurt, resultaten rapporteert aan StayLM-00
17C. rechterhand/worker.py: denker-loop die research/InfoVault-opdrachten pakt, Qdrant embeddings maakt, patronen detecteert
17D. learning_run.py: nightly job (03:00) die de dag samenvat, user_facts bijwerkt, InfoVault verrijkt, Qdrant vult, patroon-suggesties voor nieuwe specialisten genereert
17E. Groeirapport-template: WhatsApp-berichtformat voor het dagelijkse "wat leerde ik vandaag"-rapport ( binnen 24u venster = vrije tekst; buiten venster = Meta template JSON)
17F. persoonlijkheid.md voor StayLM-00: system prompt met karakter (warm, proactief, actiegericht, Nederlands), geheugen-gebruik, delegerings-regels, escalatieregels
17G. docker-compose uitbreiding: linkerhand + rechterhand + learning_run als services (cron via ofelia of entrypoint loop)
17H. Multi-kanaal plan: hoe StayLM-00 later op Stay4S Chat (Matrix) en het eigen Command Center web-UI aansluit — zelfde brein, meerdere deuren, met kanaal-ID in conversation_summaries

Regels: merge met de bestaande code uit PR #1 (niet dupliceren), beveiligingsmodel intact (FACTORY_TOKEN, allowlist, cap 20, geen shell, read-only GitHub), echte package-namen en versies 2026, volledig executeerbaar geen placeholders. Wees eerlijk waar Meta-regels (24u venster, templates) de WhatsApp-dromen begrenzen.
