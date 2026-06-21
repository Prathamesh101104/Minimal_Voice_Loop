# Week 3 – Streaming TTS Integration

## Overview

This week focused on upgrading the voice agent pipeline by replacing the previous speech output module with a streaming Text-to-Speech (TTS) system. The goal was to reduce perceived latency and make conversations feel more natural by beginning audio playback before complete speech synthesis finished.

The updated voice agent pipeline is:

Continuous Microphone → Silero VAD → Faster-Whisper ASR → Gemini LLM → Piper Streaming TTS → Speaker Output

---

## TTS Provider Selected

### Piper TTS (Local Streaming Playback)

Piper is an open-source neural Text-to-Speech engine that runs entirely on local hardware without requiring external APIs.

### Why Piper?

Piper was selected because:

* Runs completely offline.
* No API key required.
* No recurring usage costs.
* Low deployment complexity.
* Privacy-friendly (all processing remains local).
* Supports low-latency chunked audio playback.
* Suitable for real-time voice agent applications.

While cloud providers such as Cartesia and ElevenLabs offer higher voice quality, Piper provides a strong balance between performance, simplicity, privacy, and cost.

---

## Updated Voice Agent Architecture

```text
User Speech
      │
      ▼
Microphone Input
      │
      ▼
Silero VAD
      │
      ▼
Faster-Whisper ASR
      │
      ▼
Gemini LLM
      │
      ▼
Piper Streaming TTS
      │
      ▼
Speaker Output
```

---

## Streaming TTS Implementation

The generated LLM response is sent directly to Piper.

Instead of waiting for the complete audio file to be synthesized before playback begins, audio is played as synthesized chunks become available.

```text
LLM Response
      │
      ▼
Piper TTS
      │
      ├── Audio Chunk 1 → Playback Starts
      ├── Audio Chunk 2
      ├── Audio Chunk 3
      └── Remaining Audio Chunks
```

This significantly reduces perceived latency and creates a more conversational user experience.

---

## Performance Evaluation

Latency measurements were collected during multiple test conversations.

### ASR Latency

Observed Range:

* Minimum: 0.61 sec
* Maximum: 2.03 sec
* Average: ~1.18 sec

### LLM Latency

Observed Range:

* Minimum: 0.36 sec
* Maximum: 2.00 sec
* Average: ~0.84 sec

### TTS Time-To-First-Byte (TTFB)

Observed Range:

* Minimum: 0.883 sec
* Maximum: 2.442 sec
* Average: ~1.47 sec

TTFB measures the time between receiving the complete LLM response and playing the first audio chunk through the speakers.

### Total Synthesis Time

Observed Range:

* Minimum: 5.864 sec
* Maximum: 23.955 sec
* Average: ~16.99 sec

Total synthesis time measures the duration from the start of speech generation until the final audio chunk has been played.

---

## Sample Results

| User Query                  | TTFB (sec) | Total Synthesis Time (sec) |
| --------------------------- | ---------- | -------------------------- |
| Forties Generative AI       | 1.665      | 15.669                     |
| Future Trends in AI         | 1.443      | 23.871                     |
| What is Agentic AI?         | 1.496      | 23.955                     |
| What is Rack Pipeline?      | 1.476      | 20.441                     |
| What is Generative AI?      | 1.487      | 23.894                     |
| Fetch Current Euro-INR Rate | 0.883      | 17.137                     |

---

## Quality Observations

### Naturalness

* Voice output is clear and understandable.
* Speech sounds reasonably natural for a local TTS model.
* Slightly more robotic than premium cloud providers such as ElevenLabs or Cartesia.
* Consistent pronunciation and pacing across responses.

### Numbers

Tested Examples:

* Exchange rates
* Years (2025, 2026)
* Currency values

Observations:

* Numbers were pronounced correctly in most scenarios.
* Currency-related responses remained understandable.

### Acronyms and Technical Terms

Tested Examples:

* AI
* ASR
* LLM
* Agentic AI

Observations:

* Common AI-related terminology was pronounced clearly.
* Technical terms were generally understandable.

### Long Responses

Observations:

* No mid-sentence cutoffs observed.
* No audio truncation detected during testing.
* Long responses maintained sentence continuity.
* Audio playback remained stable throughout synthesis.

---

## Streaming Behaviour

The system begins audio playback approximately 1–2 seconds after receiving the LLM response.

Observed behaviour:

```text
LLM Response Ready
        │
        ▼
~1.47 sec
        │
        ▼
First Audio Chunk Played
        │
        ▼
Remaining Audio Continues Streaming
        │
        ▼
Final Audio Chunk Played
```

This allows users to hear responses much earlier than waiting for complete synthesis, resulting in a significantly more conversational interaction loop.

---

## Key Learnings

* TTFB has a larger impact on perceived responsiveness than total synthesis duration.
* Streaming playback significantly improves user experience.
* Piper provides an effective local alternative to cloud TTS services.
* Local deployment removes API costs and privacy concerns.
* Measuring both TTFB and total synthesis time provides a better understanding of real-world voice agent performance.

---

## Conclusion

Piper TTS was successfully integrated as a streaming speech synthesis module within the voice agent pipeline.

The final system supports:

Continuous Mic → VAD → ASR → Gemini LLM → Streaming TTS → Speakers

The implementation achieves low-latency speech output, local deployment, offline capability, and conversational responsiveness while maintaining acceptable speech quality for real-time voice interactions.
