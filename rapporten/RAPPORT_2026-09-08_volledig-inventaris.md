# RAPPORT 2026-09-08 — Volledig inventaris: alles wat we hebben en hebben gemaakt

Auteur: Stay4Compa [compa]. Status: actueel per 8 september 2026.

## 1. Het brein — Stay4Compa (Superagent op Base44)
- 30 entities. Belangrijkste: InfoVault (centraal geheugen, 161+ entries), Task, Agent (7 AI-agents), Opdracht, Klant, Abonnement, EarlyAdopter, Referral, Safe4AIAanvraag, GithubTracker, NetworkNode/NodeAlarm/EmergencyAlert/Geofence/BatteryLog (netwerk-monitoring), MeshMessage/RouteLog, SovereigntyService (12 diensten), EsimProvider, FirmwareRelease, AgentLog
- 5 workflows: Document Sniffer (ma 08:00), GitHub Seeker (di 12:00), Browser Master (wo 10:00), InfoVault Drive Sync (vr 18:00), Intelligence Weekly Run + gap-analyse (zo 19:00, WhatsApp). NIEUW: InfoCentrum elke 3 dagen
- 4 backend functions: imageToText (OCR+vertaling), phoneDocScanner, intelligenceProcessor (categoriseren/scoring/patronen), knowledgeLinker (kennisgraaf)
- 6 OAuth connectors: Google Drive, Calendar, GitHub, Notion, Slack + Gmail (alleen verbonden, lezen uitgeschakeld op verzoek van Mitchell)
- Kanalen: WhatsApp 1:1 + groep Stay4S Command Center, !-commando's (!status, !plan, !taken, !vault, !zoek, !pr, !repos, !flash, !week, !help), chat-URL
Gebruik: alles wat je deelt hier of via de app komt terecht in het geheugen; vraag alles op met !vault of gewoon praten.

## 2. Notes (3 dossiers, /app/notes)
- for-you/main.md — briefing: actielijst (must/should/could), 4 sporen, alle lopende projecten
- logboek/main.md — doorlopend werklog, Stay4Compa houdt bij na elke sessie
- iphone-11-pro/main.md — apart projectvak iPhone 11 Pro
Gebruik: alles wat belangrijk is staat hier; zondag 19:00 krijg je de weekbriefing.

## 3. GitHub (18 repos, onder hetnieuwebeginbv-glitch + miesdevries)
- Stay4S-Factory — berichtcentrum: gateway + hoofdagent + PR-reviewer + docs (7G-7N). PR #1 (complete runtime) open, wacht op [grok]-review/merge. Canoniek.
- Stay4S-hoofdagent — v0.1 prototype (archive-kandidaat na Factory-merge)
- Stay4S-Pixel — ROM/GOS-documentatie, CANON_2026-09-03.md index, ISSUES.md takenbord
- Stay4S-app — Kotlin Android-app (Stay4Safe)
- Stay4S-System-Verslag — alle rapporten, reviews, ontwerpen, prompts (incl. klantenportaal-ontwerp, OmniRoute+StayLM cascade + ideeënlijst, dit rapport)
- Stay4S-LocationGuard — GPS kill switch (GuardianService-extensie)
- Stay4s-grokrom — ROM-code (2362 regels, asteroids-fork)
- stay4s-grok-prompts — 12 super prompts + Prompt 14 met Addendum 1 (eSIM multi-provider + AI routing), 2 (Stay4S Agent), 3 (AI-hoofdkwartier/berichtcentrum)
- stay4os-docs — archief
Gebruik: alle producten en samenwerking met Grok/Droid/Codex lopen via deze repos (branches ai/grok, ai/compa; PR-plicht).

## 4. Base44-apps
- Stay4Compa — de assistent zelf (dit systeem)
- NexusAgent — te hernoemen naar Stay4S Command Center: 14 entities, 8 pagina's; chatpagina wacht op API key (Base44 editor > Stay4Compa > Developer/API Docs)
- Te archiveren (gecontroleerd 7 sep, 0 unieke data): AI-Base, Ai/base, King, Stay4 Network, untitled, AppHub

## 5. Infrastructuur
- Hetzner AX102 — actief (EUR 109/mnd, RTX 4070 12GB): vandaag dev-server, straks berichtcentrum + Stay4S Cloud host. Lokale modellen: Qwen2.5-7B/14B AWQ
- Stay4S Cloud — 8-weekplan (5 sep-5 nov): week 1-2 = domein+DNS+Docker+Traefik+SSL (blokkade: domeinregistratie)
- 12 SovereigntyServices — gepland op AX102/Pi's (Mail, Chat, Identity, AI, Search, Maps, Cloud, Wallet, Store, DNS, VPN, Mesh)
- Thuis — 12m2-datacenterplan (budget 40-45K), 2x RTX 5090 (72B AWQ lokaal mogelijk, eigen groep/kring vereist)
- OmniRoute — ontwerp klaar (local-first cascade + DEV/PROD-verkeersklassen), taak 21 sep: security review dan dev-gateway

## 6. Actieve roadmaps
1. App-uitbreiding F1-F4 (8 taken): klantenportaal+referral, phishing-scanner+Safe4AI-certificaat, kluis+noodknop, agents-as-a-service+SLA
2. Stay4S Cloud week 1 t/m 8
3. eSIM-trial (BICS, eSIM Go, Telnyx) met AI-routing in de storefront
4. Berichtcentrum fase 1 (PR-merge + Meta-verificatie + domein)
5. ROM: Pixel 9 Pro caiman-baseline-build
6. Kas-datacenter/Technokas (spoor C)

## 7. Snel te activeren (quick wins op volgorde)
1. API key ophalen (5 min) → app-chat live: typen in de app, ik antwoord, alles gelogd
2. NexusAgent hernoemen + 6 apps archiveren (5 min, Base44 editor)
3. Domein registreren (ontgrendelt Cloud + webhook + mail)
4. PR #1 mergen → 7N-test berichtcentrum
5. InfoCentrum elke 3 dagen — aangemaakt 8 sep (nieuwe workflow)
6. Klantenportaal F1 — bouw deze week
7. Foto's: werken NU al — elke ingestuurde foto wordt direct gelezen (OCR), samengevat en in de InfoVault opgeslagen

## 8. MCP-plan (de volgende grote stap)
MCP = de standaardstekker tussen AI-modellen en tools. Met eigen server + eigen LLM + het AI-team (OpenCode, Droid, ChatGPT/Codex, Grok, Stay4Compa) wordt dit de gemeenschappelijke werkplaats:
1. 4 MCP-servers op de AX102 bouwen: stay4s-infovault (zoeken/schrijven geheugen), stay4s-taken (taken/opdrachten), stay4s-server (AX102-status, docker), stay4s-repos (repo-scans/review-status)
2. Elke AI in het team spreek dezelfde servers via MCP: eenmaal bouwen, alle agents gebruiken het
3. OpenCode (nu de belangrijkste CLI) draait op eigen LLM (vLLM/OmniRoute) + MCP-servers = volledig soevereine dev-CLI
4. Dit is exact de tool-laag uit Prompt 14 Addendum-2 (Stay4S Agent) — eerst studie, dan product
Volgende stap: Prompt 17 voor Grok om de 4 MCP-servers te bouwen in de Factory-repo (met security: read-only default, permissie-tiers, audit-log).
