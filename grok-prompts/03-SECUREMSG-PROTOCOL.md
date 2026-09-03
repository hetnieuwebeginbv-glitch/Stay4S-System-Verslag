# PROMPT 3: GrokSecureMsg Post-Quantum Protocol

Plak dit in Grok:

---

Je bent een senior cryptografie engineer die werkt voor Stay4S B.V. Ontwerp het GrokSecureMsg protocol — een post-quantum mesh berichtprotocol voor de GrokPhone.

CONTEXT:
- GrokPhone draait Stay4OS (custom Android ROM, Pixel 9 Pro)
- Berichten gaan via Meshmatic LoRa mesh OF WiFi/Bluetooth mesh
- Moet werken off-grid (geen internet)
- Moet post-quantum veilig zijn (we gaan ervan uit dat quantum computers bestaan)
- Geoptimaliseerd voor lage bandbreedte (LoRa: max 51 bytes per packet in EU868)

ONTWERP DEZE COMPONENTEN:

1. KEY EXCHANGE
   - Gebruik Kyber-768 (NIST PQC standard) voor key agreement
   - Hoe werken de round trips over een low-bandwidth mesh?
   - Hoe cache je keys per node pair?
   - Hoe roteer je keys?

2. SIGNATURES
   - Gebruik Dilithium-3 (NIST PQC standard) voor message authentication
   - Hoe verifieer je signatures in een mesh waar nodes elkaar niet direct kennen?
   - Web-of-trust model voor mesh nodes?

3. MESSAGE FORMAT
   - Definieer het binary message format (byte-by-byte)
   - Header: version, type, sender_id, receiver_id, timestamp, msg_id, flags
   - Body: encrypted payload
   - Trailer: signature
   - Hoe pas je dit in 51 bytes per LoRa packet? (fragmentation protocol)

4. FRAGMENTATION
   - Hoe splits je een bericht over meerdere LoRa packets?
   - Reassembly protocol
   - Retry en acknowledgment mechanisme
   - Hoe ga je om met out-of-order packets?

5. ANDROID SERVICE ARCHITECTUUR
   - Schrijf de Java class outline voor GrokSecureMsgService
   - Extends Android Service
   - AetherBridge integration
   - Methods: sendMessage(), receiveMessage(), verifyNode(), rotateKeys()
   - Hoe integreer je met MeshmaticService voor transport?

Geef me:
1. Volledig protocol spec (markdown)
2. Binary message format tabel
3. Fragmentation protocol diagram (ASCII art)
4. Java class outline voor GrokSecureMsgService.java
5. Een voorbeeld message exchange (Alice → Bob, step by step)

Alles als copy-paste ready code blocks in markdown.
