# PROMPT 4: AetherCore Android Service

Plak dit in Grok:

---

Je bent een senior Android system developer die werkt voor Stay4S B.V. Schrijf de AetherCoreService — de centrale AI service voor Stay4OS op de Pixel 9 Pro.

CONTEXT:
- Pixel 9 Pro heeft een Tensor G4 chip met TPU
- AetherCore draait lokale AI modellen op de TPU via Ollama/llama.cpp
- De service draait als system service (niet user app)
- Integreert met AetherBridge middleware
- Moet werken zonder internet (volledig offline AI)

SCHRIJF DE VOLGENDE BESTANDEN:

1. AetherCoreService.java
   - Extends android.app.Service
   - Runt als foreground service met persistent notification
   - Start Ollama daemon als subprocess
   - Laadt model bij boot (configurabel: llama3.2:3b, qwen2.5:3b, of custom StayLM)
   - Methods: query(prompt), queryWithImage(prompt, image), getStatus(), loadModel(name), unloadModel()
   - AetherBridge API: bindt aan com.stay4s.aether.IAetherBridge
   - Model cache management (auto-cleanup bij low memory)
   - TPU acceleration detection en fallback naar CPU

2. IAetherBridge.aidl
   - AIDL interface voor AetherBridge
   - Methods: query(String prompt, String callback), queryWithImage(String prompt, byte[] image, String callback), getStatus(), loadModel(String name), getLoadedModel(), unloadModel()
   - Callback mechanisme voor async responses

3. AetherBridgeReceiver.java
   - BroadcastReceiver die queries van andere OS services ontvangt
   - GuardianService kan bijvoorbeeld vragen: "Is dit verdachte activiteit?" met context
   - VaultService kan vragen: "Versleutel dit bestand met welk algoritme?"
   - MeshmaticService kan vragen: "Bepaal optimale route voor dit bericht"

4. aether_core.rc
   - Android init script
   - Start service bij boot (class main)
   - Restart on crash
   - SELinux context: u:r:aether_core:s0

5. aether_core.te
   - SELinux policy
   - Allow: exec ollama binary, access /data/local/tmp/ollama, bind to localhost:11434
   - Allow: communicate with binder (AetherBridge)
   - Deny: network access (offline only, tenzij expliciet enabled)
   - Allow: access model files in /data/system/aether/models/

Geef me alle 5 bestanden als copy-paste ready code blocks. Gebruik echte Android 15 (API 35) APIs. Volledig functioneel, geen placeholders.
