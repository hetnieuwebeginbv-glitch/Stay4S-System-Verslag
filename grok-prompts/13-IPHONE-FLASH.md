# PROMPT 13: iPhone 11 Pro Custom Firmware Flash (Stay4OS Demo)

Plak dit in Grok:

---

Je bent een senior iOS security researcher en jailbreak developer die werkt voor Stay4S B.V. Maak een volledige, stap-voor-stap executeerbare handleiding om custom firmware te flashen op een iPhone 11 Pro en die daarna te transformeren tot Stay4OS demo-toestel.

CONTEXT:
- Doeltoestel: iPhone 11 Pro (A13 Bionic, model identifier iPhone12,3) — spare/demo device, geen daily driver
- Doel: custom iOS 27.0 beta 2 firmware (build 24A5370h) flashen met kernel patches, daarna Stay4OS theming voor Kickstarter demo
- Exploit: usbliter8 BootROM exploit (Paradigm Shift) — tethered, via USB DFU mode, werkt op A12/A13
- Bronrepo: https://github.com/34306/usbliter8-fun (fork van wh1te4ever) — bevat werk-27.0b2/ map met make_cfw.py, tss_proxy_server.py, restore_cfw.sh, get_rd.py, boot_rd.sh, get_boot.py, boot.py, net_up.sh en patches/ map met userland_patches.py en disable_screentime.py
- De repo veronderstelt macOS als host. ONZE HOST IS EEN LENOVO IDEAPAD 5 MET LINUX (Debian). Adapteer ALLE stappen naar Linux: pymobiledevice3, pyimg4, usbmuxd, iproxy, sshpass zijn beschikbaar op Debian. img4tool en de tss proxy moeten op Linux draaien. Geef exacte apt/pip install commando's en wat je zelf moet bouwen vanaf source.
- Exploit-hardware: Raspberry Pi Pico 2 (RP2350). Wiring van gesloopte Lightning kabel: rood naar VBUS, zwart naar GND, wit (D-) naar G13, groen (D+) naar G12. De Pico voert de usbliter8 exploit uit en zet de iPhone in PWN DFU mode (LED knippert 2x tijdens exploit, blijft branden bij succes).
- Bekende CFW patches in de repo (offsets build-specifiek voor 24A5370h / iPhone12,3): kernel isDeviceInRestoreMode (USB Restricted Mode bypass), kernel sandbox file_check_mmap + mount patches (Allow /var/jb), kernel AMFIIsCDHashInTrustCache (trust everything), DeviceTree ephemeral-storage (99% progress fix), coreauthd NOP (anti SEP crash), ctkd patch (anti SEP crash), mobileactivationd should_hactivate + getActivationState (hacktivation), launchd disabled.plist (ScreenTimeAgent deadlock fix)
- SSHRD: SSH op ramdisk via iproxy 2222 22, wachtwoord "alpine", /sbin/mount_apfs -o rdonly /dev/disk1s6 /mnt6, sep-firmware.img4 extraheren met img4tool -e -m t8030_apticket.der
- Na normale boot: userland patches her-signen met ldid (-e entitlements, dan -S ents.plist -Cadhoc), staged apps verplaatsen naar /Applications/, uicache draaien, bootstrap (bootstrap_1900.tar.zst), Sileo installeren
- Internet op het toestel: alleen via USB (net_up.sh deelt host-internet)
- BEKEND GEBROKEN NA FLASH: WiFi, baseband (cellular), Bluetooth (partieel), SEP (Face ID, passcode, Apple Pay). Boot is TETHERED: elke herstart vereist opnieuw PWN DFU via de Pico rig + boot scripts. Dit is een demo-toestel.

SCHRIJF DE VOLGENDE LEVERANCIEREN:

1. HANDLEIDING FASE 0 — Voorbereiding op de IdeaPad 5 (Debian)
   - Exacte installatiecommando's voor alle dependencies (apt + pip3: pymobiledevice3, pyimg4, requests, usbmuxd, iproxy, sshpass)
   - img4tool op Linux: bouwen vanaf source of alternatief (git clone + build stappen, dependencies zoals libplist, libzip, openssl)
   - De repo clonen en de werk-27.0b2 map structuur verifiëren
   - iOS 27.0 beta 2 IPSW voor iPhone 11 Pro (iPhone12,3) downloaden en SHA verifiëren
   - Udev rules voor USB device access zonder sudo-problemen
   - Hoe je controleert dat Linux de iPhone in DFU mode ziet (verwachte lsusb output)

