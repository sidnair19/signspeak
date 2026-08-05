# SignSpeak – AI-Powered Sign Language Communication System

**Nov 2024 – Apr 2025**

SignSpeak is an AI-driven real-time communication system designed to bridge the gap between deaf/mute individuals and hearing users. The platform converts American Sign Language (ASL) gestures to English speech, English speech/text to animated ASL signs, and supports multilingual translation to improve accessibility in everyday communication.

---

## Overview

SignSpeak enables bidirectional communication between sign language users and hearing individuals. Deaf and mute users can perform ASL gestures in front of a webcam and receive grammatically correct English sentences with audio output. Hearing users can enter spoken or typed English text and receive animated ASL sign videos in response.

The system is built for real-world use in public spaces, education, and healthcare, with an emphasis on inclusivity and ease of use.

---

## Key Features

### ASL Gesture → English Speech

- Real-time hand tracking via **MediaPipe**
- Custom-trained **SVM** and **deep learning (Keras)** models for gesture and letter recognition
- **Google Gemini** for natural sentence construction from recognized signs
- **Text-to-speech (gTTS)** for audio output

### English Speech/Text → ASL Animation

- **NLTK**-based NLP pipeline for tokenization, POS tagging, lemmatization, and tense detection
- Maps processed words to pre-recorded ASL video animations
- Fallback to letter-by-letter signing when a word animation is unavailable

### Multilingual Support

- Translates recognized sentences into multiple languages using **Google Translate**
- Supports Tamil, Hindi, Kannada, and additional language output via TTS

### Indian Sign Language (ISL) Module

- Dedicated one-hand and two-hand SVM models for ISL recognition
- Real-time gesture-to-speech conversion

### User Authentication

- Secure sign-up and sign-in with **SQLite** user database
- Personalized dashboard for accessing all SignSpeak modules

---

## Tech Stack

| Layer | Technologies |
|-------|-------------|
| **Backend** | Python, Flask |
| **Frontend** | HTML, CSS, Bootstrap, JavaScript |
| **Computer Vision** | OpenCV, MediaPipe |
| **Machine Learning** | TensorFlow/Keras, scikit-learn (SVM), NumPy |
| **NLP & AI** | NLTK, Google Gemini API |
| **Speech & Translation** | gTTS, Google Translate |
| **Database** | SQLite |
| **Desktop UI** | Tkinter (ASL capture module) |

---

## Project Structure

```
SignSpeak/
├── app.py                  # Flask web server & routing
├── main2.py                # ASL letter recognition + Gemini + TTS (Tkinter GUI)
├── gesture_to_voice.py     # Deep learning gesture recognition module
├── hand_recognition.py     # Indian Sign Language recognition
├── model.pkl               # ASL letter SVM model
├── one_hand.pkl            # ISL one-hand SVM model
├── two_hand.pkl            # ISL two-hand SVM model
├── mp_hand_gesture/        # Keras gesture recognition model
├── gesture.names           # Gesture class labels
├── requirements.txt        # Python dependencies
├── user_data.db            # SQLite user database
├── templates/
│   ├── index.html          # Landing page & authentication
│   ├── userlog.html        # User dashboard
│   └── animation.html      # Speech/text → ASL animation viewer
└── static/
    ├── assets/             # ASL sign video animations (.mp4)
    ├── css/                # Stylesheets
    ├── js/                 # Client-side scripts
    └── img/                # UI images
```

---

## System Architecture

**Sign → Speech**

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│   Webcam Input  │────▶│  MediaPipe Hands │────▶│  ML Models      │
│   (ASL Gestures)│     │  (Landmark Det.) │     │  SVM / Keras    │
└─────────────────┘     └──────────────────┘     └────────┬────────┘
                                                          │
                        ┌──────────────────┐              ▼
                        │  Google Gemini   │◀── Recognized Signs/Letters
                        │  (Sentence Gen.) │
                        └────────┬─────────┘
                                 │
          ┌──────────────────────┼──────────────────────┐
          ▼                      ▼                      ▼
   ┌─────────────┐      ┌──────────────┐      ┌─────────────────┐
   │  gTTS Audio │      │  Translation │      │  ASL Animation  │
   │  Output     │      │  (Multi-lang)│      │  Video Playback │
   └─────────────┘      └──────────────┘      └─────────────────┘
