# ANALYSE: DE VIJF DRCINTAS-REELS IN DE STAY4S-ARCHITECTUUR
## Volledige synthese — 8 september 2026

# DE VIJF VONDSTEN

1. AGENT-BUILDER (reel DbqcQUgxlyC, aug 2026): gratis open-source repo, AI-agent bouwen zonder terminal via Claude Code (repo + setup-prompt plakken, API-key toevoegen). Demo: dagelijkse AI-nieuwssamenvatting op Telegram 08:00, auto-gedeployd naar cloud. Marketing-funnel ("comment AGENT").
2. OPUS 5 SYSTEEMPROMPT (reel DbVxumMR8bm, jul 2026): gelekt volledige systeemprompt Anthropic Opus 5 — 135.000+ tekens met tools-schema's, memory-filesystem, safety-behaviors. Studiemateriaal achter "comment PROMPT"-funnel.
3. AIRLLM (reel DbGRrbxx_q6, jul 2026): 70B-modellen draaien op 4GB VRAM door laag-voor-laag laden (layer streaming). Traag maar mogelijk. Open-source.
4. NVIDIA BUILD PLATFORM (reel DcoOrPrRqbJ, aug 2026): 100+ grote AI-modellen gratis achter één NVIDIA API-key via build.nvidia.com. Werkt met agents zoals Hermes.
5. NEMOTRON 3 ULTRA (reel Da25b99RCPT, jul 2026): NVIDIA's open-source coding-model, gratis alternatief voor Claude Code, via OpenCode + gratis NVIDIA API. Community-signalen gemengd.

# DWARSDOORDE LIJN 1 — MARKTVALIDATIE

Alle vijf reels gaan viraal (595-2.200 likes, honderden comments) op hetzelfde verlangen: "mijn eigen AI-agent, gratis, zonder tech-expertise".
Stay4S bouwt precies dat — maar dan als product (StayLM + Stay4S Agent), niet als hack.
De Agent Factory + StayLM-00-trio zit op de golflengte van de vraag; de markt is bewezen vóórdat we lanceren.

# DWARSDOORDE LIJN 2 — DE SOUVEREINITEITSGAT-ANALYSE

Per reel: wat is de externe afhankelijkheid en wat is het soevereine Stay4S-antwoord?

REEL | HUN AANPAK | AFHANKELIJKHEID | STAY4S-ANTWOORD
1 agent-builder | Claude Code + Anthropic API + cloud-deploy | API-kosten, data verlaat EU, derde-partij infra | Eigen factory op eigen infra (PR #1), eigen gateway, eigen geheugen
2 systeemprompt | Anthropic's geheim | n.v.t. (studiemateriaal) | Gebruik als blauwdruk voor StayLM-systeemprompt; onze architectuur (user_facts/InfoVault/Qdrant) spiegelt het memory-filesystem al
3 AirLLM | layer-streaming | geen — draait lokaal | ADOPTEREN: dev-tool op AX102 om 72B te testen zonder wachten op 4090's
4 NVIDIA Build | gratis API | data verlaat EU, gelimiteerde credits | Alleen dev/tests: provider in OmniRoute dev-gateway; productie = vLLM eigen hardware
5 Nemotron | open-source model | geen als zelf gehost | ADOPTEREN: coding-model voor OpenCode in multi-AI-workflow; zelf hosten op eigen GPU's = soevereine coding-agent

CONCLUSIE: 3 van de 5 tools (AirLLM, Nemotron, systeemprompt) versterken de soevereiniteit direct; 2 (agent-builder, NVIDIA API) zijn alleen dev-materiaal. GEEN enkele vervangt eigen infra — ze bewijzen juist dat Stay4S het gat vult dat zij laten: alles van anderen is óf gratis-maar-extern, óf lokaal-maar-geen-product.

# DWARSDOORDE LIJN 3 — CONCRETE INPASSING PER SPOOR

SPOOR A (DE AI):
- Dev-gateway op AX102: OmniRoute + NVIDIA Build als providers (alleen dev/agent-verkeer)
- StayLM-modelkeuze: Nemotron 3 Ultra testen als coding-kandidaat; AirLLM om 72B-gedrag te testen op de 4070
- StayLM-systeemprompt: Opus 5-lek als referentie bij het ontwerpen van de hoofdagent-instructies (tools-schema, geheugenregels, veiligheid)
- Factory: de "agent die agents bouwt" is al gebouwd — de reels bewijzen de verkoopbaarheid

SPOOR B (ROM & OS):
- Geen directe rol. De les: documentatie en onboarding simpel houden (de reels die viraal gaan beloven "no terminal" — de GrokPhone-flows straks ook zo simpel mogelijk presenteren)

SPOOR C (INDUSTRIEEL):
- Geen directe rol. AirLLM is wel relevant zodra edge-nodes bij kassen beperkte hardware hebben: trage maar zware inferentie op locatie.

SPOOR D (THUIS 12M2 / AX102):
- GPU-strategie scherper: 4070 (AX102) = 7B/14B AWQ via vLLM voor productie + AirLLM voor 72B-experimenten; 4090's = 72B productie
- Nemotron zelf gehost = gratis coding-agent in de multi-AI-workflow (Grok/Compa/OpenCode) zonder externe API-kosten
- Dev-tools-winkelwagen: NVIDIA-key aanmaken (gratis), OmniRoute deployen na security-review, AirLLM installeren

# DWARSDOORDE LIJN 4 — HET MARKETING-BLUEPRINT

drcintas' systeem: 1 reel = 1 concrete tool, belofte "gratis", call-to-action "comment X", repo als lead-magnet. 5x viraal in 2 maanden.
Stay4LM-lancering kan dit formaat kopiëren MET het verschil als pitch:
- HUN verhaal: "gebruik gratis tools van Big Tech"
- ONZE verhaal: "je eigen AI, op je eigen voorwaarden" — privacy, EU, geen abonnement op andermans grillen
Format-bouwstenen voor de launch: korte demo-reel van de Stay4LM-site (chat live), "comment"-funnel voor wachtlijst Early Adopters, de Agent Factory als haak ("bouw je eigen agenten"), Stay4Safe als safety-hook.

# ACTIEPUNTEN (IN PRIORITEIT)

1. NVIDIA developer account + API-key aanmaken (gratis, 10 min) — dev/tests
2. OmniRoute security-review en deploy op AX102 (taak bestaat al) + NVIDIA Build als provider
3. AirLLM installeren op AX102 als experiment-tool (naast vLLM, niet voor productie)
4. Nemotron 3 Ultra binnnenhalen en toetsen als OpenCode-model (kwaliteit zelf beoordelen — signalen gemengd); op termijn zelf hosten
5. Bij het schrijven van de StayLM-systeemprompt: Opus 5-referentie erbij pakken
6. Marketing-blueprint "Stay4LM launch-formaat" uitwerken (comment-funnel + sovereign pitch) — na livegang site

# FEITEN-CHECK (EERLIJKHEID)

- Alle vijf de reels komen van dezene maker; kwaliteit van de beloftes wisselt (Nemotron: "It is so bad" als top-comment; AirLLM: snelheid-grapjes). Behandel als lead-generators, niet als gouden standaard — feiten per vondst bewaakt in InfoVault.
- Geen van de tools mag in het productiepad van Stay4LM/Stay4S Cloud (data-soevereiniteit), behalve AirLLM en Nemotron zodra zelf gehost.
