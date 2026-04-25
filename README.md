# kyc-quality-check




<div align="center">

<br/>

<!-- Banner -->
<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0d1117,50:1e3a5f,100:4A9EFF&height=200&section=header&text=ClearCapture&fontSize=56&fontColor=ffffff&fontAlignY=38&desc=Real-time%20KYC%20Document%20Quality%20Scanner&descAlignY=58&descSize=18&descColor=88b4d4" width="100%"/>

<br/>

<!-- Badges -->
![Flutter](https://img.shields.io/badge/Flutter-3.x-02569B?style=for-the-badge&logo=flutter&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.110-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![AWS S3](https://img.shields.io/badge/AWS%20S3-Storage-FF9900?style=for-the-badge&logo=amazons3&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-Audit%20Log-47A248?style=for-the-badge&logo=mongodb&logoColor=white)

<br/>
<br/>

> **Stop rejections before they happen.**
> ClearCapture runs four real-time quality checks while the user is still holding their phone — so a blurry photo, a glare patch, or a cropped corner gets caught at capture, not days later.

<br/>

</div>

---

## 🔍 The Problem

Most KYC submissions fail not because users are careless — but because there is **no feedback at the moment of capture.**

| ❌ Without ClearCapture | ✅ With ClearCapture |
|---|---|
| Photo submitted → sent to server → rejected days later | Problem caught in real-time, before capture |
| Generic error: *"Document rejected"* | Specific guidance: *"Move away from the light"* |
| User re-submits multiple times | Single successful capture, first try |
| High support load, slow onboarding | Faster KYC, lower drop-off rate |

---

## ✨ Key Features

- 📸 **Live camera analysis** — processes frames in real-time as the user holds their phone
- 📊 **Four quality metrics** — each with its own live progress bar and numeric score
- 🔒 **Gated capture button** — stays locked until every threshold passes
- 💬 **Contextual error messages** — *"Hold steady"*, *"Move away from the light"* — never a generic error
- ☁️ **Secure backend** — FastAPI + AWS S3 storage + MongoDB audit log
- 🔎 **Server-side validation** — secondary quality check after upload for full coverage

---

## 📱 App Screens

<div align="center">

| Onboarding | Live Scanner | Review & Submit | Error State |
|:---:|:---:|:---:|:---:|
| ![Screen 1](https://placehold.co/160x300/0d1117/4A9EFF?text=Onboard) | ![Screen 2](https://placehold.co/160x300/0d1117/4A9EFF?text=Scanner) | ![Screen 3](https://placehold.co/160x300/0d1117/22C55E?text=Review) | ![Screen 4](https://placehold.co/160x300/0d1117/F59E0B?text=Glare+Error) |
| Set context, 3-step flow | 4 live quality bars | Score checklist + submit | Specific warning, locked capture |

</div>

---

## 🧠 How It Works

```
┌─────────────────────────────────────────────────────────┐
│                   Flutter Mobile App                    │
│                                                         │
│  Camera Stream → Frame Processor → Quality Engine       │
│                                      │                  │
│              ┌───────────────────────┼──────────┐       │
│              ▼           ▼           ▼          ▼       │
│         Sharpness     Glare      Corners   Contrast     │
│        (Laplacian)  (Overexposed (Edge    (Luminance    │
│         edge filter)  pixels)   density)   std dev)     │
│              │           │           │          │       │
│              └───────────┴───────────┴──────────┘       │
│                          │                              │
│                    Score < threshold?                   │
│                   /              \                      │
│               YES                 NO                    │
│          Show hint            Unlock capture            │
│        Keep locked              button ✓                │
└─────────────────────────────────────────────────────────┘
                          │
                          ▼ on capture
┌─────────────────────────────────────────────────────────┐
│                  Python / FastAPI Backend                │
│                                                         │
│   POST /upload  →  Server-side quality validation       │
│                 →  Store on AWS S3                      │
│                 →  Write audit log to MongoDB           │
└─────────────────────────────────────────────────────────┘
```

---

## ⚙️ Quality Checks

| Check | Algorithm | Threshold | What it catches |
|---|---|---|---|
| **Sharpness** | Laplacian edge filter | ≥ 70 | Motion blur, out-of-focus captures |
| **Glare** | Overexposed pixel ratio | ≥ 80 | Light reflections masking text |
| **Corner visibility** | Edge density mapping | ≥ 75 | Cropped or partially obscured IDs |
| **Contrast** | Luminance standard deviation | ≥ 60 | Low-light, washed-out captures |

> The capture button remains **locked** until all four checks pass simultaneously.

---

## 🏗️ Architecture

```
clearcapture/
│
├── mobile/                  # Flutter app
│   ├── lib/
│   │   ├── camera/          # Camera stream & frame processing
│   │   ├── quality/         # Laplacian, glare, corner, contrast checks
│   │   ├── ui/              # Screens, progress bars, hint messages
│   │   └── api/             # Upload service, HTTP client
│   └── pubspec.yaml
│
└── backend/                 # Python / FastAPI
    ├── main.py              # FastAPI app entry point
    ├── routers/
    │   └── upload.py        # POST /upload endpoint
    ├── services/
    │   ├── quality.py       # Server-side quality validation
    │   ├── storage.py       # AWS S3 upload
    │   └── audit.py         # MongoDB audit logging
    └── requirements.txt
```

---

## 🚀 Getting Started

### Mobile (Flutter)

```bash
# Clone the repo
git clone https://github.com/yourusername/clearcapture.git
cd clearcapture/mobile

# Install dependencies
flutter pub get

# Run on device or emulator
flutter run
```

> Requires Flutter 3.x and a physical or emulated device with camera access.

### Backend (FastAPI)

```bash
cd clearcapture/backend

# Create virtual environment
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# → Add your AWS credentials and MongoDB URI

# Start the server
uvicorn main:app --reload
```

### Environment Variables

```env
AWS_ACCESS_KEY_ID=your_key
AWS_SECRET_ACCESS_KEY=your_secret
AWS_S3_BUCKET=your_bucket_name
AWS_REGION=ap-south-1

MONGODB_URI=mongodb+srv://...
MONGODB_DB=clearcapture
```

---

## 📡 API Reference

### `POST /upload`

Accepts a document image, runs server-side quality validation, stores to S3, and writes an audit record.

**Request**
```http
POST /upload
Content-Type: multipart/form-data

file: <image_file>
user_id: string
doc_type: "passport" | "national_id" | "driving_license"
```

**Response**
```json
{
  "status": "accepted",
  "doc_id": "doc_abc123",
  "s3_url": "https://s3.amazonaws.com/your-bucket/...",
  "quality_scores": {
    "sharpness": 82,
    "glare": 91,
    "corners": 88,
    "contrast": 77
  },
  "audit_id": "audit_xyz789"
}
```

---

## 🛣️ Roadmap

- [x] Real-time sharpness, glare, corner, and contrast checks
- [x] Gated capture button with live feedback
- [x] FastAPI backend with S3 + MongoDB
- [ ] Face liveness detection
- [ ] Multi-document type support (passport, driving license)
- [ ] Offline-first mode with background sync
- [ ] Admin dashboard for audit log review

---

## 🤝 Contributing

Contributions are welcome. Please open an issue first to discuss what you'd like to change.

1. Fork the repo
2. Create your branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m 'add: your feature'`
4. Push: `git push origin feature/your-feature`
5. Open a Pull Request

---

## 📄 License

Nabaraj kc © 2026 — Built for hackathon submission

---

<div align="center">

<br/>

**Built with the belief that a better capture experience = fewer rejections = faster onboarding.**

<br/>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:4A9EFF,50:1e3a5f,100:0d1117&height=100&section=footer" width="100%"/>

</div>
