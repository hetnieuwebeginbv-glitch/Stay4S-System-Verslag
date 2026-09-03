# PROMPT 8: Stay4S Identity (Keycloak) Setup

Plak dit in Grok:

---

Je bent een senior identity & access management engineer die werkt voor Stay4S B.V. Configureer Keycloak als Stay4S Identity — de centrale SSO voor alle 12 Stay4S diensten.

CONTEXT:
- Keycloak draait op Docker achter Traefik (id.stay4s.com)
- Alle Stay4S diensten authenticeren via Keycloak (OIDC)
- Dit is de sleuteldienst — alle andere diensten hangen hiervan af
- Moet 2FA/TOTP ondersteunen
- Moet werken als identity provider voor: Mail (Mailu), Chat (Matrix), Cloud (Nextcloud), App Store, VPN, etc.

GEEF ME:

1. Keycloak realm config (JSON)
   - Realm: stay4s
   - 2FA verplicht voor admin, optioneel voor users
   - Password policy: min 12 chars, uppercase, lowercase, numbers, special
   - Session timeout: 8 hours
   - Brute force protection: 5 attempts → 15 min lockout

2. Client configurations
   - Voor elke Stay4S dienst een Keycloak client:
     * stay4s-mail (Mailu), stay4s-chat (Matrix), stay4s-cloud (Nextcloud)
     * stay4s-ai (Ollama proxy), stay4s-store (F-Droid), stay4s-vpn (WireGuard)
     * stay4s-search, stay4s-maps, stay4s-wallet, stay4s-mesh
     * stay4s-commandcenter (Base44 Command Center)
   - Elke client: protocol OIDC, redirect URIs, client auth
   - Geef me als JSON realm import

3. Mailu OIDC integration
   - Hoe configureer je Mailu om Keycloak te gebruiken voor authenticatie
   - config.yml aanpassingen

4. Nextcloud OIDC integration
   - Nextcloud Social Login plugin config
   - Keycloak als OIDC provider

5. Matrix Synapse OIDC integration
   - Synapse homeserver.yaml OIDC config
   - Keycloak als identity provider voor Matrix

6. Custom login theme
   - Stay4S branding op Keycloak login page
   - Donker thema, teal accent (#00FF9D)
   - HTML/CSS voor custom login page

Output alles als copy-paste ready code blocks. Volledig geconfigureerd, klaar om te importeren.
