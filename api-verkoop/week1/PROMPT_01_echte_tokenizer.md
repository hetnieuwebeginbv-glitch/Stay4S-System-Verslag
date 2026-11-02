# PROMPT 01 — Echte token-telling in plaats van tekens

**Doel:** de Stay4S API factureert nu tekens in plaats van tokens. Daardoor betaalt een klant
voor qwen2.5-7b effectief €6,00 per miljoen input-tokens en €12,00 per miljoen output-tokens,
terwijl de markt voor een 7B-model rond €0,15 / €0,30 ligt. Dit moet kloppen voordat er ook maar
één betalende klant is.

## Bewijs uit de huidige code (repo stay4s-sitebouw)

- `api_keys_ext.py:781` — `estimated_tokens_in = len(str(messages))`
- `api_keys_ext.py:782` — `estimated_tokens_out = max_tok * 6`
- `api_keys_ext.py:818` — `actual_tokens_in = len(str(messages))`
- `api_keys_ext.py:819` — `actual_tokens_out = len(result.get("content", ""))`
- `api_keys_ext.py:307` — docs vermelden zelf: "Tokenaantallen zijn een benadering op basis van tekens; facturatie gebruikt dezelfde maat."

Let op: `len(str(messages))` telt ook de JSON-structuur mee (quotes, accolades, veldnamen), dus de
input wordt dubbel overschat.

## Opdracht

1. **Gebruik de provider-usage als die beschikbaar is.** OpenAI-compatibele providers (OpenAI, Groq,
   DeepSeek, Mistral, OpenRouter, LiteLLM, vLLM) geven `usage.prompt_tokens` en
   `usage.completion_tokens` terug. Anthropic geeft `usage.input_tokens` / `usage.output_tokens`.
   Die waarden zijn de bron van waarheid voor de afrekening.
2. **Fallback: lokale tokenizer per model.** Voeg aan elk model in `model_registry.py` twee velden toe:
   - `tokenizer_id` (HF-modelnaam of lokaal pad)
   - `chat_template: true/false`

   Tel de prompt als volgt: `ids = tokenizer.apply_chat_template(messages, add_generation_prompt=True); len(ids)`
   en de output als `len(tokenizer.encode(content))`. Cache tokenizers in het geheugen (lazy load),
   zodat de eerste call per model wat langzamer is en daarna niet meer.
3. **Reservering blijft een harde bovengrens**, maar de afrekening gaat op echte tokens. De bestaande
   terugboeklogica (`api_keys_ext.py:826-835`) blijft ongewijzigd van opzet; alleen de invoer wordt echt.
4. **Herleidbaarheid:** voeg aan `api_usage` een kolom `token_bron` toe met `'provider'` of `'tokenizer'`.
   Bestaande rijen niet aanpassen (historische tekentelling blijft staan).
5. **Nieuw endpoint voor klanten:** `POST /api/v1/tokenize` met body `{model, messages}` →
   `{prompt_tokens, estimated_output_tokens, estimated_cost_eur}`. Developers verwachten dit om
   kosten te ramen; het kost geen modelrekenkracht.
6. **Docs bijwerken:** haal de regel over de tekens-benadering uit `api_keys_ext.py:307` en zet er
   "tokenaantallen volgen de tokenizer van het model" neer.
7. **Prijzen NIET aanpassen in deze stap.** Lever wel een tabelletje op met de effectieve prijs per
   miljoen échte tokens vóór en ná deze wijziging, per model. De prijsbeslissing is apart.

## Acceptatiecriteria

- Een call met een Nederlandse zin van circa 1.000 woorden geeft een prompt_token-aantal dat minder
  dan 2% afwijkt van de provider-usage.
- `usage.prompt_tokens + usage.completion_tokens` in het antwoord is gelijk aan wat er in `api_usage`
  wordt opgeslagen.
- Bij een provider-fout of een fout in de afwerking gaat de reservering volledig terug (bestaand
  gedrag, controleer dat het nog werkt).
- Saldo kan nooit negatief worden.
- `/api/v1/models` blijft werken en `/api/v1/tokenize` geeft een plausibele raming.

## Tests die je meelevert

1. Korte Nederlandse prompt → provider-usage vs tokenizer-telling vergelijken (uitkomst loggen).
2. Code/JSON-prompt met veel leestekens → idem.
3. Prompt met emoji → idem.
4. Lege `messages` → HTTP 400 met duidelijke foutmelding.
5. `max_tokens` extreem hoog → reservering blijft binnen de limiet en de afrekening klopt achteraf.
