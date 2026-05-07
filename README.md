# Chrome Prompt API Demo — Draw & Describe

A tiny single-file demo (`index.html`) that exercises Chrome's built-in **Prompt API** with **multimodal (image) input**. Draw anything on the canvas — or load one of the bundled examples — and ask the on-device Gemini Nano model to describe it.

## The idea

Chrome is shipping a set of built-in AI APIs that run **locally** on the user's device using Gemini Nano. No server, no API key, no network round-trip for inference. The Prompt API is the most general of these: you send text and (optionally) images or audio, and you get text back, either as a single response or as a stream.

This demo is a minimal sandbox for poking at the image-input side of that API:

- A canvas you can draw on with mouse/touch (brush size + color).
- Buttons to upload a local image, or pick one of three pre-baked examples.
- A "Describe the image" button that hands the canvas bitmap to `LanguageModel` and renders the streamed reply as Markdown.
- A settings panel exposing every knob the spec offers: system prompt, user prompt text, assistant prefix, `temperature`, `topK`, `expectedInputs` (audio toggle), streaming vs. non-streaming, `responseConstraint` (JSON Schema), `omitResponseConstraintInput`.
- A live debug log showing availability, params, download progress, token usage, context window, TTFB, chunk counts, and any errors.

It's intentionally one HTML file so you can read the whole thing top-to-bottom and see exactly how each Prompt API surface is used.

## Chrome context — what you need

The Prompt API with multimodal input is gated behind a Chrome version + flags + hardware combo. As of this demo:

- **Chrome 148+** (desktop). Earlier versions either don't have the API or don't have image input.
- Enable the flags:
  - `chrome://flags/#optimization-guide-on-device-model`
  - `chrome://flags/#prompt-api-for-gemini-nano-multimodal-input`
- Hardware: ~22 GB free disk (the model is downloaded on first use), GPU with **>4 GB VRAM**.
- The first session triggers a model download — the demo surfaces a progress bar via the `monitor` callback.

You can inspect the on-device model state at `chrome://on-device-internals`.

## Running it

The page must be served over HTTP (the example loader uses `fetch()`, which is blocked on `file://`). From this directory:

```sh
python3 -m http.server 8000
# then open http://localhost:8000/
```

## Files

- `index.html` — the entire demo (markup, styles, logic).
- `example1.png`, `example2.png`, `example3.png` — sample images selectable from the "Load example" dropdown.

## References

- [Chrome Prompt API explainer](https://developer.chrome.com/docs/ai/prompt-api)
- [Built-in AI overview](https://developer.chrome.com/docs/ai/built-in)
