# PROMPT 18 — STAY4S MCP-WERKPLAATS (Model Context Protocol voor het hele AI-team)

Geschreven door: Stay4Compa [compa], 8 september 2026
Doel: één set MCP-servers op de AX102 die alle AI-tools van het team gebruiken. Eenmalig bouwen, iedereen plugt erop. Dit is de tool-laag uit Prompt 14 Addendum-2 (Stay4S Agent), nu eerst voor ons eigen team.

## CONTEXT
- Eigen infrastructuur: Hetzner AX102 (Docker), later 2x RTX 5090 thuis
- Eigen LLM: vLLM met Qwen2.5-7B/14B AWQ op AX102 (72B later thuis), eventueel achter OmniRoute dev-gateway (OPENAI_BASE_URL)
- AI-team dat gaat gebruiken: OpenCode (belangrijkste CLI), Droid, ChatGPT/Codex CLI, Grok, Stay4Compa
- Local-first: GEEN externe API-keys (OpenAI/Anthropic), alle model-calls gaan naar ons eigen endpoint
- Repos: bouw in hetnieuwebeginbv-glitch/Stay4S-Factory, branch ai/grok/mcp-werkplaats, PR-plicht (Stay4Compa reviewt, prefix [grok])

## BOUW DE VOLGENDE 4 MCP-SERVERS (Python, officiële MCP SDK of FastMCP, versies 2026)

1. stay4s-infovault — geheugen: zoeken in de InfoVault-tabel (Postgres), entries schrijven (nieuwe vondsten, tags), zoeken op tags/belangrijkheid. Read-only by default; schrijven alleen met write-token.
2. stay4s-taken — taken en opdrachten: CRUD op Task/Opdracht-tabellen, statuswijzigingen, "wat is open en dringend?"-queries, deadline-overzicht.
3. stay4s-server — AX102-status: docker ps, schijf/geheugen, container-logs lezen, en uitsluitend via een allowlist specifieke containers herstarten. GEEN vrije shell-executie.
4. stay4s-repos — repo-scans: GitHub API read-only (open PR's, issues, laatste commits per repo), review-status-overzicht voor de PR-plicht.

## LEVERANCIEREN (genummerd, copy-paste ready)
A. Architectuurdiagram (mermaid): AI-clients (OpenCode, Droid, Codex, Grok CLI) → MCP-servers (SSE/HTTP intern) → infrastructuur (Postgres, Docker API, GitHub API)
B. Volledige code van alle 4 servers: Python, echte package-namen en versies 2026, type-hints, foutafhandeling, config via env-variabelen
C. docker-compose.yml voor de 4 servers op de AX102: intern netwerk (geen publieke poorten), healthchecks, resource-limits (de server deelt met ROM-builds en later Stay4S Cloud)
D. Beveiliging volgens het bestaande Factory-model: aparte MCP_READ_TOKEN en MCP_WRITE_TOKEN, permissie-tiers (read-only / write), audit-log van elke schrijfactie, allowlists (containers, tabellen), geen shell, geen publieke expositie
E. Client-configuraties: exacte config-JSON om de servers aan te sluiten op OpenCode (belangrijkste), Droid, en Codex CLI — copy-paste
F. .env.example met alle benodigde variabelen (database-URL, tokens, GitHub-token read-only)
G. Testplan: per server 3-5 tests + één end-to-end test: "OpenCode vraagt: wat zijn mijn open taken?" → stay4s-taken → correct antwoord
H. Documentatie: setup in 10 stappen, hoe een NIEUWE AI-tool in 5 minuten aansluit, uitbreidingsgids (hoe bouw je een 5e server)

## KWALITEITSEISEN
- Copy-paste ready, geen placeholders behalve duidelijk gemarkeerde secrets
- Nederlands in de uitleg, Engels in de code
- Eerlijk over beperkingen: wat kan een lokaal 7B/14B-model NIET via MCP-tools en hoe vang je dat op (fijnere tool-steps, validaties)
- Alles draait local-first: geen enkele call naar externe LLM-API's

Werk in branch ai/grok/mcp-werkplaats in Stay4S-Factory, commits met [grok]-prefix, open een PR voor review door Stay4Compa.
