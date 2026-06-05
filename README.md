# Minimal Voice Interaction Loop

## Overview

This project implements a minimal end-to-end voice interaction system.

Pipeline:

Microphone → Whisper ASR → Gemini LLM → Piper TTS → Audio Playback

The goal was not to build a production-grade assistant, but to demonstrate a complete voice loop that accepts spoken input and returns spoken output.

---

## Components

### Speech Recognition

* Model: OpenAI Whisper
* Input: Microphone audio
* Output: Text transcript

### Language Model

* Model: Google Gemini
* Input: Transcript from Whisper
* Output: Text response

### Text-to-Speech

* Engine: Piper
* Input: Gemini response
* Output: Synthesized speech (reply.wav)

---

## Workflow

1. Record audio from microphone.
2. Save recording as input.wav.
3. Transcribe audio using Whisper.
4. Send transcript to Gemini.
5. Generate a text response.
6. Clean markdown/special characters.
7. Convert text to speech using Piper.
8. Save generated audio as reply.wav.
9. Play generated response.

---

## What Worked

* End-to-end voice interaction.
* Accurate speech transcription using Whisper.
* Context-aware responses from Gemini.
* Speech synthesis using Piper.
* Generation of input.wav, response.txt, and reply.wav.

---

## What Didn't Work / Limitations

* Fixed-duration recording.
* No voice activity detection.
* No interruption handling.
* No streaming responses.
* No conversation memory between runs.
* Latency depends on Whisper and Gemini response time.
* Piper occasionally sounds robotic on long responses.

---

## Future Improvements

* Faster-Whisper
* Voice Activity Detection (VAD)
* Streaming LLM responses
* Conversation memory
* Wake-word detection
* Real-time audio pipeline

---

## Requirements

* Python 3.10+
* Whisper
* Google Generative AI SDK
* Piper
* SoundDevice
* NumPy
* SciPy

Install dependencies:

pip install -r requirements.txt

---

## Author

Prathamesh Khoje