```

**Speech → Sign**

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│  Text / Speech  │────▶│  NLTK Pipeline   │────▶│  ASL Video      │
│  Input          │     │  (NLP Processing)│     │  Assets         │
└─────────────────┘     └──────────────────┘     └─────────────────┘
```

---

## Prerequisites

- Python 3.8+
- Webcam
- Google Gemini API key
- macOS / Windows / Linux

---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/signspeak.git
cd signspeak
```

### 2. Create and activate a virtual environment

```bash
python3 -m venv venv
source venv/bin/activate        # macOS/Linux
# venv\Scripts\activate         # Windows
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Download NLTK data

```bash
python -c "import nltk; nltk.download('punkt'); nltk.download('stopwords'); nltk.download('averaged_perceptron_tagger'); nltk.download('wordnet')"
```

### 5. Configure environment variables

Create a `.env` file or export your API key:

```bash
export GEMINI_API_KEY="your-gemini-api-key-here"
```

> **Security note:** Never commit API keys to version control. Replace any hardcoded keys in source files with environment variables before publishing.

---

## Usage

### Start the web application

```bash
python app.py
```

Open your browser and navigate to:

```
http://127.0.0.1:5000
```

### Available Modules

| Module | Route / Script | Description |
|--------|----------------|-------------|
| Word → Sign Animation | `/animation_view` | Enter English text; view ASL sign animations |
| Indian Sign Language | `/signlanguage` | Launch ISL gesture recognition (webcam) |
| Sign → Word/Sentence | `/signtoword` | Launch ASL letter capture with AI sentence generation |

### Standalone Scripts

```bash
# ASL letter recognition with Gemini + TTS
python main2.py

# Deep learning gesture recognition
python gesture_to_voice.py

# Indian Sign Language recognition
python hand_recognition.py
```

---

## How It Works

### Sign → Speech Pipeline

1. Webcam captures hand gestures in real time
2. MediaPipe extracts 21 hand landmarks per frame
3. SVM/Keras models classify gestures or ASL letters
4. Captured letters are assembled into words
5. Google Gemini constructs a grammatically correct sentence
6. gTTS converts the sentence to speech; optional translation plays in the selected language

### Speech → Sign Pipeline

1. User enters English text via the web interface
2. NLTK tokenizes, tags, and lemmatizes the input
3. Tense is detected (past, present, future) and adjusted for ASL grammar
4. Each word is matched to a pre-recorded ASL video in `static/assets/`
5. Animations are played sequentially in the browser

---

## Skills Demonstrated

- Artificial Intelligence (AI)
- Machine Learning & Deep Learning
- Computer Vision
- Natural Language Processing (NLP)
- Full-Stack Web Development
- Accessibility & Inclusive Design

---

## Future Enhancements

- Real-time bidirectional video chat with live translation
- Expanded ASL vocabulary and phrase library
- Mobile app (React Native / Flutter)
- Support for additional sign languages (BSL, LSF, DGS)
- Cloud deployment with scalable model inference
- Offline mode with on-device models

---

## Contributing

Contributions are welcome. Please open an issue or submit a pull request with a clear description of your changes.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add your feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Open a Pull Request

---

## License

This project is for educational and research purposes. Contact the author for licensing details.

---

## Acknowledgments

- MediaPipe for hand landmark detection
- Google Gemini for AI-powered sentence generation
- NLTK for natural language processing
- The deaf and hard-of-hearing community for inspiration and accessibility guidance

---

## Contact

**SignSpeak Team**  
Project Duration: November 2024 – April 2025

For questions or collaboration, reach out via GitHub Issues.

---

## Before Publishing

1. **Replace placeholder content** — Update the clone URL, team name, and any placeholder text in `templates/index.html`.
2. **Remove secrets** — `main2.py` contains a hardcoded Gemini API key; move it to an environment variable before pushing to GitHub.
3. **Add screenshots** — Include `demo.jpg` or screenshots of the Tkinter GUI and web dashboard in a `docs/` or `screenshots/` folder.
4. **Add a `.gitignore`** — Exclude `venv/`, `*.pkl`, `user_data.db`, `*.mp3`, and `.env`.
