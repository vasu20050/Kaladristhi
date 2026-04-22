# 🎭 KALADRISHTI: Preserving Heritage through Vision

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![MediaPipe](https://img.shields.io/badge/AI-MediaPipe-blue.svg)](https://mediapipe.dev/)
[![JavaScript](https://img.shields.io/badge/Language-Vanilla%20JS-F7DF1E.svg)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

**Kaladrishti** is a premium, AI-powered educational platform dedicated to the preservation and teaching of Indian classical and folk dance traditions. By leveraging cutting-edge computer vision through Google's **MediaPipe Holistic**, the application provides real-time feedback on posture, gestures (mudras), and expressions (abhinaya).
---

## ✨ Key Features

- **🛡️ 47+ Dance Traditions** - A vast catalogue spanning Classical, Folk, Tribal, Martial, and Ritual forms across India.
- **👁️ Real-time AI Tracking** - Body skeleton, hand landmarks, and facial expression analysis using MediaPipe.
- **🎓 Comprehensive Curricula** - Each dance form features a structured 4-lecture learning path with "Guru's Notes."
- **📊 Performance Analytics** - Real-time posture scoring, stability index, and joint-level feedback.
- **🎭 Abhinaya (Expression) Detection** - AI-assisted analysis of facial emotions essential for classical storytelling.
- **📱 Premium Responsive UI** - A "Temple at Dusk" glassmorphic design optimized for both desktop and tablet experiences.
- **🔒 Privacy First** - All AI processing happens locally in the browser; video data never leaves your device.

---

## 🏛️ The Dance Catalogue

Kaladrishti categorizes India's rich heritage into five primary streams:

### 1. Classical (Sangeet Natak Akademi Recognized)
*   **Bharatanatyam (Tamil Nadu)**: Precise footwork & mudras.
*   **Kathak (North India)**: Rhythmic turns & storytelling.
*   **Odissi (Odisha)**: Graceful Tribhanga & Chouka.
*   **Kuchipudi (Andhra Pradesh)**: Dynamic stances & brass plate sequence.
*   **Kathakali (Kerala)**: Powerful character archetypes & eye movements.
*   **Mohiniyattam (Kerala)**: Soft swaying "Lasya" movements.
*   **Manipuri (Manipur)**: Gentle circular Ras Leela patterns.
*   **Sattriya (Assam)**: Devotional monastic traditions.

### 2. Folk & Regional
*   *Garba, Bhangra, Lavani, Bihu, Ghoomar, Dandiya Raas, Rouf, Dollu Kunitha, Fugdi, Thiruvathira, and more.*

### 3. Tribal Traditions
*   *Kalbelia, Cheraw (Bamboo Dance), Dumhal, Hojagiri, Siddi Dhamal, Santhali, Wangala, Laho.*

### 4. Martial & Semi-Classical
*   *Chhau, Yakshagana, Gotipua, Thang-Ta, Kalaripayattu, Perini Sivatandavam, Paika.*

### 5. Ritualistic Forms
*   *Theyyam, Padayani, Puli Kali.*

---

## 🚀 Getting Started

### Prerequisites
- A modern web browser (Chrome or Edge recommended for best MediaPipe performance).
- A webcam for live tracking.
- Python or Node.js (to host the local server).

### Installation & Launch

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-username/KALADRISHTI.git
    cd KALADRISHTI/Kaladristhi
    ```

2.  **Start the Frontend:**
    ```bash
    # Using npm (Shortcut for Python server)
    npm start
    
    # OR directly using Python
    python -m http.server 8000
    ```

3.  **Start the Backend (Optional - for advanced session analytics):**
    ```bash
    npm run backend
    ```

4.  **Access the App:**
    Open [http://localhost:8000](http://localhost:8000) in your browser.

---

## 🛠️ Technology Stack

- **Frontend**: Vanilla JavaScript (ES6+), Tailwind CSS, Lucide Icons.
- **AI/ML**: MediaPipe Holistic (Pose, Hands, Face).
- **Backend**: Node.js (Express) for session verification and metrics aggregation.
- **Data**: MongoDB (Mocked via LocalStorage for offline use).
- **Icons/Styles**: Lucide.js, Google Fonts (Cormorant Garamond & Montserrat).

---

## 💡 How it Works

### 1. Pose Comparison
The AI calculates the spatial relationship between **33 body landmarks**. When you select a "Practice" session, the system compares your live skeleton against the "Ideal" posture defined in our coordinate database.

### 2. Abhinaya Analysis
By tracking **468 facial points**,Kaladrishti detects micro-expressions. This is vital for dance forms like Kathakali and Bharatanatyam, where the face (eyes, eyebrows, mouth) tells the story.

### 3. Visual Feedback
- **Blue Skeleton**: Great alignment.
- **Red Skeleton**: Correction needed at specific joints.
- **Real-time Tips**: Text-based feedback like "Keep back straight" or "Bend knees more."

---

## 📁 Project Structure

```text
Kaladristhi/
├── index.html          # Core Application UI & Navigation
├── package.json        # Dependencies & Scripts
├── backend/
│   └── server.js      # Analytics API (Node.js)
├── media/
│   ├── img/           # Dance asset images
│   └── vid/           # Practice background loops
├── src/
│   └── app.js         # Navigation & UI State management (Planned split)
└── wiki_images.json    # Metadata for the 47+ dance forms
```

---

## 🤝 Contributing

We welcome contributions to expand the catalogue!
- Add more local dance forms.
- Improve the pose-comparison algorithm.
- Translate instructions into regional Indian languages.

---

## 📜 License & Credits

- **License**: MIT
- **MediaPipe**: Developed by Google Research.
- **Artistic Knowledge**: Guided by traditional *Natya Shastra* and *Abhinaya Darpana* texts.

---

**Made with ❤️ for Indian Culture.**  
*Preserving the past, practicing in the present, envisioning the future.* 🎭✨

