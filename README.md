# Minimal End-to-End Voice Interaction Loop

## Overview

This project implements a minimal end-to-end voice interaction loop capable of recording user speech, transcribing it into text, generating a response using a Large Language Model (LLM), converting the generated response back into speech, and playing the audio response to the user.

The objective was not to build a production-ready voice assistant, but rather to create a functional proof-of-concept that demonstrates the complete voice interaction pipeline within a limited timeframe.

### Architecture

Microphone → Whisper ASR → Gemini LLM → Text Processing → Piper TTS → Audio Playback

---

## What I Built

The system performs the following steps:

1. Records audio from the microphone.
2. Saves the recording as `input.wav`.
3. Uses Whisper to transcribe speech into text.
4. Sends the transcript to Gemini for response generation.
5. Cleans the generated text to remove unwanted formatting.
6. Converts the response into speech using Piper.
7. Saves the generated speech as `reply.wav`.
8. Plays the synthesized audio response.

Generated artifacts:

* `input.wav`
* `response.txt`
* `reply.wav`

---

## What Worked

* Successfully captured audio input from the microphone.
* Whisper accurately transcribed spoken queries.
* Gemini generated contextually relevant responses.
* Piper synthesized speech from generated text.
* The complete pipeline worked end-to-end without requiring manual intervention between stages.
* Audio responses were generated and played back successfully.

---

## Challenges Encountered and How They Were Resolved

### 1. Selecting an LLM API

My initial approach was to use OpenAI's API for the language model component. However, I quickly encountered API quota limitations and usage restrictions. I also explored other cloud-based LLM APIs, but some required paid subscriptions or billing setup before they could be used.

To continue development without additional setup overhead, I switched to the Gemini API, which integrated smoothly and provided reliable responses for the project.

### 2. Improving Speech Recognition Accuracy

The first implementation used Whisper's **base** model. While functional, transcription accuracy was not always satisfactory.

To improve recognition quality, I upgraded to Whisper's **small** model. This resulted in noticeably better transcription accuracy while maintaining acceptable performance for a proof-of-concept system.

### 3. Generating Audio Responses with Piper

One of the major issues encountered was the failure to generate the `reply.wav` file.

After debugging the issue, I discovered that Piper requires a pretrained voice model for speech synthesis. Additionally, both the model file (`.onnx`) and its configuration file (`.onnx.json`) must be present and correctly referenced.

Once the required files were downloaded and configured properly, Piper successfully generated audio responses.

### 4. Improving Voice Quality

The initial implementation used the **Alan** voice model. Although functional, the generated speech sounded noticeably robotic.

To improve the user experience, I switched to the **en_US-amy-medium** voice model, which produced significantly more natural and conversational speech output.

### 5. Unwanted Symbols in Speech Output

Another challenge occurred when the language model occasionally generated markdown symbols such as asterisks (`*`) and other formatting characters.

These symbols were sometimes spoken by the text-to-speech engine, resulting in unnatural audio output.

To address this issue:

* I instructed the LLM to generate plain-text responses without markdown formatting.
* I added a preprocessing step that removes markdown symbols and unwanted characters before sending text to Piper.

This produced cleaner and more natural speech synthesis.

---

## Current Limitations

Since the goal was to deliver a working proof-of-concept quickly, several advanced capabilities were intentionally left out:

* No streaming speech recognition.
* No streaming LLM responses.
* No interruption handling during playback.
* Fixed-duration recording instead of Voice Activity Detection (VAD).
* No conversation memory between interactions.
* No wake-word detection.
* No real-time audio pipeline.

These limitations were acceptable for the scope of this project because the primary objective was to build a complete working voice loop rather than a production-grade voice assistant.

---

## Key Takeaway

Although the final implementation is intentionally simple, the most valuable part of the project was solving the integration challenges between ASR, LLM, and TTS components. The project successfully demonstrates a complete voice interaction pipeline while providing practical insights into model selection, API integration, speech synthesis configuration, and debugging real-world AI workflows.
