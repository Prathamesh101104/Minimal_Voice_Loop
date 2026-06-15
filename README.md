# AI Voice Assistant (Week 2)

## Overview

This project is a real-time AI Voice Assistant that combines Speech Recognition, Voice Activity Detection (VAD), a Large Language Model (LLM), and Text-to-Speech (TTS) synthesis into a single conversational pipeline.

Unlike the Week 1 Proof-of-Concept, which relied on fixed-length audio recording, this implementation continuously listens to the microphone and automatically detects when the user starts and stops speaking.

---

## Architecture

```text
User Speech
      ↓
Silero VAD
      ↓
Speech Chunk Collection
      ↓
Faster-Whisper ASR
      ↓
Gemini 2.5 Flash
      ↓
Piper TTS
      ↓
Spoken Response
```

---

## Week 2 Improvements

Compared to the Week 1 implementation, the following enhancements were added:

* Continuous microphone capture
* Automatic speech start/end detection using Silero VAD
* Dynamic speech chunk collection
* Faster-Whisper integration for local speech recognition
* ASR latency measurement
* Voice-based assistant termination commands
* Improved conversational flow and responsiveness

---

## Features

* Continuous microphone listening
* Automatic Voice Activity Detection (VAD)
* Local Speech-to-Text using Faster-Whisper
* AI-generated responses using Gemini 2.5 Flash
* Offline Text-to-Speech using Piper
* Real-time latency monitoring
* Voice-controlled stop commands
* End-to-end conversational interaction

---

## Technologies Used

### Speech Processing

* Silero VAD
* Faster-Whisper

### Language Model

* Gemini 2.5 Flash

### Text-to-Speech

* Piper TTS

### Python Libraries

* torch
* numpy
* scipy
* sounddevice
* soundfile
* python-dotenv
* google-generativeai

---

## Project Structure

```text
MINIMAL_VOICE_LOOP/
│
├── Silerio_VAD.ipynb
├── .env
├── response.txt
├── temp_chunk.wav
├── reply.wav
│
├── piper/
│   ├── piper.exe
│   ├── en_US-amy-medium.onnx
│   └── en_US-amy-medium.onnx.json
│
├── requirements.txt
└── README.md
```

---

## Setup

### 1. Clone the Repository

```bash
git clone <repository-url>
cd MINIMAL_VOICE_LOOP
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure Gemini API Key

Create a `.env` file in the project root directory.

```env
GEMINI_API_KEY=YOUR_API_KEY_HERE
```

> Note: The `.env` file is excluded from GitHub using `.gitignore` to prevent API key exposure.



## Running the Assistant

Open:

```text
Silerio_VAD.ipynb
```

Run all cells sequentially.

The assistant will begin listening automatically.



## Stop Commands

The assistant can be terminated using the following voice commands:

* stop
* exit
* quit
* goodbye
* stop assistant



## Whisper Model Evaluation

Two Faster-Whisper models were evaluated during development.

| Model  | Accuracy | Latency |
| ------ | -------- | ------- |
| Small  | Good     | Low     |
| Medium | Better   | Higher  |

### Final Selection

The Faster-Whisper Small model was selected because the project prioritizes real-time conversational interaction and lower latency. While the Medium model improved transcription accuracy, it increased inference time and reduced responsiveness.



## Challenges Faced

### 1. Silero VAD Input Size Mismatch

**Issue:**

text
ValueError: Provided number of samples is 1536


**Solution:**

* Changed audio block size to 512 samples.
* Matched Silero VAD input requirements.



### 2. Speech Endpoint Detection

**Issue:**
The assistant could not reliably determine when a user had finished speaking.

**Solution:**

* Tuned silence thresholds.
* Reset VAD states after each interaction.



### 3. Continuous Listening Control

**Issue:**
The assistant continuously listened without a termination mechanism.

**Solution:**

* Added voice-based stop commands.



### 4. Piper Audio Playback Integration

**Issue:**
Speech audio was generated but not automatically played.

**Solution:**

* Integrated sounddevice and soundfile for playback.



### 5. Whisper Accuracy vs Latency Trade-off

**Issue:**
The Medium model improved accuracy but increased latency.

**Solution:**

* Selected Faster-Whisper Small for deployment.



## Future Improvements

* Real-time web search integration
* Weather and news APIs
* Multi-turn conversation memory
* Wake-word detection
* Streaming transcription
* Local LLM support
* Speaker identification





AI Voice Assistant – Week 2 Implementation
