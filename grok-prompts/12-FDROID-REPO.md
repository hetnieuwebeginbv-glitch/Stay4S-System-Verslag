# PROMPT 12: Stay4S App Store (F-Droid) Setup

Plak dit in Grok:

---

Je bent een senior Android distribution engineer die werkt voor Stay4S B.V. Configureer een F-Droid repository server als Stay4S App Store — onze eigen app store die Google Play vervangt.

CONTEXT:
- F-Droid repo server draait op Docker achter Traefik (store.stay4s.com)
- Domein: stay4s.com
- Server: Hetzner AX102, Ubuntu 24.04
- Dit vervangt Google Play Store
- Alleen open-source apps, Stay4S signed
- Stay4S apps die gedistribueerd worden: Stay4Safe AI, AetherCore, GuardianService UI, LocationGuard tile, Meshmatic, Command Center mobile
- App signing: Stay4S signing key (niet Google)
- Moet werken met Keycloak OIDC voor user accounts (optioneel, repo is publiek leesbaar)

GEEF ME:

1. F-Droid repo server docker-compose.yml
   - fdroidserver in Docker
   - Nginx voor static file serving
   - Traefik labels voor HTTPS
   - Volume voor repo data

2. fdroid/config.py
   - Repo name: Stay4S App Store
   - Repo URL: https://store.stay4s.com/repo
   - Signing key config (placeholder, wordt gegenereerd)
   - Archive URL: https://store.stay4s.com/archive
   - Update interval: daily
   - Minimum SDK: Android 14 (API 34)

3. Stay4S app build & publish pipeline
   - Script dat een APK bouwt en in de repo plaatst
   - fdroid update na het toevoegen van een nieuwe APK
   - fdroid deploy voor signing
   - Version management per app

4. Custom F-Droid client config
   - F-Droid client met Stay4S repo als default
   - Stay4S branding (donker thema, teal accent)
   - Eigen repo URL gepreconfigureerd
   - Anti-features: blokkeer apps met tracking, ads, non-free network services
   - Alleen Stay4S-signed apps tonen

5. GrokPhone ROM integratie
   - Hoe configureer je Stay4OS zodat de F-Droid client al geïnstalleerd is bij eerste boot
   - Stay4S repo als enige bron (geen F-Droid default repo)
   - System-privilege voor F-Droid (kan system apps installeren)
   - Auto-update policy voor Stay4S apps

6. Stay4S app signing
   - Hoe genereer je een Stay4S signing key
   - Hoe sign je APKs met deze key
   - Hoe configureer je F-Droid om deze key te vertrouwen
   - Key rotation policy

7. App metadata templates
   - Voor elke Stay4S app: fdroid metadata YAML
   - Stay4Safe AI, AetherCore, Guardian, LocationGuard, Meshmatic, Command Center

Output alles als copy-paste ready code blocks. Productieklaar.
