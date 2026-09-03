# PROMPT 11: Stay4S Chat (Matrix/Synapse) Setup

Plak dit in Grok:

---

Je bent een senior messaging systems engineer die werkt voor Stay4S B.V. Configureer Matrix Synapse als Stay4S Chat — onze eigen berichtdienst die WhatsApp vervangt.

CONTEXT:
- Synapse (Matrix homeserver) draait op Docker achter Traefik (chat.stay4s.com)
- Domein: stay4s.com, Matrix ID: @user:stay4s.com
- Server: Hetzner AX102, Ubuntu 24.04
- Dit vervangt WhatsApp volledig
- Moet werken met Keycloak OIDC voor authenticatie
- End-to-end encrypted by default (Olm/Megolm)
- Element web client op chat.stay4s.com
- Ondersteuning voor voice/video (Matrix VoIP via LiveKit)
- Integratie met Meshmatic (off-grid berichten via LoRa mesh)

GEEF ME:

1. Synapse docker-compose.yml
   - Synapse server
   - PostgreSQL database (niet SQLite)
   - Element web client
   - Synapse-admin (beheer interface)
   - Traefik labels voor HTTPS

2. homeserver.yaml (Synapse config)
   - Server name: stay4s.com
   - Database: PostgreSQL
   - OIDC provider: Keycloak
   - E2E encryption enabled by default
   - Federation: uitgeschakeld (gesloten ecosysteem, alleen eigen server)
   - Registration: alleen via Keycloak (geen open registratie)
   - Rate limiting
   - File uploads/media storage
   - TURN server config voor VoIP

3. Element config.json
   - Default homeserver: stay4s.com
   - Branding: "Stay4S Chat"
   - Donker thema, teal accent
   - OIDC login via Keycloak
   - Custom welcome page

4. Keycloak OIDC config voor Synapse
   - Client: stay4s-chat
   - Scopes: openid, profile, email
   - User attribute mapping (Matrix username = Keycloak username)

5. Meshmatic bridge
   - Bridge config: Matrix ↔ MQTT (Meshmatic)
   - Als internet uitvalt, berichten gaan via LoRa mesh
   - Synapse bot die berichten van mesh ontvangt en doorgeeft
   - Config voor maubot of custom bridge

6. Migratie script
   - WhatsApp chat export → Matrix import
   - WhatsApp groepen → Matrix rooms
   - Media (foto's/Video's) importeren

7. Custom Element theme
   - Donker (#000000 bg, #00FF9D accent)
   - Stay4S branding
   - CSS voor custom login screen

Output alles als copy-paste ready code blocks. Productieklaar.
