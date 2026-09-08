# ONTWERP 2026-09-08 — OmniRoute + StayLM: local-first cascade

Auteur: Stay4Compa [compa]
Status: ter review door Mitchell
Kernprincipe: de eigen LLM eerst, OmniRoute als schakel en vangnet — nooit andersom. Soevereiniteit blijft leidend.

## 1. Eén endpoint voor alles
Alle Stay4S-tools (agents, coding tools, de hoofdagent uit de Factory) praten met ÉÉN OpenAI-compatible endpoint:
OPENAI_BASE_URL = http://omniroute:3000/v1
OmniRoute draait als Docker-service op de AX102, zelfde netwerk als vLLM (stay4s-factory compose). Poort alleen intern/localhost binden.

## 2. Onze eigen vLLM als provider nr. 1
OmniRoute ondersteunt custom providers: registreer vLLM als provider "stay4s-local":
- base_url: http://vllm:8000/v1 (OpenAI-compatible)
- api_key: niet nodig (intern)
- priority: 1 (hoogste), kosten: 0, regio: EU/eigen
Modellen: Qwen2.5-7B/14B AWQ op AX102 (12GB). Zodra de 2x RTX 5090-rig thuis draait (64GB, Qwen2.5-72B AWQ): thuis-rig wordt primary, AX102 wordt dev/fallback.

## 3. Routeringsregels (cascade)
1. Standaard-verzoeken → stay4s-local, altijd
2. Context > 8192 tokens of GPU vol of lokaal model ongeschikt → externe provider via OmniRoute — MAAR alleen verkeersklasse DEV
3. Vision (Qwen2.5-VL) → lokaal, niet naar extern
4. Verkeersklasse PROD (klantdata, productie) → ALTIJD stay4s-local, externe fallback hard uitgezet in config

## 4. Twee API-keys = twee verkeersklassen
- DEV-key: mag naar externe gratis providers (geen secrets, geen klantdata)
- PROD-key: local-only, routeert nooit naar buiten
De hoofdagent en agents krijgen PROD; coding tools en experimenten krijgen DEV. Dit sluit aan op de permissie-tiers uit Prompt 14 Addendum-2 (read-only / intern / extern-actief).

## 5. Logging = leerdata
Elke routing-beslissing loggen (welke provider, waarom, latency, kosten). Dezelfde filosofie als de eSIM RoutingLog: dit wordt de dataset waarmee StayLM zelf slimmer leert kiezen. Later kan de keuze volledig AI-gestuurd, nu eerst regel-engine.

## 6. Toekomst: multi-tenant Stay4S Agent
Zelfde patroon herbruikbaar per abonnee: privacy-abonnees → local tier (garantie), frontier-optie → externe providers alleen als klant expliciet kiest (Addendum 6H "fallback naar groter model achter betaalde API als klant dat kiest"). OmniRoute maakt die keuze per tenant configureerbaar.

## 7. Bouwvolgorde
1. Security review OmniRoute (community CVE-kritiek) — blijft verzoek
2. omniroute-service toevoegen aan stay4s-factory compose (localhost-bind, healthcheck)
3. stay4s-local registreren als provider, priority-cascade + DEV/PROD keys configureren
4. Cutover: OPENAI_BASE_URL van vllm → omniroute (één regel, reversibel)
5. Weekrapport routing-statistieken (hoeveel lokaal vs. extern)

## 8. Eerlijke risico's
- OmniRoute is een jong, snelgroeiend project: pin een versie, neem geen :latest
- Gratis externe providers = data buiten EU: DEV-only, hard coderen in de PROD-config
- Extra hop = paar ms latency: verwaarloosbaar t.o.v. model-inferentie
