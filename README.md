<div align="center">

# Speech AI Scripts & Workflows

**A growing library of provider-agnostic speech AI scripts, relays, and workflow examples.**

[![Node.js](https://img.shields.io/badge/Node.js-CommonJS-339933?logo=nodedotjs&logoColor=white)](speech-to-text-realtime-transcript-deepgram/package.json)
[![Deepgram SDK](https://img.shields.io/badge/Deepgram-SDK%201.x-13EF93)](speech-to-text-realtime-transcript-deepgram/package.json)

[Current catalog](#current-catalog) · [Catalog direction](#catalog-direction) · [Quick start](#quick-start) · [Protocol](#protocol) · [Status](#status-and-limitations)

</div>

## What is it?

This repository is intended as a growing collection of speech AI scripts, relays, and workflow examples. It can include text-to-speech, speech-to-text, streaming, batch, and provider-specific integrations.

**Current contents:** one speech-to-text relay and no TTS implementation. The current relay uses Deepgram, but the collection itself is not tied to one provider.

The current project opens a local WebSocket server, forwards incoming audio frames to Deepgram live transcription, and sends Deepgram transcript events back to the connected client.

Each entry documents its runtime, external dependencies, and operational limits independently.

## Current catalog

| Project | Runtime | Local endpoint | Service dependency |
| --- | --- | --- | --- |
| [`speech-to-text-realtime-transcript-deepgram/`](speech-to-text-realtime-transcript-deepgram) | Node.js, CommonJS | `ws://localhost:3002` | Deepgram live transcription API |

## Catalog direction

Future entries may add TTS utilities, alternative STT providers, local-model experiments, and reusable speech workflows. They will be listed only after their source, configuration requirements, and operational limitations are committed.

## Quick start

```bash
cd speech-to-text-realtime-transcript-deepgram
npm install
```

Create a `.env` file in that directory:

```env
DG_KEY=your_deepgram_api_key
```

Start the relay directly—the package does not define a `start` script:

```bash
node server.js
```

Connect a WebSocket client to `ws://localhost:3002` and send audio frames in a format accepted by the configured Deepgram SDK. The server forwards raw `transcriptReceived` payloads to the client.

## Protocol

```text
WebSocket client
    → binary audio frames
Local relay on :3002
    → Deepgram live transcription
Local relay
    → transcript event payloads
WebSocket client
```

The live configuration enables interim results, punctuation, endpointing, and voice-activity turnoff.

## Status and limitations

- **Implemented:** one-client-at-a-time connection handling per WebSocket session and live Deepgram forwarding.
- **Not implemented:** a browser recorder, audio format negotiation, authentication, rate limiting, persistence, TTS, or a production deployment configuration.
- `npm test` is the package template’s failing placeholder; there is no test suite.
- The package metadata points to an older standalone repository and names `server.js` as `main`, but documentation links should be treated as historical.
- `express` and `readme-md-generator` are installed but not used by `server.js`.
- The package declares MIT in metadata, but no license file is committed at the repository root.
