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

## Stack (verified 16 Sep 2026)

| Need | Package | Version |
|---|---|---|
| LLM, ASR, OCR, embeddings, TTS | `react-native-executorch` | 0.10.2 |
| LLM alternative | `llama.rn` | 0.13.0-rc.3 |
| Speech to text | `whisper.rn` | 0.7.4 |
| OCR fallback | `@react-native-ml-kit/text-recognition` | 2.0.0 |
| NPU path | `onnxruntime-react-native` | 1.24.3 (QNN provider for Hexagon) |

Built with React Native + ExecuTorch. CPU/XNNPACK path first so it demonstrably works, then the hot model ported to the QNN execution provider on Sunday morning for the technical-depth points.

## The 30-hour build plan

| When | Ships |
|---|---|
| **Sat 11:00** | Fresh repo, dev build, model loads on the loaner and answers one prompt on-device |
| **Sat 13:00–17:00** | Clipboard round trip end to end — copy on laptop, summary back on laptop |
| **Sat 19:00** | Evaluation one — lead with airplane mode |
| **Sat 20:00–00:00** | Camera + ML Kit OCR path (built during Red Light window, phone-only on purpose) |
| **Sun 00:00–04:00** | Voice in, voice out (Whisper + TTS) |
| **Sun 07:00–09:00** | Local embedding index — cross-document memory |
| **Sun 09:00–12:00** | Port hot model to QNN; measure first-token latency before / after |
| **Sun 12:30** | Freeze, rehearse, submit repo before Top 10 lock |
| **Sun 15:00** | Top 10 pitch — airplane mode already on when we walk up |

## Why this shape scores

25% of the rubric is scored by HackTracker device data (camera, microphone, on-device inference, Office Kit bridge). Sidecar earns those counters through its core loop rather than through features bolted on at hour 28.

| Dimension | Weight | How Sidecar earns it |
|---|---|---|
| End product quality | 30% | A tool Priya would keep using on Monday |
| Novelty and impact | 20% | Phone as a trusted inference plane for a PC |
| Creative phone use | 15% | Camera, mic and NPU are the input path — not extras |
| Technical depth | 15% | Quantisation, QNN, thermals, latency budget |
| Office Kit usage | 10% | The bridge is the product, not a convenience |
| Demo and presentation | 10% | Airplane mode, one live counter, four beats |

## The demo

Phone in airplane mode on stage for the whole three-minute pitch, live byte counter reading zero the whole time. Copy an indemnity clause on the laptop → two seconds later the risk summary is on the laptop's clipboard. Point the camera at a printed annexure nobody can copy-paste and ask a question out loud. Ask a third question that needs both documents — the local embedding index answers.

## Artifacts in this repo

- `Sidecar_iQOO_Hyderabad.pdf` — the 7-slide submission deck
- `Sidecar_iQOO_Hyderabad.pptx` — editable source of the deck
- `Sidecar Build Kit.pdf` — the technical execution playbook (spike week + 30-hour blocks + pitch script)
- `Edge Runner Playbook.pdf` — the strategy document (event rules, competitive research, scoring, chosen idea)

## Team

**Team Edge Runner** — Working Professional bucket. Led by **Naga Balaji Nagendra**, Full Stack Developer at Gamyam.

## Originality

This repository holds Phase 1 submission artifacts only — the deck and the planning documents. The competition app will be built from a clean repository starting at 11:00 on Saturday 26 September 2026 in accordance with the iQOO Hackathon original-work rule. Any spike code produced during the week of 16–21 Sep is a throwaway prototype in a separate repository and is disclosed on the originality checkbox at submission.
