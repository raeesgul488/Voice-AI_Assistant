# Voice AI Assistant

A real-time voice AI assistant built with [LiveKit Agents](https://docs.livekit.io/agents/). It listens to a user over a LiveKit room, transcribes speech, generates a conversational response with an LLM, and speaks the reply back — with noise cancellation and turn detection built in for natural, low-friction conversations.

## Features

- **Speech-to-Text**: Deepgram Nova-3 (multilingual) via LiveKit Inference
- **LLM**: Google Gemma for generating responses
- **Text-to-Speech**: Inworld TTS with a configurable voice
- **Turn Detection**: Automatic turn-taking so the agent knows when to listen vs. respond
- **Noise Cancellation**: ai-coustics audio enhancement for cleaner input audio
- **Personality**: Friendly, concise, and formatting-free responses — designed to sound natural when read aloud by TTS

## How It Works

1. The agent joins a LiveKit room as a participant.
2. Incoming audio is cleaned up with ai-coustics noise cancellation, then transcribed via Deepgram STT.
3. The transcript is sent to the LLM, which generates a reply based on the assistant's instructions.
4. The reply is converted to speech via Inworld TTS and streamed back into the room.
5. On session start, the agent proactively greets the user.

## Prerequisites

- Python 3.9+
- A [LiveKit Cloud](https://cloud.livekit.io/) project (or self-hosted LiveKit server)
- API credentials for LiveKit and any inference providers you use

## Installation

```bash
git clone <your-repo-url>
cd <your-repo-name>
pip install -r requirements.txt
```

## Configuration

Create a `.env.local` file in the project root with your LiveKit credentials:

```dotenv
LIVEKIT_URL=wss://your-project.livekit.cloud
LIVEKIT_API_KEY=your-api-key
LIVEKIT_API_SECRET=your-api-secret
```

> ⚠️ **Never commit `.env.local` to version control.** Add it to `.gitignore` and rotate any keys that have been exposed publicly (e.g., pasted into a chat, issue, or commit).

## Running the Agent

```bash
python agent.py
```

This starts the agent server, which listens for and joins LiveKit rooms under the configured agent name (`my-agent`).

## Project Structure

```
.
├── agent.py          # Main agent definition and session logic
├── .env.local        # Environment variables (not committed)
├── requirements.txt  # Python dependencies
└── README.md
```

## Customization

- **Change the assistant's personality**: Edit the `instructions` string in the `Assistant` class.
- **Swap models**: Update the `stt`, `llm`, or `tts` model identifiers in `AgentSession`.
- **Adjust voice**: Change the `voice` parameter passed to `inference.TTS`.
- **Noise cancellation model**: Swap `ai_coustics.EnhancerModel.QUAIL_VF_S` for another supported model tier.

