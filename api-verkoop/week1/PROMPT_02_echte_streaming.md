# PROMPT 02 — Echte SSE-streaming in plaats van één blok

**Doel:** de API belooft OpenAI-compatibel te zijn, maar levert bij `"stream": true` één keer de
volledige tekst als losse SSE-chunk. Developers haken daarop af nog voor ze betalen: zonder
tokenstream voelt de API traag en werkt hij niet met de meeste SDK's en chatinterfaces.

## Bewijs uit de huidige code

- `api_keys_ext.py:851-857` — de `stream`-tak stuurt één `data:`-blok met de complete
  `result["content"]` en daarna `[DONE]`.
- `api_keys_ext.py:798` — de provider wordt altijd met `stream=False` aangeroepen, ook als de
  klant om streaming vraagt.
- `model_router.py:136` — `route_chat_sync` heeft een `stream`-parameter, maar `_route_openai`
  verwerkt het antwoord als geheel.
- `api_keys_ext.py:307` — docs zeggen eerlijk: "Streaming wordt als één blok geleverd, niet als tokenstream."

## Opdracht

1. **Nieuwe functie `route_chat_stream()` in `model_router.py`** die de SSE-stream van de provider
   doorgeeft (OpenAI-compatibel: `data: {...}`-regels tot `data: [DONE]`). Anthropic-route idem met
   zijn eigen event-formaat, omgezet naar OpenAI-chunks.
2. **Chunk-formaat (OpenAI-compatibel):**
   - eerste chunk: `choices[0].delta = {"role": "assistant"}`
   - daarna per stukje: `choices[0].delta = {"content": "<stukje>"}`
   - laatste chunk: `choices[0].finish_reason = "stop"` en, als de klant
     `stream_options: {"include_usage": true}` meestuurt, een `usage`-object
   - afsluiten met `data: [DONE]`
   - elke chunk heeft `id` (`chatcmpl-…`), `object: "chat.completion.chunk"`, `created` en `model`
3. **Credits bij streaming:**
   - reserveren vóór de eerste byte (bestaande schatting, harde bovengrens)
   - afrekenen ná de stream op basis van `usage` van de provider; ontbreekt die, tel dan de
     verzonden delta's met de tokenizer uit PROMPT 01
   - **klant verbreekt de verbinding:** reken af op wat de provider rapporteerde, of op de reeds
     gegenereerde delta's. Nooit de volledige reservering laten staan als er maar 20 tokens zijn
     geproduceerd.
   - **fout nadat de stream is gestart:** stuur `data: {"error": {"message": ..., "type": ...}}`,
     sluit netjes af en boek de reservering terug. Nooit halverwege een 500 teruggeven.
4. **Headers:** `Content-Type: text/event-stream`, `Cache-Control: no-cache`, `X-Accel-Buffering: no`
   (anders houdt een proxy de stream tegen).
5. **Docs bijwerken:** haal de streaming-beperking weg en zet een `curl -N` voorbeeld in de docs.

## Acceptatiecriteria

- `curl -N` met `"stream": true` levert meer dan één content-chunk, met een eerste chunk binnen
  2 seconden bij een 3B-model.
- De som van alle delta's is exact gelijk aan wat een niet-streamende call zou teruggeven.
- `usage` in de laatste chunk is gevuld bij `include_usage: true` en klopt met `api_usage`.
- Verbinding verbreken na circa 1 seconde levert geen volledige afboeking op.
- Een provider-fout halverwege levert een nette `error`-chunk en een volledige terugboeking.

## Tests die je meelevert

1. Streaming-call met 200 tokens → aantal chunks loggen en de time-to-first-token meten.
2. Idem met `include_usage: true` → usage vergelijken met `api_usage`.
3. Client-disconnect → saldo controleren.
4. Provider weigert (ongeldige sleutel simuleren) → error-chunk en terugboeking controleren.
