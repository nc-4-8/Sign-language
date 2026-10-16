# Sign Language Commands

A browser-based hand gesture recognizer. It uses your webcam and [MediaPipe Hands](https://github.com/google/mediapipe) to track your hand in real time, figures out which gesture you're making, and says it out loud.

Everything runs locally in the browser. No server, no build step, no video leaves your device.

## Features

- Real-time hand tracking with a skeleton overlay on the live webcam feed
- Recognizes 10 hand signs (listed below)
- Text-to-speech announces each sign using the browser's built-in speech synthesis
- Voice on/off toggle
- "Recent" history of the last 5 detected signs
- Hold-to-confirm filtering (a sign must be held for 6 frames) to reduce flickering and false detections

## Supported Signs

| Sign | Hand shape |
|------|-----------|
| Hello | Open hand, all five fingers extended |
| Stop | Four fingers extended, thumb tucked |
| Yes | Fist |
| Good | Thumbs up |
| Bad | Thumbs down |
| Call me | Thumb + pinky extended |
| I love you | Thumb + index + pinky extended |
| Peace | Index + middle fingers extended |
| One | Index finger only |
| L | Thumb + index finger extended |

> **Note:** These are simple hand gestures, not full American Sign Language (ASL) translation.

## Getting Started

### Requirements

- A modern browser (Chrome or Edge recommended)
- A webcam
- An internet connection (MediaPipe loads from a CDN)

### Run it

1. Clone the repo:
   ```bash
   git clone https://github.com/<your-username>/<your-repo>.git
   cd <your-repo>
   ```
2. Serve the folder over `localhost` (browsers only allow camera access on `localhost` or HTTPS, so opening the file directly may not work):
   ```bash
   # Python
   python3 -m http.server 8000

   # or Node
   npx serve
   ```
3. Open `http://localhost:8000` in your browser.
4. Click **Start** and allow camera access.
5. Hold a sign up to the camera.

## How It Works

1. **Capture:** the webcam stream is passed frame by frame to MediaPipe Hands.
2. **Landmarks:** MediaPipe returns 21 landmark points for the detected hand.
3. **Finger state:** each finger is marked as extended or curled by comparing distances from the wrist to the fingertip and the middle joint. The thumb uses its own check.
4. **Classification:** the five finger states form a 5-bit code (for example, `11111` is Hello), which is matched to a sign. Thumbs up vs. thumbs down is decided by whether the thumb tip is above or below the wrist.
5. **Smoothing:** a sign is only accepted after it stays the same for several frames in a row.
6. **Output:** the sign is shown on screen, added to history, and spoken aloud.

## Configuration

You can tweak these values in the script:

| Setting | Default | What it does |
|---------|---------|--------------|
| `HOLD_FRAMES` | `6` | Frames a sign must be held before it registers |
| `maxNumHands` | `1` | Number of hands tracked |
| `modelComplexity` | `1` | MediaPipe model accuracy vs. speed |
| `minDetectionConfidence` | `0.6` | Minimum confidence to detect a hand |
| `minTrackingConfidence` | `0.6` | Minimum confidence to keep tracking |

## Tech Stack

- HTML, CSS, JavaScript (single file)
- [MediaPipe Hands](https://github.com/google/mediapipe)
- Web Speech API (`speechSynthesis`)
- Canvas API

## Known Limitations

- Works best with good lighting and a plain background
- Only one hand is tracked at a time
- Similar shapes (like Yes vs. a partially closed hand) can occasionally be confused
- Speech voice depends on your browser and operating system

## Ideas for Future Work

- Add more signs, including the ASL alphabet
- Support two-handed and motion-based signs
- Train a custom classifier instead of rule-based detection
- Add an on-screen sentence builder

## License

MIT. Add a `LICENSE` file if you want this to be official.
