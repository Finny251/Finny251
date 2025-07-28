
# 📘 Intelligent PDF Analyzer for Investment Research

> 🔍 **Submission for Adobe 'Connecting the Dots' Hackathon — Round 2**

This project is a backend system that analyzes a collection of PDFs and extracts the most relevant sections based on a given **persona** and **job-to-be-done**. It is tailored to help users like investment analysts quickly access critical insights buried in large documents.

---

## 🧠 Problem Statement

In professional environments, experts often deal with lengthy documents without clear structure or navigation. This project enables machines to:
- Interpret document structure (headings and hierarchy)
- Identify and rank content relevant to the user's role and goal
- Summarize it for quick understanding

---

## 👤 Use Case

**Persona**: Investment Analyst  
**Goal**: Analyze trends in R&D spending across companies

---

## 🛠️ How It Works (Architecture Overview)

The solution follows a modular pipeline:

### 📑 1. Outline Extraction (Extractor Module)
- Library: `PyMuPDF`
- Function: Extracts the title and hierarchical headings (H1, H2, H3) from each PDF.
- Output: JSON structure of the document's outline with page numbers.

### 🔎 2. Semantic Relevance Ranking (Ranker Module)
- Library: `sentence-transformers`
- Model: `all-MiniLM-L6-v2`
- Function:
  - Converts job description and all extracted headings into semantic embeddings
  - Uses cosine similarity to rank relevance
  - Selects the top N headings most relevant to the persona's goal

### 🧠 3. Paragraph Extraction
- Tool: `PyMuPDF`
- Function: Extracts full text from the relevant pages associated with ranked headings

### ✍️ 4. NLP Summarization (Summarizer Module)
- Library: `transformers`
- Model: `t5-small`
- Function:
  - Uses encoder-decoder architecture to generate abstractive summaries
  - Helps condense dense content into a few key lines

---

## 📦 Output

The final output is a single JSON file that contains:
- Persona and job
- Timestamp
- Ranked and summarized sections (title, page, relevance score, and refined text)

### Sample Output
```json
{
  "persona": "Investment Analyst",
  "job_to_be_done": "analyze trends in R&D spending across companies",
  "processing_timestamp": "2025-07-23",
  "extracted_sections": [
    {
      "document": "inv.pdf",
      "page_number": 4,
      "heading": "Research and Development Expenditure",
      "score": 0.88,
      "refined_text": "This section discusses the recent increase in R&D investment across major firms..."
    }
  ]
}
```

---

## 📁 Folder Structure

```
pdf-intelligence/
│
├── main.py                  # Main orchestrator
├── config.json              # Contains persona + job
├── requirements.txt         # Python dependencies
├── Dockerfile               # Containerization
│
├── input/                   # Input PDFs
├── output/                  # Final JSONs
│
└── utils/                   # Modular components
    ├── extractor.py         # Heading extractor
    ├── ranker.py            # Embedding-based relevance scoring
    └── summarizer.py        # T5 summarizer
```

---

## 🐳 Run with Docker

### 1. Build the image:

```bash
docker build -t pdfintelligence:submission .
```

### 2. Run the container:

```bash
docker run --rm -v "%cd%/input:/app/input" -v "%cd%/output:/app/output" --network none pdfintelligence:submission
```

---

## ⚙️ Technologies Used

| Component       | Technology / Library                |
|----------------|-------------------------------------|
| PDF Parsing     | PyMuPDF (`fitz`)                    |
| Embeddings      | `sentence-transformers`, MiniLM     |
| NLP Model       | `transformers`, `t5-small`          |
| Summarization   | Abstractive via encoder-decoder     |
| Ranking         | Cosine similarity in vector space   |
| Containerization| Docker (Python 3.10-slim)           |

---

## 🧪 Tested On

- ✔️ Windows 10 + Docker Desktop
- ✔️ Python 3.10 (in Docker)
- ✔️ Sample investment domain PDFs

---

## 🙌 Author

Built with purpose and passion for Adobe Hackathon  
By: **Finny Balagam**
