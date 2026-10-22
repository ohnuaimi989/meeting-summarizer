# Home Assistant Meeting Summarizer

An offline-first Python tool for summarizing meeting transcripts and audio recordings. Uses Hugging Face models for local summarization, with optional OpenAI-powered summarization.

Designed to help you turn meeting transcripts into concise, readable notes while keeping local processing available when you prefer not to send transcript content to an external API.

---

## Features

- Offline text summarization using Hugging Face Transformers models, including BART and T5-compatible models.
- Optional OpenAI integration for cloud-based summarization.
- Audio transcription using Whisper, followed by local summarization.
- Multiple input types: summarize text strings and transcript files.
- Configurable models and backends through the summarizer configuration.
- Logging and error handling to help diagnose problems.
- Examples and tests to help you get started and validate behavior.

Privacy note: Local summarization can keep transcript text on your machine after the necessary models have been downloaded. The optional OpenAI backend sends the relevant transcript content to OpenAI. Audio transcription and model downloads may also involve external services depending on your configuration and environment.

---

## Installation

Clone or upload this project folder to your system.

Install dependencies:

```bash
pip install -r requirements.txt
````

> ⚠️ **System dependency:** FFmpeg must be installed for audio file support.
>
> * Ubuntu: `sudo apt install ffmpeg`
> * macOS: `brew install ffmpeg`
> * Windows: Download from [https://ffmpeg.org/download.html](https://ffmpeg.org/download.html)


For online GPT models, set your OpenAI API key:

```bash
export OPENAI_API_KEY="your_openai_api_key"
```

---

## Usage

### Offline summarization (default)

```python
from ha_meeting_summarizer.ha_summarizer import HASummarizer

summarizer = HASummarizer()  # defaults to offline Hugging Face
summary = summarizer.summarize_file("examples/sample_meeting.txt")
print(summary)
```

### **Offline audio summarization**

```python
summary_audio = summarizer.summarize_audio("examples/sample_meeting.wav")
print("Offline audio summary:")
print(summary_audio)
```

### Online summarization (optional)

```python
summarizer_online = HASummarizer(
    backend="online",
    openai_model="gpt-4",
    openai_api_key="YOUR_API_KEY"
)
summary_online = summarizer_online.summarize_file("examples/sample_meeting.txt")
print(summary_online)
```

---

## Testing

Run the included tests using pytest:

```bash
pytest tests/
```

* Tests cover offline summarization, empty text, and missing file handling.
* Online tests are optional since they require an OpenAI API key.

---

## Dependencies

* `transformers>=4.40.0` (offline summarization)
* `torch>=2.0.0` (required for transformers)
* `openai>=0.27.0` (online summarization)
* `pytest>=7.0.0` (testing)
* `torchaudio>=2.1.0`
* `System dependency: FFmpeg for audio support.`

---

## Project Structure

```text
ha-meeting-summarizer/
├── ha_meeting_summarizer/
│   ├── __init__.py
│   ├── ha_summarizer.py
│   └── utils.py
├── examples/
│   ├── sample_meeting.txt
│   ├── sample_meeting.wav
│   ├── run_example.py
│   └── generate_sample_audio.py
├── tests/
│   └── test_ha_summarizer.py
├── requirements.txt
├── README.md
└── .gitignore
```

---

## Acknowledgements

This project was inspired by [Liquid4All’s Home Assistant Meeting Summarization example](https://github.com/Liquid4All/cookbook/tree/pau/examples/home-assistant/examples/meeting-summarization).

