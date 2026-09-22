# Sidecar

**The private AI co-processor for your work laptop.**

Team Edge Runner (code D8LYC2) · iQOO Hackathon 2026 · Hyderabad Battle · Productivity Track · Working Professional bucket

---

## The idea

Every AI tool a working engineer uses is a browser tab that ships the employer's data to someone else's servers. So the most useful AI is banned exactly where the work is most valuable — client contracts, internal source code, unreleased specs, customer calls.

Sidecar turns the iQOO phone into a private inference plane for the laptop next to it. Office Kit's clipboard bridge and file transfer are the interface. A quantised Llama 3.2 3B runs on the phone's Hexagon NPU. The entire demo runs in airplane mode with a live byte counter reading zero.

## The user and the moment

**Priya, a delivery lead in Hyderabad.** Reviewing a 40-page client MSA on a laptop that blocks every AI tool, with a status call in an hour.

| Today | With Sidecar |
|---|---|
| Reads all 40 pages herself, twice | Copies a clause; the risk summary is back in her clipboard in about two seconds |
| Retypes payment terms into a mail to legal | Drops the PDF into the Office Kit folder and asks out loud what the exit terms are |
| Misses the indemnity clause on page 31 | Points the phone camera at the printed annexure that cannot be copied |
| Photographs the printed annexure and forgets | The laptop never opened a network connection to do any of it |

## Architecture

```
LAPTOP                    OFFICE KIT                 iQOO PHONE  (on-device only)
work happens here   ──── clipboard ────▶   ┌──────────────────────────────┐
no new software     ◀─── file xfer ─────   │ Whisper       ASR / TTS      │  ◀── CAMERA
no network calls                           │ ML Kit OCR    camera → text  │  ◀── MIC
                                           │ Llama 3.2 3B  INT4           │
                                           │ Embeddings    private index  │
                                           └──────────────────────────────┘
                                              Snapdragon Hexagon NPU
                                              ONNX Runtime + QNN
```

Every scored counter is load-bearing: camera (what can't be copied), microphone (hands-free), on-device model (the whole point), Office Kit (the only transport). Remove any one and the product stops working.

## Stack

| Need | Package |
|---|---|
| LLM, ASR, OCR, embeddings, TTS | `react-native-executorch` |
| LLM alternative | `llama.rn` (llama.cpp bindings, GGUF) |
| Speech to text | `whisper.rn` |
| OCR fallback | `@react-native-ml-kit/text-recognition` |
| NPU path | `onnxruntime-react-native` (QNN provider for Hexagon) |

Built with React Native + ExecuTorch. CPU / XNNPACK path first so it demonstrably works, then the hot model ported to the QNN execution provider on the Snapdragon Hexagon NPU.

## The demo

Phone in airplane mode on stage for the whole three-minute pitch, live byte counter reading zero the whole time. Copy an indemnity clause on the laptop → two seconds later the risk summary is on the laptop's clipboard. Point the camera at a printed annexure nobody can copy-paste and ask a question out loud. Ask a third question that needs both documents — the local embedding index answers.

## What Sidecar delivers

- **Zero bytes** leave the desk — total privacy
- **~2 second** answers — model runs on the phone chip, not the cloud
- **Fully offline** — works in airplane mode
- **Three input paths** — text, voice, camera

## Files in this repo

- `Sidecar_iQOO_Hyderabad.pdf` — the submission deck
- `deck.html` — a three-slide interactive walkthrough deck (open in a browser, arrow keys to navigate, `F` for fullscreen)

## Team

**Team Edge Runner** — Working Professional bucket. Led by **Naga Balaji Nagendra**, Full Stack Developer at Gamyam.

## Originality

This repository holds Phase 1 submission artifacts only — the deck and a walkthrough deck. The competition app will be built from a clean repository starting at 11:00 on Saturday 26 September 2026 in accordance with the iQOO Hackathon original-work rule.
