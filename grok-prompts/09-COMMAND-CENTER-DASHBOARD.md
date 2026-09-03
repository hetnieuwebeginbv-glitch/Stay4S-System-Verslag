# PROMPT 9: Stay4S Command Center Dashboard

Plak dit in Grok:

---

Je bent een senior frontend developer die werkt voor Stay4S B.V. Ontwerp en bouw de Stay4S Command Center dashboard specs — het centrale beheerpaneel voor het hele ecosysteem.

CONTEXT:
- Command Center is een web app (Next.js 15 / React) die alle Stay4S diensten beheert
- 8 dashboard pagina's: Dashboard, InfoVault, AI Hub, Netwerk Monitor, Agent Team, Klantbeheer, ROM Dev, Settings
- Donker thema, teal accent (#00FF9D), responsive, sidebar navigatie
- Data komt uit Base44 entities via REST API (entity CRUD)
- Real-time updates waar mogelijk (WebSocket / polling)

GEEF ME VOOR ELKE PAGINA:

1. DASHBOARD (overview)
   - System health cards: 12 services status (groen/oranje/rood)
   - Threat level indicator (SAFE/ELEVATED/HIGH/CRITICAL/SEALED)
   - InfoVault stats: totaal records, open acties, critical items
   - Network status: online nodes, degraded nodes, mesh messages
   - GitHub: open PRs, issues, latest commits
   - Revenue: active subscriptions, MRR, early adopters
   - Recent agent activity (laatste 5 acties uit AgentLog)

2. INFOVAULT
   - Tabel met: titel, samenvatting, bron, type, tags, belangrijkheid, actie_status
   - Filters: type, bron, belangrijkheid, actie_status, tags
   - Search bar
   - Detail view met ruwe_inhoud
   - Actie knop: markeer als verwerkt, verander actie_status

3. AI HUB
   - Model selector (uit AIModel entity)
   - Chat interface (messages naar AIConversation entity)
   - Conversatie geschiedenis sidebar
   - AetherCore status indicator (lokaal model op phone? welke model geladen?)
   - Token usage display

4. NETWERK MONITOR
   - NetworkNode lijst met status indicators
   - Signal strength bars
   - MeshMessage log (laatste 20 berichten)
   - RouteLog (AI routing beslissingen)
   - NodeAlarm sectie met severity indicators
   - EmergencyAlert panel
   - Geofence map (als er geo data is)

5. AGENT TEAM
   - 7 agent kaarten met avatar, naam, rol, status, level
   - Opdracht lijst met prioriteit kleuren
   - Task board (kanban: Todo / In Progress / Done / Blocked)
   - AgentLog (recente activiteit per agent)

6. KLANTBEEHEER
   - Klant tabel met abonnement status
   - Abonnement overzicht (MRR berekening)
   - EarlyAdopter lijst met badges
   - Referral overzicht (verdiende maanden)
   - Safe4AIAanvraag queue

7. ROM DEV
   - GithubTracker tabel: repos, open PRs, issues, commits
   - FirmwareRelease lijst met versies en status
   - BatteryLog (voor edge nodes)
   - Build status indicators

8. SETTINGS
   - SovereigntyService tabel: alle 12 diensten met install_status
   - AIProviderConfig tabel
   - Connectors overzicht (OAuth status)
   - Gebruikers beheer

GEEF ME:
1. Volledige pagina component structuur (React/Next.js)
2. Voor elke pagina: de layout met cards/tables/charts beschreven in JSX-achtige pseudocode
3. Sidebar navigatie component
4. Dark theme CSS ( Stay4S branding)
5. API endpoint mapping (welke entity → welke pagina)

Output als copy-paste ready code blocks in markdown. Volledig, geen placeholders.
