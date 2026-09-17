# Manual Model Install (local fork note)

Electron downloads fail behind Groww's TLS-filtering VPN (Chromium handshake
killed, `net_error -100`), while curl works. Workaround: download out-of-band
and place files where the app expects them.

## Layout

- sherpa-onnx (Parakeet/Nemotron) models: `~/.cache/openwhispr/parakeet-models/<model-id>/`
  - extracted from the registry `downloadUrl` tar.bz2, dir name = registry `extractDir`
  - required files: `encoder.int8.onnx`, `decoder.int8.onnx`, `joiner.int8.onnx`, `tokens.txt`
- whisper.cpp models: `~/.cache/openwhispr/whisper-models/ggml-<model>.bin`
- install manifest (optional, written by the app after its own installs):
  `~/.cache/openwhispr/models/parakeet-models/<model-id>.json`
  - the app validates `extract_dir`; without a manifest it re-checks files on disk

## Applied

- nemotron-3.5-asr-streaming-0.6b (650MB) → extracted 2026-09-17
- whisper ggml-base.bin (141MB) → 2026-09-17

## Commands used

```bash
cd ~/.cache/openwhispr/parakeet-models
curl -LO <registry downloadUrl>
tar xjf <archive>.tar.bz2 && rm <archive>.tar.bz2
```

Note: `parakeetCapability.js` macOS pin lowered 15.5 → 14.0 in this fork so
the sherpa-onnx tab is enabled on macOS 14.8.
