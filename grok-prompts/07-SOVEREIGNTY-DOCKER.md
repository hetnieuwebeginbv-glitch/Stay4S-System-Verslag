# PROMPT 7: Sovereignty Docker Stack (alle 12 diensten)

Plak dit in Grok:

---

Je bent een senior DevOps engineer die werkt voor Stay4S B.V. Maak een complete Docker Compose setup voor alle 12 Stay4S soevereiniteitsdiensten op een enkele Hetzner AX102 server (AMD Ryzen, 64GB RAM, 1TB NVMe, Ubuntu 24.04).

DIENSTEN EN HUN OPEN-SOURCE BASIS:
1. Stay4S Mail → Mailu (mail server met webmail, IMAP, SMTP, antispam)
2. Stay4S Chat → Matrix Synapse (server) + Element (web client)
3. Stay4S AI → Ollama (local LLM server, models: llama3.2:3b, qwen2.5:3b)
4. Stay4S Identity → Keycloak (SSO, OAuth2, OIDC)
5. Stay4S App Store → F-Droid repository server
6. Stay4S Search → SearXNG
7. Stay4S Maps → Tile server (tilemaker + tileserver-gl) met OpenStreetMap data
8. Stay4S Cloud → Nextcloud + MinIO (S3 storage backend)
9. Stay4S Wallet → BTCPay Server
10. Stay4S VPN → WireGuard (wg-easy)
11. Stay4S DNS → Pi-hole + Unbound (DNS resolver + ad blocking)
12. Stay4S Mesh → Mosquitto MQTT broker (voor Meshmatic bridge)

EISEN:
- Eén docker-compose.yml bestand voor alles (of opgesplitst in 3 compose files: core, services, edge)
- Reverse proxy: Traefik met automatic SSL (Let's Encrypt)
- Domein: stay4s.com (met subdomains: mail., chat., ai., id., store., search., maps., cloud., wallet., vpn., dns., mesh.)
- Alles draait achter Traefik, interne communicatie via Docker network
- Persistent volumes voor alle data
- Environment variables in .env bestand (geen hardcoded secrets)
- Health checks voor elke service
- Auto-restart policies
- Resource limits per container (geen enkele container mag de hele server claimen)
- Backup strategie: daily volume snapshots naar /backup/

GEEF ME:
1. .env.example (alle environment variables met veilige defaults)
2. docker-compose.yml (volledig, alle 12 services + Traefik)
3. traefik/dynamic.yml (reverse proxy config met SSL en subdomain routing)
4. Een installatie script (install.sh) dat:
   - Docker + Docker Compose installeert
   - Directory structuur aanmaakt
   - .env invult (vraagt alleen domein + admin email)
   - Traefik SSL certificates haalt
   - Alle services start in juiste volgorde
5. Een backup script (backup.sh) dat alle volumes backuped

Output alles als copy-paste ready code blocks. Volledig productieklaar, geen placeholders.
