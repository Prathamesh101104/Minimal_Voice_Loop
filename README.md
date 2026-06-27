# Week 4 – LLM Prompt Engineering & Voice Agent Optimization

## Overview

This week's objective was to improve the conversational behavior of the voice agent by designing voice-specific system prompts and evaluating their impact on response quality and latency.

Unlike Week 3, which focused on integrating streaming Text-to-Speech (TTS), this week concentrated on making the Large Language Model (LLM) behave like a natural voice assistant. Two different assistant personalities were implemented and evaluated while keeping the rest of the voice pipeline unchanged.

The complete voice agent pipeline is:

```
Continuous Microphone
        ↓
Silero VAD
        ↓
Faster-Whisper ASR
        ↓
Gemini LLM
        ↓
Piper Streaming TTS
        ↓
Speaker Output
```

---

# Model Choice

## LLM

Groq API (Llama-based Model)

Reason for Selection

The Groq API was selected because it provides:

Very low inference latency, making it suitable for real-time voice agents.
Fast token generation, which helps reduce the delay before speech synthesis begins.
Good instruction-following capability, allowing the assistant's behaviour to be controlled through system prompts.
Streaming response support, enabling responses to be processed as they are generated.
Simple API integration for conversational AI applications.

The LLM is responsible for understanding the user's query, generating context-aware responses, and controlling the assistant's personality through prompt engineering. In this project, two different system prompts (Technical Assistant and Friendly Assistant) were implemented using the same Groq-powered pipeline to demonstrate how prompt design changes the behaviour of the voice agent without modifying the underlying architecture.

---

## Text-to-Speech

**Piper (Local Streaming TTS)**

### Reason for Selection

Piper was retained from Week 3 because it offers:

* Fully local execution
* Offline speech synthesis
* No API cost
* Low deployment complexity
* Privacy-friendly processing
* Streaming audio playback support

Although cloud services such as Cartesia and ElevenLabs produce more natural speech, Piper provides an effective balance between latency, simplicity, and local deployment.

---

# Voice-Tuned System Prompts

## 1. Technical Assistant

```
You are a concise technical assistant.

Keep responses brief and accurate.

Reply in one or two short sentences.

Avoid unnecessary greetings.

Avoid markdown, bullet points, and headings.

Use simple spoken English.

If uncertain, clearly say you do not know.

Maintain a professional and concise tone.
```

### Observed Behaviour

* Very concise responses.
* Mostly limited to one or two sentences.
* Lower total latency due to shorter outputs.
* Suitable for technical question answering.

---

## 2. Friendly Assistant

```
You are a friendly voice assistant.

Speak naturally and conversationally.

Be warm and welcoming.

Explain concepts in an easy-to-understand manner.

Avoid markdown, bullet points, and headings.

Keep responses conversational while remaining informative.

Maintain a friendly personality throughout the interaction.
```

### Observed Behaviour

* More conversational responses.
* Longer explanations.
* Friendlier tone.
* Higher total latency because of increased response length.
* Better suited for educational and customer-facing applications.

---

# Performance Evaluation

The voice agent was evaluated using three latency metrics.

## TTFT (Time to First Token)

TTFT measures the time between sending the prompt to the LLM and receiving the first generated token.

### Observations

```
Minimum : ~0 sec
Maximum : ~0 sec
Average : ~0 sec
```

The measured value remained approximately zero because token generation begins almost immediately and the current implementation records the first streamed token as soon as it is received.

---

## TTFA (Time to First Audio)

TTFA measures the time from when the user finishes speaking until the first synthesized audio begins playing.

### Technical Assistant

```
Minimum : 2.56 sec
Maximum : 4.62 sec
Average : ~3.49 sec
```

### Friendly Assistant

```
Minimum : 2.85 sec
Maximum : 4.32 sec
Average : ~3.79 sec
```

Observation:

The friendly assistant generally produced slightly higher TTFA due to longer responses and additional text generation before speech playback.

---

## Total Response Latency

Total latency measures the complete interaction time from the end of user speech until playback finishes.

### Technical Assistant

```
Minimum : 5.64 sec
Maximum : 18.35 sec
Average : ~10.73 sec
```

### Friendly Assistant

```
Minimum : 6.38 sec
Maximum : 60.34 sec
Average : ~33.97 sec
```

Observation:

The technical assistant consistently produced shorter responses, resulting in significantly lower total latency. In contrast, the friendly assistant generated more detailed explanations, increasing the overall interaction time.

---

# Sample Performance Comparison

| Prompt Type         | TTFA Range      | Total Latency Range | Response Style                           |
| ------------------- | --------------- | ------------------- | ---------------------------------------- |
| Technical Assistant | 2.56 – 4.62 sec | 5.64 – 18.35 sec    | Short, precise and task-oriented         |
| Friendly Assistant  | 2.85 – 4.32 sec | 6.38 – 60.34 sec    | Conversational, explanatory and engaging |

---

# Sentence Chunking Evaluation

Several edge cases were tested to evaluate sentence chunking and speech-friendly response generation.

| Test Case       | Example                          | Observation                                                                    |
| --------------- | -------------------------------- | ------------------------------------------------------------------------------ |
| Abbreviations   | AI, NLP, LLM, IPv4               | Expanded and pronounced correctly.                                             |
| Decimal Numbers | 3.14, 2.5, 32.5                  | Spoken naturally as "three point one four", "two point five", etc.             |
| Percentages     | 25%                              | Converted into natural speech.                                                 |
| Units           | 16 kHz                           | Pronounced as "sixteen thousand Hertz".                                        |
| Version Numbers | Version 2.5                      | Spoken naturally without incorrect sentence splitting.                         |
| URLs            | OpenAI.com, GitHub.com           | LLM summarized the websites instead of reading the URL character-by-character. |
| Long Responses  | Neural Networks, Cloud Computing | Streaming playback remained smooth with no audio cut-offs.                     |

---

# Observations

## Technical Assistant

Advantages

* Concise responses.
* Faster completion.
* Lower overall latency.
* Suitable for productivity-oriented assistants.

Limitations

* Less conversational.
* Occasionally produced responses that were too brief.

---

## Friendly Assistant

Advantages

* Natural conversational style.
* Better explanations.
* More engaging interaction.

Limitations

* Longer responses increased total latency.
* Occasionally ignored the instruction to remain concise.

---

# Key Learnings

* System prompts significantly influence the personality and conversational style of the voice agent without requiring any changes to the underlying pipeline.
* Response length has a direct impact on total interaction latency.
* Short, voice-optimized responses improve conversational flow and reduce waiting time.
* Streaming TTS combined with concise LLM responses provides a more responsive user experience.
* Testing edge cases such as abbreviations, decimal numbers, percentages, and version numbers helps verify the robustness of sentence chunking and speech synthesis.

---

# Conclusion

During Week 4, the voice agent was enhanced through prompt engineering rather than architectural changes. Two distinct assistant personalities were implemented and evaluated using the same ASR, LLM, and TTS pipeline.

The experiments demonstrate that carefully designed system prompts can substantially change the perceived behaviour of the assistant while also influencing latency and user experience. The technical assistant provided faster, concise responses, whereas the friendly assistant offered richer explanations at the cost of increased interaction time.

The final voice agent pipeline is:

```
Continuous Microphone
        ↓
Silero VAD
        ↓
Faster-Whisper ASR
        ↓
Gemini LLM (Voice-Tuned System Prompt)
        ↓
Piper Streaming TTS
        ↓
Speaker Output
```

This implementation establishes a foundation for future work involving token-level streaming, improved TTFA, interruption handling, and more advanced conversational memory.
