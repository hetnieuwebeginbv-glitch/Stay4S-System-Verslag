# MEGA PROMPT 17 — REVIEW COMPLETE RUNTIME (Grok/ChatGPT) — 8 september 2026

## De prompt (copy-paste):

ROL: Je bent de "Eternal Software Boss" en mede-eigenaar van Stay4S. Vanuit die rol review je als senior AI-platform-architect de COMPLETE runtime die je mede-AI Stay4Compa heeft gebouwd in PR #1 van hetnieuwebeginbv-glitch/Stay4S-Factory (branch: ai/compa/complete-runtime). Dit is de eerste [compa]-PR die op jouw review wacht.

CONTEXT:
- Doel: een soortgelijk AI-hoofdkwartier, volledig soeverein op eigen infra (Hetzner AX102, RTX 4070, eigen Postgres + Qdrant), WhatsApp Cloud API zonder tussenpartij
- Trio-architectuur: StayLM-00 "Baas" (WhatsApp-intake + planning), "Linkerhand" (Bouwer/GitHub), "Rechterhand" (Denker/InfoVault)
- Meta Business-verificatie loopt; tot livegang max 5 testontvangers; code testbaar via POST /v1/opdrachten met INTERNAL_TOKEN
- Achtergrond: Stay4Compa's pariteitsdoel is dat dit systeem alles kan wat Base44 Stay4Compa kan (skills, automations, geheugen) maar dan op eigen hardware

WAT ER IN PR #1 ZIT (review dit allemaal):
1. runtime/whatsapp_gateway.py — FastAPI, HMAC-signatureverificatie (X-Hub-Signature-256), webhook-verify, afzender-whitelist, idempotentie (wa_id), mapping binnenkomende berichten naar opdrachten-wachtrij
2. runtime/hoofdagent.py — LangGraph-brein (plan -> actie -> synthese), router naar linkerhand/rechterhand/skills, tool-bindingen: github (read-only), postgres (eigen tabellen), bestanden (workspace), pr_review
3. Agent Factory — agent_register (Postgres), aparte FACTORY_TOKEN, template-allowlist, agent-cap 20, geen shell, GitHub read-only, HMAC, UUID-schema met CHECKs — volgens jouw beveiligingsmodel
4. NIEUW: hoofdagent/skills_runner.py — skillsysteem (skills/<naam>/SKILL.md frontmatter + scripts/run.py), subprocess met timeout 120s, poller voor !-commando's uit de opdrachten-wachtrij, audit-log per run
5. NIEUW: scheduler.py — nightly learning run 03:00 (dag samenvatten, user_facts bijwerken, infovault verrijken, patronen, groeirapport) + zondag 19:00 weekplanning/gap-analyse; beide via opdrachten-wachtrij (auditeerbaar)
6. NIEUW: docs/SEEKER_TEAM_BLUEPRINT.md — uitbreiding met 6 teamleden: notulist, web_seeker, github_seeker, phone_seeker, network_seeker, rapportmaker. ADHD-jachtprotocollen (parallel, breed, associatief, obsessief, niets kwijt) MAAR output-discipline (alleen bruikbare InfoVault-entries). Veiligheidsregels: read-only, eigen apparaten/netwerk alleen, audit-log per actie, geen persoonsgegevens naar buiten
7. Eerste 2 skills: vault-search (ILIKE over infovault-titel/body/tags) en status (agents + opdrachten-telling)
8. Schema migraties/001_init.sql: opdrachten (wa_id, raw_text, intent, plan_json, assigned_agent, status, result_text, error_text), user_facts, conversation_summaries, infovault (title, body, source, tags, importance), tool_audit_log, agent_register
9. docker-compose: postgres, gateway, hoofdagent, linkerhand, rechterhand, skills-runner, scheduler

REVIEWVRAGEN (beantwoord ALLEMAAL):
1. Beveiliging: gaten in gateway/HMAC/idempotentie/whitelist? Eén UUID-schema voor alles of scheiden per entiteit?
2. Schema: velden compleet voor productie? Welke indices/constraints ontbreken?
3. Skills runner: is subprocess met timeout veilig genoeg voor v0.1, of direct sandboxing (containers/rlimits)? Wat is je minimale eis?
4. Scheduler: naive CEST-offset vs zoneinfo — hoe hoog prioriteit? Race-conditions bij dubbele runs?
5. Hoofdagent: is plan->actie->synthese geschikt als brein, of mis je nodes (router, memory-check, escalatie)?
6. Zoekteam: accepteer je de 6 templates in de allowlist? Per template: welke extra beperkingen eis je (bijv. network_seeker alleen vanaf eigen infra)?
7. Wat ontbreekt NOG voor livegang op de AX102 (rate-limits, retry/redelivery, health checks, backups, secrets-beheer, resource-limits naast de ROM-builds)?

LEVER OP (in deze volgorde):
1. PER MODULE: akkoord / akkoord met wijzigingen / afkeuren + waarom
2. Eén genummerde lijst concrete wijzigingen, prioriteit blocker/major/nice
3. GO/NO-GO voor merge van PR #1 (met voorwaarden)
4. Deployment-checklist voor livegang op de AX102 (volgorde, env-vars, hardening, resource-limits naast ROM-builds, testplan met max 5 testontvangers)
5. Jouw oordeel over het Zoekteam-voorstel: allowlist-uitbreiding + beveiligingseisen per seeker
6. Zeg expliciet wat je NIET kunt beoordelen zonder de code zelf te zien — als je repo-toegang hebt: kijk zelf en zeg dat je dat deed

Wees kritisch en direct. Een merge met fouten kost ons meer dan een week vertraging. Dit wordt het hart van Stay4S — alles wat we daarna live zetten bouwt hierop voort.
