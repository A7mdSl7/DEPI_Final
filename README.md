# DEPI_Final — Real-Time Sign Language Translator

**DEPI_Final** is a project that aims to provide **real-time translation of sign language** from webcam video feed to text. It leverages ML / Deep Learning, computer vision, and a WebSocket-based backend to process frames, predict gestures, and return predictions to a frontend client.

## 🔎 Overview

- Converts real-time video feed from user’s webcam into sign language landmarks, then feeds those landmarks into a trained model to generate predictions.
- Offers a **client-side frontend** (HTML + JS) for live webcam feed and real-time display of predictions/translated characters.
- Includes a **backend server** (Python, FastAPI / WebSocket) that accepts frames via WebSocket, does processing (landmarks → model → prediction), and sends predictions back to the client.
- Designed to support deployment: frontend can be hosted e.g. via GitHub Pages; backend can run locally or on server; option to expose backend via tunneling (e.g. Ngrok) so frontend can connect remotely.

## 📁 Repository Structure

```
    DEPI_Final/
    │
    ├─ Models/ ← Contains pretrained models and_classes
    │ ├─ asl_landmarks_final.h5
    │ ├─ asl_landmarks_best.h5
    │ └─ asl_landmarks_classes.pkl
    │
    ├─ WebApp/ ← Frontend code (HTML + JS + CSS)
    │ └─ index.html ← Client-side interface (webcam feed, UI)
    │
    ├─ server.py ← WebSocket backend server (FastAPI)
    ├─ mock_ws_server.py ← Optional: mock WebSocket server that sends random predictions
    ├─ requirements / pyproject.toml ← Dependencies and environment configuration
    └─ README.md ← This documentation file
```

## 🛠️ Setup & Run

### Prerequisites

- Python 3.12+
- Virtual environment tool (e.g., `uv`, `venv`, or `conda`)
- `pip` / `uv sync` to install dependencies
- Webcam (for live feed)
- (Optional) Ngrok — if you want to expose backend to internet for remote frontend usage

### Installation & Running Locally

1. Clone your fork of the repo:

   ```bash
    git clone https://github.com/YourUsername/DEPI_Final.git
    cd DEPI_Final

   ```

2. Create & activate virtual environment, then install dependencies:

   `uv sync    # or pip install -r requirements.txt if you have that file`

3. Start backend server:

- For real model:
  `uv run uvicorn server:app --host 0.0.0.0 --port 8080 --reload`

- Or run mock server (for testing frontend without ML model):
  `python mock_ws_server.py`

4. Open the client:

- Either open `WebApp/index.html` locally (double-click or `file://...`) — not recommended for cross-origin requests.

- Recommended: host frontend (e.g. via GitHub Pages) and point it to backend server URL (with WebSocket).

### (Optional) Expose Backend via Ngrok

If backend runs on localhost and you want frontend (hosted elsewhere) to connect:
`ngrok http 8080`

Copy the generated public URL (e.g. `https://xyz123.ngrok-free.app`) and in `WebApp/index.html`, set:
`const WEBSOCKET_URL = "wss://xyz123.ngrok-free.app/sign-model";`

Then deploy or open frontend and it will connect to backend via Ngrok.

### 🚀 Deployment (Frontend + Backend)

- Frontend:

  - Copy `WebApp/index.html` (or entire WebApp folder) into `docs/` directory at root, then enable GitHub Pages for branch `main`, `folder` `/docs`.

  - After push, site will be available at `https://<YourUsername>.github.io/DEPI_Final/`

- Backend:

  - Run server as described above (on your machine or a remote VM).

  - If remote — ensure public address or reverse-proxy; or use Ngrok to create secure tunnel.

  - Update `WEBSOCKET_URL` in frontend accordingly.

### ✔️ What works / What to know

- ✅ Real-time webcam capture & streaming via browser
- ✅ Landmark detection + ML model inference (provided the `Models/` files are present)
- ✅ Prediction display on frontend (live, per frame)
- ✅ Mock server support — useful for UI testing without running ML backend
- ⚠️ Real model predictions depend on correct frame preprocessing and model input shape. If results are random or incorrect, check:
  - Model loading path
  - Preprocessing steps (resizing, normalization, landmarks extraction)
  - That webcam feed resolution matches model expectations

### 📚 Technologies & Libraries Used

- Python (backend)
- FastAPI & WebSockets — server-side realtime communication
- TensorFlow — deep learning model for gesture recognition
- MediaPipe — hand / pose landmark detection from webcam frames
- Frontend: HTML, JavaScript, CSS (with Tailwind CSS)
- Optional: tunneling via ngrok for public access

### 🎯 How to Use

1. Launch backend server (or mock server).
2. Set `WEBSOCKET_URL` in frontend to match backend (localhost or Ngrok URL).
3. Open frontend page (locally or hosted).
4. Allow camera access — webcam feed will show.
5. Perform sign language gestures — predictions (or mock predictions) will appear live.

### 🧑‍💻 Contributing

Contributions are welcome! Some ideas:

- Improve model — retrain with more data, better accuracy
- Add functionality: full-word / sentence recognition instead of just character-by-character
- Add UI enhancements: better layout, mobile support, accessibility
- Add features: logging, saving sessions, exporting translated text
  To contribute:

`git fork
git checkout -b feature/<your-feature-name>
make changes
git commit -m "Add <feature>"
git push origin feature/<your-feature-name>

# then open Pull Request

`

### 📄 License & Credits

- Built by:

  - Samy Adel
  - Mahmoud Elsherbiny
  - Mazen Arafat
  - Ahmed Salah
  - Fouad Ramzy
  - Youssef Mustafa
  - Kirollos Safwat

- Uses open-source libraries: TensorFlow, FastAPI, MediaPipe, etc.

- Feel free to reuse, extend, adapt — with proper acknowledgment 👍
