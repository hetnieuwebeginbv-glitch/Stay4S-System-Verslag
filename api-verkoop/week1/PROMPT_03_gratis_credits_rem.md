# PROMPT 03 — Rem op de gratis credits

**Doel:** elke nieuwe account krijgt nu automatisch 1.000 credits (€10) zonder enige verificatie.
Met een script is dat gratis rekenkracht: één bot maakt honderd accounts en heeft €1.000 aan
inference gratis. Dit moet dicht voordat er betaald verkeer is.

## Bewijs uit de huidige code

- `api_keys_ext.py:347-392` — bij de eerste login wordt een account aangemaakt met
  `INSERT INTO ... VALUES (?, 'free', 1000, ?, ?)` en `credits_balance: 1000`.
- `billing_ext.py:507-511` — dezelfde toekenning in de billing-module.
- `billing_ext.py:43` — de free tier is gedefinieerd als 1.000 credits = €10 waarde.
- `api_keys_ext.py:29` — `MAX_REQUESTS_PER_KEY_PER_DAY = 10000`, maar niets begrenst het aantal
  accounts of het aantal sleutels per persoon (max 5 per account, dus 5 × 1.000 credits mogelijk).

## Opdracht

1. **Credits pas na e-mailverificatie.** Bij registratie: account aanmaken met saldo 0, een
   verificatiemail sturen met een token (24 uur geldig), en pas na klikken de 1.000 credits
   toekennen. Zonder verificatie blijft het saldo 0 en kan er geen sleutel worden aangemaakt.
2. **Signup-throttle.** Maximaal 3 registraties per IP-adres per 24 uur. Blokkeer wegwerp-domeinen
   (lijst in de repo, minimaal mailinator, guerrillamail, 10minutemail, tempmail, throwaway).
   Zet een eenvoudige captcha of snelheidscontrole op het registratieformulier.
3. **Misbruikdetectie als vangnet.** Dagelijkse controle op `api_usage`:
   - gratis account, geen betaling, én meer dan 20.000 requests per dag of meer dan 300.000
     requests in totaal → automatisch schorsen en een mail naar Mitchell
   - meer dan 5 accounts vanaf hetzelfde IP → signaleren in de mail
4. **Zichtbaarheid in `/v1/me`.** Toon de herkomst van het saldo (`gratis` vs `betaald`) en de
   verificatiestatus, zodat support en Mitchell kunnen zien wat er speelt.
5. **Beslispunt, niet zelf invullen:** de hoogte van de gratis tier. Twee opties:
   a. 1.000 credits houden maar alleen na verificatie (ruim, veel potentiële misbruikers)
   b. 250 credits (€2,50) na verificatie en de rest op aanvraag

   Bouw optie (a) als standaard, zet de hoogte in één constante zodat Mitchell hem kan omzetten.

## Acceptatiecriteria

- Een nieuwe registratie zonder e-mailverificatie kan geen sleutel aanmaken en heeft saldo 0.
- Na verificatie verschijnt 1.000 credits en werkt de sleutel direct.
- De vierde registratie vanaf hetzelfde IP binnen 24 uur wordt geweigerd met een nette melding.
- Een registratie met een wegwerp-domein wordt geweigerd.
- De dagelijkse detectie draait zonder fouten op een lege database (geen crash bij nul resultaten).

## Tests die je meelevert

1. Registratie → direct een sleutel proberen → verwacht: geweigerd, saldo 0.
2. Verificatielink klikken → 1.000 credits, sleutel werkt.
3. Vier registraties vanaf één IP → de vierde wordt geweigerd.
4. Registratie met een wegwerp-adres → geweigerd.
5. Detectie-script op een lege database → geen fout.
