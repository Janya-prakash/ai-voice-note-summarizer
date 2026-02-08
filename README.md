# AI Voice Note Summarizer

## Overview

This project implements a simple **AI-based voice note summarizer** in Python.

The idea is to help users quickly review the main points from recorded voice notes by:

1. **Transcribing** a short `.wav` audio file (voice note) into text using speech recognition (Google Web Speech API via the `speech_recognition` library).
2. **Summarizing** the transcribed text into a shorter version using an extractive NLP summarization algorithm (LSA-based summarizer from the `sumy` library).

The entire pipeline is implemented in a single Jupyter notebook.

---

## Features

- Accepts short `.wav` voice note recordings as input.
- Converts speech to text using Google's free Web Speech API.
- Applies an NLP-based summarizer to generate a concise 1–3 sentence summary.
- Provides reusable helper functions to:
  - `transcribe_audio(file_path)`: audio → transcript
  - `summarize_text(text, num_sentences)`: text → shorter text
  - `summarize_voice_note(audio_path, num_sentences)`: audio → transcript + summary

> **Note:** This is a prototype designed for short English voice notes and requires an internet connection for the Google speech recognition API.

---

## Tech Stack

- **Language:** Python
- **Libraries:**
  - [`speechrecognition`](https://pypi.org/project/SpeechRecognition/) – speech-to-text using Google Web Speech API
  - [`sumy`](https://pypi.org/project/sumy/) – extractive text summarization (LSA)
  - [`nltk`](https://www.nltk.org/) – sentence tokenization (`punkt`)
  - [`pydub`](https://github.com/jiaaro/pydub) – audio utilities (optional, if you want to do more advanced audio handling)
- **Environment:** Jupyter Notebook (run via VS Code)

---

## Project Structure

- `voice_note_summarizer.ipynb` – main notebook with:
  - Setup and imports
  - Helper functions for transcription and summarization
  - Example runs on sample voice notes (`my_note.wav`, `note2.wav`)
- `requirements.txt` – list of Python dependencies
- `README.md` – this documentation

> Audio files (`.wav`) are **not** included in this repository.  
> You should record your own short voice notes and place them in the same folder as the notebook.

---

## How It Works

### 1. Speech-to-Text (Transcription)

We use the `speech_recognition` library to call Google's Web Speech API:

- Load a `.wav` audio file with `sr.AudioFile`.
- Record the audio data using `Recognizer().record(...)`.
- Call `recognize_google(audio_data, language="en-IN")` (or `"en-US"`) to obtain the transcript.

This step converts a spoken voice note (e.g., explaining your study plan or daily tasks) into plain text.

### 2. Text Summarization

We use an **LSA-based extractive summarizer** from the `sumy` library:

- Parse the raw transcript as plain text.
- Use `Tokenizer("english")` and `LsaSummarizer()` to score sentences.
- Select the top `N` sentences (e.g. 2–3) as the summary.

This produces a shorter version that captures the key points of the voice note.

### 3. End-to-End Helper

A convenience function `summarize_voice_note(audio_path, num_sentences)` wraps the full pipeline:

1. Transcribe the given audio file.
2. Print the full transcript.
3. Generate and print a short summary.


# How to Run the Notebook
Clone or download this repository:

git clone https://github.com/Janya-prakash/ai-voice-note-summarizer.git
cd ai-voice-note-summarizer

# (Optional) Create and activate a virtual environment.

# Install Python dependencies:

pip install -r requirements.txt

# Or install manually:

pip install speechrecognition sumy nltk pydub

# Download NLTK resources required for tokenization (inside a Python session or notebook cell):

import nltk
nltk.download('punkt')
### some environments may also require:
### nltk.download('punkt_tab')

# Record or prepare your own .wav voice notes:

Example: my_note.wav, note2.wav
Place them in the same folder as voice_note_summarizer.ipynb.

# Open the notebook:

In VS Code: open the folder, then open voice_note_summarizer.ipynb and run the cells in order.
Or in Jupyter: run jupyter notebook and open the notebook in your browser.

# Run the example cell:

transcript, summary = summarize_voice_note("my_note.wav", num_sentences=2)
You should see both the full transcript and a shorter summary printed.

# Limitations & Future Improvements
Current limitations:

Designed for short English voice notes (e.g. 20–60 seconds).
Depends on an internet connection for Google’s speech recognition API.
Transcripts often lack punctuation, which can limit the quality of sentence-based summarization.
No graphical user interface; interaction is via notebook cells.

Potential future work:

Improve punctuation and sentence splitting before summarization.
Experiment with different summarization algorithms or transformer-based models.
Add support for longer audio files or batch processing.
Wrap the pipeline in a simple web or desktop interface (e.g. Streamlit, Gradio).
