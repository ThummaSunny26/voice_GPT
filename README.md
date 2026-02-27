# voice_GPT
Pure Voice Assistant (Jack) – Google Gemini + Optional OpenAI TTS

Overview

- Voice assistant that listens via your microphone and answers using Google Gemini (gemini-pro).
- Wake word is “Jack”. Say “Jack …” to wake; say “that’s all” to end the conversation.
- Speaks responses with local TTS (pyttsx3) by default; optionally uses OpenAI TTS for higher-quality voice.
- Streams model output and appends each conversation to a dated log file (chatlog-YYYY-MM-DD.txt).

What’s in the Repo

- pure.py – main script: wake word loop, Google Gemini calls, speech recognition, TTS playback, and chat logging.

Requirements

- Python 3.10+
- Windows with a working microphone
- Google Generative AI API key (Gemini) and optional OpenAI API key for TTS
- Packages:
  - google-generativeai
  - SpeechRecognition
  - pyttsx3
  - pyaudio
  - pygame
  - openai (only if using OpenAI TTS)

Quick Start

1) Create and activate a virtual environment

PowerShell:

python -m venv .venv
.venv\Scripts\Activate.ps1

2) Install dependencies

pip install google-generativeai SpeechRecognition pyttsx3 pygame openai

If PyAudio fails to install on Windows:

pip install pipwin
pipwin install pyaudio

or install a matching PyAudio wheel for your Python version from a trusted wheel index.

3) Configure API keys (recommended: environment variables)

- Google Gemini: set GOOGLE_API_KEY to your key.
- OpenAI (optional, only for OpenAI TTS): set OPENAI_API_KEY.

PowerShell examples:

$env:GOOGLE_API_KEY="your-gemini-key"
$env:OPENAI_API_KEY="your-openai-key"

Important: The current script contains a hard-coded Gemini API key line. For security, replace this with an environment read before running:

Replace:

genai.configure(api_key='YOUR_HARDCODED_KEY')

With:

genai.configure(api_key=os.getenv("GOOGLE_API_KEY", ""))

4) Run

python pure.py

You’ll see “Listening …”. Speak “Jack …” followed by your request. Say “that’s all” to end the active conversation and return to sleep mode.

Configuration and Controls

- Wake word: “Jack” (changeable in pure.py where text.lower().split("jack") is used).
- End phrase: “that’s all” while the assistant is awake.
- Local TTS: pyttsx3 is enabled by default.
  - Voice selection uses voices[1] (often female) and rate 190 WPM; adjust in pure.py.
- OpenAI TTS: set openaitts = True in pure.py to enable. Requires OPENAI_API_KEY.
- Energy threshold: the recognizer uses dynamic_energy_threshold = False and energy_threshold = 400; tweak for your environment.
- Logs: each run appends conversation lines to chatlog-YYYY-MM-DD.txt in the working directory.

Troubleshooting

- PyAudio installation issues on Windows:
  - Use pipwin as shown above or install a prebuilt wheel matching your Python version and architecture.
- Microphone not detected or access denied:
  - Check Windows Privacy & Security → Microphone settings, and ensure Python has access.
- TTS voice issues:
  - Some systems have limited voices. Try changing the voices index or installing additional voices.
- OpenAI TTS playback issues:
  - Ensure pygame is installed and initialized; check that the generated MP3 can be loaded.
- API errors or empty responses:
  - Verify GOOGLE_API_KEY and network access. For OpenAI TTS, verify OPENAI_API_KEY.

Security Notes

- Never commit API keys to source control. Use environment variables or a secrets manager.
- If the repository currently includes a hard-coded key, remove it immediately and rotate the key in your provider dashboard.

License

- No license is provided. Add a LICENSE file if you plan to share or open-source this project.

Acknowledgments

- Google Generative AI (Gemini) for text generation.
- OpenAI TTS (optional) for speech synthesis.
- SpeechRecognition, PyAudio, pyttsx3, and pygame for audio I/O and playback.