2. HANDLEIDING FASE 1 — Raspberry Pi Pico 2 rig
   - Welke usbliter8 source/binary je op de Pico 2 zet en waar je die haalt (Paradigm Shift publicatie / securerom.fun)
   - Exact hoe je de Pico 2 flasht (BOOTSEL / UF2 drag-drop procedure)
   - Wiring in tekst (rood-VBUS, zwart-GND, wit-G13, groen-G12) + veelgemaakte fouten en preventie
   - Hoe je de rig test ZONDER iPhone (Pico LED gedrag)

3. HANDLEIDING FASE 2 — PWN DFU en CFW restore
   - iPhone 11 Pro in DFU mode zetten (exacte knoppencombinatie en timing)
   - PWN DFU uitvoeren via de rig, verificatie via lsusb ("PWND:[usbliter8]" bij Apple Mobile Device DFU Mode)
   - make_cfw.py draaien op Linux (sudo, inputs, wat het script doet)
   - tss_proxy_server.py + restore_cfw.sh op Linux — fallback als Apple's TSS signing weigert
   - Wat je op het iPhone-scherm moet zien (progress bar, recovery na afloop)
   - Failsafe: als het bij 99% hangt (DeviceTree ephemeral-storage check)

4. HANDLEIDING FASE 3 — SSHRD en SEP extract
   - get_rd.py + boot_rd.sh op Linux
   - iproxy 2222 22 + sshpass -p alpine ssh -p 2222 root@localhost
   - mount_apfs commando's, sep-firmware.img4 terugkopiëren naar de IdeaPad
   - img4tool -e -m t8030_apticket.der dev_sep.img4 op Linux

5. HANDLEIDING FASE 4 — Normale boot + hacktivation
   - get_boot.py + boot.py
   - userland_patches.py voor mobileactivationd, coreauthd, ctkd
   - ldid re-signing procedure met entitlements backup (.orig)
   - disable_screentime.py + disabled.plist naar /var/db/com.apple.xpc.launchd/
   - Setup scherm passeren — debug checklist als Setup blijft hangen

6. HANDLEIDING FASE 5 — Internet via USB + bootstrap + Sileo
   - net_up.sh op Linux (usb0 interface, NAT, DNS)
   - bootstrap_1900.tar.zst installatie, symlink fixes, Sileo naar /Applications/, uicache
   - Staged apps verplaatsen als je maar 3 apps ziet

7. HANDLEIDING FASE 6 — Stay4OS theming (Kickstarter demo)
   - Welke Sileo tweaks/repo's voor: custom boot logo (Stay4OS hexagon + GROKPHONE tekst), volledige dark theme met teal accent (#00FF9D), custom lock screen, icon pack, Settings branding
   - Welke tweaks STABIEL zijn op iOS 27 CFW en welke te vermijden (geen SEP-afhankelijke tweaks)
   - Boot loop risico's: 1 tweak per keer, SSH verbinding open houden, SSHRD recovery procedure
   - Hoe je de tethered reboot procedure automatiseert (1 script op de IdeaPad: DFU-PWN + boot)

8. HERSTELPLAN
   - Bricked toestel: DFU restore met originele 27.0b2 IPSW via pymobiledevice3 (Linux alternatief voor Finder)
   - Op welk punt is er geen weg terug (SEP patch corruptie)
   - Backup strategie voor dev_sep.img4 en apticket

9. CHECKLIST DOCUMENT
   - Compacte 1-pagina checklist van alle stappen, per stap: verwacht resultaat + faaldetectie

Geef alle leverancieren als copy-paste ready code blocks en commando's, specifiek voor Debian Linux op een Lenovo IdeaPad 5. Gebruik echte paden, echte package namen, echte tool versies van 2026. Volledig executeerbaar, geen placeholders. Waar iets macOS-specifiek is in de originele repo, geef je de Linux-equivalente aanpak. Waar iets onbekend of onzeker is, zeg je dat expliciet in plaats van het te verzinnen.

---
