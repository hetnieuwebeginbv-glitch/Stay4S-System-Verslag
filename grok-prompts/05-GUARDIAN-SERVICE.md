# PROMPT 5: GuardianService Security Daemon

Plak dit in Grok:

---

Je bent een senior Android security engineer die werkt voor Stay4S B.V. Schrijf GuardianService — de security daemon voor Stay4OS.

CONTEXT:
- GuardianService is de centrale beveiligingslaag van Stay4OS
- Runt als system service op Pixel 9 Pro (Android 15 / API 35)
- Detecteert phishing, scams, deepfakes, malware, suspicious activity
- Integreert met AetherCore voor AI-analyse
- Integreert met LocationGuard voor location security
- Kan kill-switches triggeren bij dreiging
- Werkt volledig offline (alle AI via AetherCore op TPU)

SCHRIJF DE VOLGENDE BESTANDEN:

1. GuardianService.java
   - Extends android.app.Service, system service
   - Real-time monitoring van: network traffic (VPN-based), file access patterns, app behavior, SMS/call patterns
   - Threat detection engine met rules + AI hybrid:
     * Rules-based: bekende phishing patterns, malicious domains, suspicious permissions
     * AI-based: vraagt AetherCore "Analyseer deze activiteit: [context], is dit verdacht?"
   - Kill-switch trigger: kan GPS, camera, mic, radio, TPU uitschakelen bij dreiging
   - Alert system: stuurt alerts naar Command Center + AetherCore voor logging
   - Methods: scanActivity(context), checkUrl(url), checkFile(path), triggerKillSwitch(component), getThreatLevel(), getThreatHistory()
   - Threat levels: SAFE, ELEVATED, HIGH, CRITICAL, SEALED (alles dicht)

2. GuardianMonitor.java
   - VpnService implementatie voor traffic monitoring
   - Inspecteert DNS requests (phishing domain detection)
   - Inspecteert HTTP(S) patterns (known scam patterns)
   - Geen MITM (geen certificaat interceptie) — alleen metadata + DNS
   - Block list: kan domeinen blokkeren via local DNS redirect

3. GuardianDatabase.java
   - SQLite database voor threat log
   - Tables: threats (timestamp, type, severity, description, action_taken), blocklist (domain, added_by, reason, expires), allowlist (domain, reason)
   - Auto-cleanup na 30 dagen
   - Export functie (JSON) voor Command Center

4. guardian_service.rc
   - Init script, start bij boot, class main
   - SELinux context: u:r:guardian:s0

5. guardian_service.te
   - SELinux policy
   - Allow: VPN service, binder communication met AetherCore
   - Allow: write to /data/system/guardian/
   - Allow: trigger kill-switches via sysfs
   - Allow: read network state
   - Deny: directe internet toegang (alles via VPN tunnel)

6. GuardianQuickSettingsTile.java
   - Quick Settings tile voor het aan/uit zetten van Guardian
   - Toont huidig threat level als subtitle
   - Rood/oranje/groen indicator

Geef me alle bestanden als copy-paste ready code blocks. Android 15 (API 35), geen placeholders, volledig functioneel.
