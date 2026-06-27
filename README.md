# MarkClass 📸

**MarkClass** is an AI-powered, multimodal attendance system that makes marking class attendance faster and effortless. Instead of manual roll calls or sign-in sheets, MarkClass uses **face recognition**, **QR-code-based student registration**, and **voice detection** to automatically identify and log students in real time — all from a simple Streamlit web app.

> Making Attendance faster using AI.

🔗 **Live app:** [markclass-main.streamlit.app](https://markclass-main.streamlit.app/)

---

## ✨ Features

- **QR-code student registration** — each student gets a unique QR code generated at registration time, which can be scanned to quickly identify/check-in the student without manual data entry.
- **Face recognition–based attendance** — detects faces from a live camera feed and classifies/matches them against registered students using a trained **SVC (Support Vector Classifier)** model.
- **Voice detection** — adds a secondary, multimodal verification signal alongside face recognition for more reliable identification.
- **Automated attendance logging** — recognized/verified students are marked present automatically, removing manual registers.
- **Streamlit web interface** — lightweight, browser-based UI with no separate install needed beyond Python.

---

## 🧠 How It Works

### 1. Student Registration
- A new student is registered through the app by capturing their face samples (and/or basic details).
- Face samples are converted into **facial embeddings** (numerical feature vectors representing facial structure).
- These embeddings are used to train (or update) a **Support Vector Classifier (SVC)** — a supervised ML model that learns to draw decision boundaries between different students' face-embedding clusters, so it can classify "whose face is this?" at inference time.
- A **unique QR code** is generated for the student at registration. This QR code acts as a fast, reliable identity token — useful as a fallback or quick-checkin method when scanning a face isn't convenient (e.g. partially obscured face, poor lighting), or simply to speed up the check-in flow.

### 2. Attendance Session
- During a class session, the camera feed is captured frame-by-frame.
- Faces detected in each frame are converted into embeddings and passed to the trained **SVC classifier**, which predicts the most likely matching student identity (with a confidence/probability score).
- If a **QR code** is shown/scanned instead, the app reads the encoded student ID directly — a deterministic, near-instant alternative to face matching.
- **Voice detection** can be used as an additional check — e.g. confirming a verbal roll-call response — to cross-verify presence alongside the visual signal, reducing false positives from photos or look-alikes.

### 3. Logging
- Once a student is identified (via face match, QR scan, or voice confirmation), their attendance is automatically recorded for that session — no manual marking required.

---

## 🎯 Why an SVC (not a deep classifier)?

For face recognition, MarkClass doesn't train a deep neural net from scratch per student. Instead:
- A pretrained face embedding model converts each face image into a fixed-length feature vector.
- A lightweight **SVC** is then trained on top of these embeddings to classify *which student* a given face belongs to.

This approach is fast to retrain whenever a new student registers (you only need to refit the SVC on the updated embedding set, not retrain a deep network), and works well even with a relatively small number of face samples per student — which matters a lot for a classroom-sized dataset.

---

## 📁 Project Structure

```
MARK-CLASS/
├── app.py               # Streamlit app entry point — registration, attendance session, UI flow
├── src/                  # Core logic:
│                         #   - Face embedding extraction
│                         #   - SVC training/inference for face classification
│                         #   - QR code generation (registration) & QR scanning (check-in)
│                         #   - Voice detection / verification
│                         #   - Attendance logging
├── .streamlit/            # Streamlit configuration
├── .devcontainer/          # Dev container config for consistent local setup
└── requirements.txt        # Python dependencies
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.9+
- A webcam (for face capture/recognition)
- A microphone (if using voice detection)
- A way to display/scan QR codes (e.g. phone camera or a second device, depending on how the QR flow is implemented)

### Installation

```bash
git clone https://github.com/Ayushgit51/MARK-CLASS.git
cd MARK-CLASS
pip install -r requirements.txt
```

### Running locally

```bash
streamlit run app.py
```

Then open the local URL Streamlit provides:

1. **Register** a new student — capture face samples, generate their QR code.
2. **Start a session** — recognized students are detected via face match or QR scan and marked present in real time.
3. Optionally use **voice detection** to confirm/verify presence alongside the visual check.

---

## 🛠️ Tech Stack

- **Python** — core application logic
- **Streamlit** — web UI and app framework
- **Face recognition / embedding extraction** — converts faces into feature vectors
- **Scikit-learn SVC (Support Vector Classifier)** — classifies face embeddings to identify students
- **QR code generation & scanning** — fast registration and check-in flow
- **Voice detection** — secondary signal for multimodal verification

---

## 👤 Author

**Ayush** — AI/ML Engineer | Building practical, story-driven AI projects
- GitHub: [@Ayushgit51](https://github.com/Ayushgit51)
