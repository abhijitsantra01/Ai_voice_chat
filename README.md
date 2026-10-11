# Leela AI Voice Chatbot

A Windows-friendly voice chatbot using the Gemini Live API. It listens for a
Leela wake phrase, streams speech to Gemini, and plays Gemini's spoken response
through the selected output device.

## Files

- `gemini_voice_test.py` — chatbot and wake-word listener.
- `AI_voice_chat_req.txt` — Python package dependencies.
- `.env` — optional local configuration for the API key and voice.

## Requirements

- Python 3.13 (the version recorded in `.python-version`).
- A working microphone and speaker or headphones.
- A Gemini API key with access to the configured Gemini Live model.
- Internet access while chatting. Wake-word detection itself runs locally using
  PocketSphinx.

## Setup

Open PowerShell in the folder containing the chatbot files. Create and activate
a virtual environment, then install the dependencies:

```powershell
py -3.13 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r AI_voice_chat_req.txt
```

If PowerShell blocks activation, either allow script activation for your user
or invoke the virtual environment's Python directly:

```powershell
.\.venv\Scripts\python.exe -m pip install -r AI_voice_chat_req.txt
```

## Configure the API key

Create a `.env` file beside `gemini_voice_test.py`:

```dotenv
GEMINI_API_KEY=your-gemini-api-key
# Optional: Aoede, Charon, Fenrir, Kore, or Puck
GEMINI_VOICE=Kore
```

Keep `.env` private; do not commit or share your API key. Alternatively, set
`GEMINI_API_KEY` or `GOOGLE_API_KEY` in the environment.

## Run

```powershell
python gemini_voice_test.py
```

Say **“Hi Leela”**, **“Hey Leela”**, or another supported Leela wake phrase when
the console says it is in standby. If you say only the wake phrase, the chatbot
sends a short greeting prompt to Gemini. If you continue speaking immediately
after the wake phrase, that follow-up speech is sent as the first request.
Speech from before the wake phrase is not sent to Gemini.

By default, microphone input is paused while Gemini is speaking. To allow
interruptions (barge-in), run:

```powershell
python gemini_voice_test.py --allow-barge-in
```

Say “bye” to return to wake-word standby. After two minutes without user input,
the current chat closes and the chatbot returns to standby.

## Troubleshooting

- The console prints the microphone selected for the current standby cycle. If
  the preferred OPPO Enco Buds2 headset is unavailable, the script reports that
  it is using the system default microphone.
- Check Windows microphone privacy permissions and confirm the intended input
  device is enabled and selected.
- If package installation or startup fails, verify that the virtual environment
  is active and reinstall from `AI_voice_chat_req.txt`.
- If Gemini cannot connect, verify the API key and internet connection.

The chatbot also writes Gemini's spoken reply to `gemini_voice_reply.wav` in the
script folder; this file is generated during use and is not needed to start the
chatbot.
