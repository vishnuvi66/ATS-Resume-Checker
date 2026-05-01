# 🧠 ATS Resume Analyzer (NLP + ML + Docker)

An **Applicant Tracking System (ATS)** that analyzes resumes against job descriptions to compute a match score, extract skill gaps, and classify resumes into suitable roles using **explainable NLP and ML techniques**.

This project is designed with a focus on **clarity, interpretability, and interview readiness**, not overengineering.

---

## 🚀 Features

- 📄 **PDF Resume Parsing**  
  Extracts text from real resume PDFs using `pdfplumber`.

- 🧹 **NLP Preprocessing**  
  Cleans and normalizes text using spaCy (lowercasing, stopword removal, lemmatization).

- 📊 **Resume–JD Match Score (0–100)**  
  Uses **TF-IDF vectorization + cosine similarity** to measure relevance between resume and job description.

- 🧩 **Skill Gap Analysis**  
  Hybrid **rule-based + regex-based** skill extraction to identify missing skills.

- 🏷️ **Resume Classification**  
  Classifies resumes into one of the following categories:
  - Data
  - Web
  - ML
  - Core CS  
  using **Logistic Regression**.

- 🌐 **Web Interface + API**  
  Minimal frontend with a Flask backend.

- 🐳 **Dockerized Deployment**  
  Fully containerized for consistent execution across environments.

---

## 🧠 System Architecture

```text
Resume (PDF)                Job Description (Text)
       ↓                               ↓
PDF Text Extraction            Text Input
       ↓                               ↓
          NLP Preprocessing (spaCy)
                     ↓
            TF-IDF Vectorization
                     ↓
            Cosine Similarity  ────→  Match Score
                     ↓
            Skill Extraction   ────→  Missing Skills
                     ↓
            Resume Classifier  ────→  Category
```


---

## 🧠 ML & NLP Techniques Used

| Component | Technique | Reason |
|--------|----------|--------|
| Text Representation | TF-IDF | Fast, interpretable, ATS-relevant |
| Similarity Measure | Cosine Similarity | Length-independent relevance |
| Classification | Logistic Regression | Lightweight and explainable |
| Skill Extraction | Keywords + Regex | Deterministic and controllable |

❌ No Transformers  
❌ No BERT  
❌ No Deep Learning  

**Reason:** Classical NLP methods are sufficient, faster, and easier to explain for ATS-style document matching problems.

---

## 📁 Project Structure

```text
ATS system/
├── src/
│   ├── app.py              # Flask API
│   ├── pdf_parser.py       # PDF text extraction
│   ├── text_cleaner.py     # NLP preprocessing
│   ├── skill_extractor.py  # Rule-based skill extraction
│   ├── matcher.py          # Match score logic
│   └── classifier.py       # Resume classification (inference)
│
├── models/
│   ├── classifier.pkl
│   └── vectorizer.pkl
│
├── Data/
│   ├── Resumes/
│   └── job_descriptions/
│
├── static/                 # Frontend (HTML/CSS/JS)
├── requirements.txt
├── Dockerfile
└── README.md
```
---

## ⚙️ How to Run (Docker – Recommended)

### 1️⃣ Build the Docker image
```bash
docker build -t ats-system .
```

### 2️⃣ Run the container
```bash
docker run -p 5000:5000 ats-system
```

### 3️⃣ Open in browser
```
http://localhost:5000
```

---

## ⚙️ How to Run (Local – Optional)
```bash
pip install -r requirements.txt
python -m spacy download en_core_web_sm
python src/app.py
```

---

## 🧪 Example Output
```json
{
  "match_score": 72.45,
  "resume_category": "Data",
  "matched_skills": ["python", "sql", "pandas"],
  "missing_skills": ["docker"]
}
```

---

## 🧠 Key Design Decisions

**Why TF-IDF instead of deep learning models?**  
Classical NLP methods are faster, interpretable, and sufficient for resume–JD matching tasks.

**Why rule-based skill extraction?**  
Skills are deterministic entities; rule-based systems provide better control and explainability.

**Why train the classifier offline?**  
Models are trained once, persisted, and loaded at runtime to reduce latency and ensure consistent inference.

**Why Docker?**  
Docker ensures consistent behavior across environments and eliminates dependency issues.

---

## ⚠️ Limitations

- Does not handle scanned or image-only PDFs (OCR not included).
- Uses a small, representative corpus for resume classification (prototype-focused).
- Designed as a single-service application (no authentication or database).