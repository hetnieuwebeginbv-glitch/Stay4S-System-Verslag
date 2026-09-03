# PROMPT 2: Stay4OS Boot Animatie

Plak dit in Grok:

---

Je bent een senior Android ROM developer die werkt voor Stay4S B.V. Bouw een boot animatie voor Stay4OS — de custom ROM voor GrokPhone (Pixel 9 Pro, codename "tokay").

CREËER TWEE DINGEN:
1. Een boot animation spec in JSON format (Android bootanimation format)
2. De desc.txt configuratie

BOOT ANIMATION CONCEPT:
- Fase 1 (0-2s): Zwart scherm, fade-in van een teal (#00FF9D) hexagon outline in het centrum
- Fase 2 (2-4s): Hexagon vult zich met een glow effect, "Stay4OS" tekst verschijnt eronder (wit, sans-serif, clean)
- Fase 3 (4-5s): Onder het logo verschijnen 7 kleine dots — 5 lichten op in teal (SEALED indicators), 2 blijven donker
- Fase 4 (5-6s): "5/7 SEALED" tekst verschijnt onder de dots (mono, teal)
- Fase 5 (6-7s): Alles fade naar zwart, alleen "● STAY4S" blijft in rechteronderhoek (mono, teal, klein)
- Loop: 1x, dan zwart tot boot compleet

TECHNISCHE EISEN:
- Resolutie: 1080x2400 (Pixel 9 Pro)
- FPS: 30
- Format: PNG frames in folder structuur + desc.txt
- Kleuren: #000000 bg, #00FF9D accent, #FFFFFF text
- Duur: ~7 seconden tot boot compleet

Geef me:
1. De desc.txt inhoud
2. Een lijst van alle PNG frames die nodig zijn (met beschrijving van elk frame)
3. Een Python script dat de frames kan genereren met Pillow (PIL)
4. De folder structuur voor de bootanimation.zip

Output alles als copy-paste ready code blocks.
