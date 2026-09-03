# PROMPT 10: Stay4S Mail (Mailu) Setup

Plak dit in Grok:

---

Je bent een senior mail systems engineer die werkt voor Stay4S B.V. Configureer Mailu als Stay4S Mail — onze eigen mailserver die Gmail vervangt.

CONTEXT:
- Mailu draait op Docker achter Traefik (mail.stay4s.com)
- Domein: stay4s.com
- Server: Hetzner AX102, Ubuntu 24.04
- Dit vervangt Mitchell's Gmail volledig
- Moet werken met Keycloak OIDC voor authenticatie
- DKIM, SPF, DMARC allemaal correct geconfigureerd
- Anti-spam: Rspamd
- Webmail: Roundcube (donker thema, Stay4S branding)

GEEF ME:

1. Mailu docker-compose.yml
   - Alle Mailu services: front (Nginx), imap, smtp, antispam (Rspamd), webmail (Roundcube), admin, webdav
   - Volumes, networks, environment variables
   - Geïntegreerd met Traefik voor SSL

2. mailu.env
   - Alle Mailu configuratie variabelen
   - Domein, hostname, admin credentials
   - Secret key (placeholder, wordt gegenereerd)
   - Enable: antispam, antivirus, webmail, webdav

3. DNS records
   - MX record voor stay4s.com
   - SPF (TXT)
   - DKIM (TXT — Mailu genereert key)
   - DMARC (TXT)
   - PTR (reverse DNS — via Hetzner console)
   - Autodiscover/autodiscover voor automatische mail client setup

4. Traefik labels voor Mailu
   - HTTPS routing voor webmail
   - SMTPS routing (port 465)
   - IMAPS routing (port 993)

5. Roundcube custom theme
   - Donker thema, teal accent (#00FF9D)
   - Stay4S branding in login page
   - Stay4S logo placeholder

6. Keycloak OIDC integration config
   - Mailu OIDC config voor Keycloak
   - User provisioning via Keycloak

7. Migratie script
   - Hoe verhuis je Gmail emails naar Stay4S Mail
   - IMAP sync met imapsync
   - Contacten importeren
   - Calendar importeren

8. First-run checklist
   - DNS propagation check
   - SMTP test (send test email)
   - DKIM verification
   - SPF verification
   - DMARC verification
   - Spam score test (mail-tester.com)

Output alles als copy-paste ready code blocks. Productieklaar.
