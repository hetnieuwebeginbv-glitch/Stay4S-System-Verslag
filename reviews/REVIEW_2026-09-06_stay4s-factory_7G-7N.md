# REVIEW 2026-09-06 — Stay4S-Factory 7G–7N (Grok-levering)

Reviewer: Stay4Compa [compa]
Input: Grok-levering 7G–7N (5 lagen, compose, FastAPI, LangGraph, SQL, PR-reviewer, Meta-setup, testplan)
Methode: statische analyse (grok-output-review skill) + code-inspectie van main en ai/compa/complete-runtime in hetnieuwebeginbv-glitch/Stay4S-Factory

## OORDEEL: COMMENT
Ontwerp en afspraken: APPROVE.
Repo-status claim ("7G–7N staan lokaal en in repo"): REQUEST_CHANGES — de executeerbare runtime (7I gateway, 7J hoofdagent) staat NIET op main.

## 1. Statische analyse (skill)
5 agreement-checks afgevinkt: aparte FACTORY_TOKEN ✓ (env-model), image-pinning deels (zie 3.4), GitHub read-only ✓, WhatsApp HMAC op ruwe body + verify_token ✓ (claim, code ontbreekt op main), SQL parameterized — verifiëren na merge.

## 2. Wat staat er WEL op main (goed)
1. docs: 7G_ARCHITECTUUR, 7M_WHATSAPP_CLOUD_SETUP, 7N_TESTPLAN
2. docker-compose.yml: postgres 16.9, redis 7.4.2-alpine, qdrant v1.19.1 gepind; gpu-profile voor vllm; whisper-service; hoofdagent + agent-runtime + gateway services
3. sql/schema.sql: agent_register (UUID, CHECK, tool_scopes, spawned_by), opdrachten, infovault, tool_audit_log, conversation_summaries + seed van hoofdagent en pr-reviewer — solide
4. hoofdagent/tools_github.py (53 regels): uitsluitend GET-calls → read-only klopt
5. agents/pr_reviewer: SYSTEM.md + mcp_github.json met correcte allowlist (list/get) en denylist (push/merge), MCP-server versie gepind (2025.4.8)
6. .env.example: alleen placeholders, geen echte secrets ✓

## 3. Bevindingen

### 3.1 KRITIEK — runtime ontbreekt op main
gateway/ bevat alleen een lege __init__.py: geen app.py.
hoofdagent/ bevat lege __init__.py + tools_github.py: geen graph.py, geen worker.py, geen runtime_worker.
Gevolg: beide Dockerfiles craschen bij start (uvicorn gateway.app:app → ModuleNotFoundError; python -m hoofdagent.worker → idem; agent-runtime → hoofdagent.runtime_worker bestaat niet op main).
Commit 7117b68 claimt "gateway FastAPI + LangGraph hoofdagent" — die code staat niet in de commit.

### 3.2 KRITIEK — oplossing bestaat al
Branch ai/compa/complete-runtime (PR #1, wacht op [grok]-review) bevat wél de complete runtime: gateway/app.py (243), hoofdagent/graph.py (202), linkerhand.py (143), rechterhand.py (112), agents/pr_reviewer/main.py (119) — totaal 873 regels toevoeging.
ACTIE: merge PR #1 naar main, daarna is 7N (testplan) uitvoerbaar. [grok] review gevraagd.

### 3.3 BEVEILIGING — aandachtspunten
Poorten 5432/6379/6333/6334/8000/9000 gepubliceerd op de host → op de publieke Hetzner-IP van AX102 risico. Binden aan 127.0.0.1 of poort-mapping weglaten (docker-intern netwerk volstaat) + UFW actief houden.
HMAC-vergelijking moet timing-safe (hmac.compare_digest) — verifiëren na merge.
Redis streams: pending-reclaim (XAUTOCLAIM) voor gecrashte berichten ontbreekt — toevoegen.
Zwakke plek (bekend): FACTORY_TOKEN-houder kan specialisten spawnen. Mitigatie naast "token alleen in hoofdagent-container": rate-limit op POST /v1/factory/spawn en elke spawn loggen in tool_audit_log.

### 3.4 KLEIN
whisper + vllm images op :latest → pinnen bij eerste stabiele release.
ALLOWED_WA_IDS allowlist in env — goed, maar documenteren dat dit de WA-allowlist uit 7G is.
docs/7M is erg mager (6 regels) — aanvullen met de concretestappen uit sectie 5.

## 4. Repo-consolidatie (afspraak 4 sep)
Stay4S-Factory is canoniek zodra PR #1 gemerged is. Daarna Stay4S-hoofdagent archiveren (verwijs naar Factory).

## 5. Resttaak: Meta Business-verificatie — ALLEEN Mitchell (menselijke taak)
1. business.meta.com → account + bedrijfsverificatie (KvK/inschrijvingsbewijs) — dit duurt dagen tot weken, vandaag starten
2. App aanmaken + product WhatsApp toevoegen
3. Telefoonnummer koppelen + display name laten goedkeuren
4. Webhook URL (https://<ax102-domein>/webhook) + verify token invullen, subscribe op veld "messages"
5. Tot app Live: testnummer met max 5 ontvangers
6. Ondertussen testbaar zonder Meta via POST /v1/opdrachten met INTERNAL_TOKEN

## 6. Conclusie
Ontwerp, beveiligingsmodel en infrastructuur-keuzes zijn volgens afspraak (twee tokens, allowlists, cap 20, geen shell, GitHub read-only). Merge PR #1 om de levering echt executeerbaar te maken; start vandaag de Meta-verificatie want dat pad is het langzaamst.
