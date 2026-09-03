# PROMPT 6: LocationGuard Pixel 9 Pro Migratie

Plak dit in Grok:

---

Je bent een senior Android ROM developer die werkt voor Stay4S B.V. Migreer de LocationGuard module van Nothing Phone 3a (codename "asteroids") naar Google Pixel 9 Pro (codename "tokay").

HUIDIGE SITUATIE:
- LocationGuard is gebouwd voor Nothing Phone 3a
- Werkt in 3 lagen: hardware (GPS via sysfs), OS (location daemon), netwerk (SUPL blocking via iptables)
- De Nothing Phone heeft een Glyph interface (LED dots) voor status
- Code staat op GitHub: hetnieuwebeginbv-glitch/Stay4S-LocationGuard

WAT VERANDERT VOOR PIXEL 9 PRO:
- Device tree: asteroids → tokay
- LineageOS 23.2 heeft officiële Pixel 9 support (geen community fork nodig)
- SELinux enforcing werkt direct (grootste probleem opgelost)
- Geen Glyph LED → vervangen door Pixel notification LED / always-on display indicator
- Andere sysfs paden voor GPS/Location hardware
- Tensor G4 chip heeft andere GPS module dan Nothing Phone

SCHRIJF DE MIGRATIE:

1. Migratie Document
   - Volledige diff van wat verandert van asteroids → tokay
   - Nieuwe sysfs paden voor Pixel 9 Pro GPS/radio
   - SELinux policy aanpassingen (wat mag weg, wat moet blijven)
   - Glyph → notification LED mapping
   - Tensor G4 specifieke hardware paths

2. LocationGuardService.java (gemigreerd)
   - Aangepaste sysfs paden voor Pixel 9 Pro
   - Glyph interface vervangen door NotificationManager-based indicator
   - Quick Settings tile blijft hetzelfde concept
   - Titan M2 security chip integratie (extra layer voor GPS tamper detection)

3. LocationGuardTile.java (gemigreerd)
   - Zelfde functionaliteit
   - Pixel-specifieke styling
   - Toont "SEALED" / "UNSEALED" status

4. location_guard.te (gemigreerde SELinux policy)
   - Aangepast voor LineageOS 23.2 Pixel 9 (tokay) policies
   - SELinux enforcing compatible
   - Nieuwe paden voor Tensor G4 hardware

5. location_guard.rc (init script)
   - Aangepast voor tokay device tree
   - Startvolgorde aangepast

6. Android.bp (build file)
   - Aangepast voor tokay build target

Geef me alle bestanden als copy-paste ready code blocks. Markeer duidelijk wat VERANDERT ten opzichte van de asteroids versie met "// CHANGED:" comments.
