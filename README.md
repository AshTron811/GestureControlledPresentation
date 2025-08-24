# GestureControlledPresentation

GestureControlledPresentation lets you control a slide deck using hand gestures detected from your webcam. It's a lightweight Python project using `cvzone` (which wraps MediaPipe) and OpenCV for real-time hand detection and simple on-slide annotations.

---

## Features

* Navigate forward/backward through presentation slides using simple hand gestures.
* Draw annotations on the current slide with your index finger (freehand drawing).
* Erase the last annotation with a specific gesture.
* Live small-preview of the webcam feed overlaid on the slides.

---

## Requirements

* Python 3.8+ (works with 3.8–3.11)
* Webcam

Python packages (install via `pip`):

```
opencv-python
cvzone
mediapipe
numpy
```

> Optionally create a `requirements.txt` with the lines above for easy installation.

---

## Installation

1. Clone the repository:

```bash
git clone <your-repo-url>
cd GestureControlledPresentation
```

2. (Recommended) Create and activate a virtual environment:

```bash
python -m venv venv
# Windows
venv\Scripts\activate
# macOS / Linux
source venv/bin/activate
```

3. Install dependencies:

```bash
pip install -r requirements.txt
# or
pip install opencv-python cvzone mediapipe numpy
```

4. Prepare your slides:

Create a folder named `Presentation` in the project root and add your slide images (jpg/png). The script will load images from this folder in sorted order (by filename length then name). Example:

```
GestureControlledPresentation/
├─ main.py
├─ Presentation/
│  ├─ 1.png
│  ├─ 2.png
│  └─ 3.png
└─ requirements.txt
```

> Use filenames that sort in the order you want the slides to appear (e.g. `01.png`, `02.png`, ...).

---

## Usage

Run the main script:

```bash
python main.py
```

The application will open a window titled **Slides** showing the current slide with a small webcam preview.

Press `q` to quit.

---

## Gesture Controls (as implemented in `main.py`)

> The project uses `cvzone.HandDetector` which returns a `fingers` list in the order `[thumb, index, middle, ring, pinky]`.

* **Previous slide** — *Thumb up* (`[1,0,0,0,0]`) when your hand center is above the gesture threshold line.
* **Next slide** — *Pinky up* (`[0,0,0,0,1]`) when your hand center is above the gesture threshold line.
* **Pointer (visual)** — *Index + Middle fingers up* (`[0,1,1,0,0]`) draws a small circle to indicate pointer position on the slide.
* **Draw / Annotate** — *Index finger up only* (`[0,1,0,0,0]`) starts/continues drawing freehand annotations on the slide.
* **Undo last annotation** — *Index + Middle + Ring fingers up* (`[0,1,1,1,0]`) removes the last annotation stroke.

**Note:** Navigation gestures are only recognized when your hand's center `cy` is above the `gestureThreshold` line (a horizontal line near the top of the window). This prevents accidental slide changes while annotating.

---

## Configuration (quick reference)

You can tweak parameters at the top of `main.py`:

```python
width, height = 1280, 720       # camera resolution used by the script
gestureThreshold = 300          # vertical pixel line to enable slide navigation
folderPath = "Presentation"     # folder where slides are stored
hs, ws = 120, 213               # size of the small webcam preview overlay
delay = 30                      # cooldown frames to avoid rapid repeated actions
```

If your webcam is not `0`, change the `cv2.VideoCapture(0)` argument to the correct camera index.

---

## Troubleshooting

* **No camera / blank window**: Ensure your webcam is connected and accessible. Try `cap = cv2.VideoCapture(0)` -> change `0` to `1` or other indices if you have multiple cameras.
* **Hands not detected**: Improve lighting, move your hand more clearly into the camera frame, or increase `detectionCon` in `HandDetector(detectionCon=0.8, ...)` to make detection stricter/looser.
* **Slides don't appear**: Make sure the `Presentation` folder exists in the project root and contains image files. Filenames influence order — prefix with numbers to force ordering.
* **Performance issues / lag**: Reduce the camera resolution (`width`, `height`) or run on a machine with better CPU/GPU support.

---

## Contributing

Contributions are welcome! Open an issue or submit a pull request with a clear description of changes and any testing notes.

---

If you want, I can also:

* Generate a `requirements.txt` for you.
* Convert annotations to permanent overlays saved per slide.
* Add keyboard fallback controls or a CLI for configuration.

Happy presenting! 🎤
