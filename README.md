# PDF Document Outline Extractor

This project extracts structured outlines (headings with corresponding page numbers) from PDF documents. It is designed to run in an offline, Dockerized environment for portability and reproducibility.

---

## Features

- Detects and extracts hierarchical headings (H1, H2, H3)
- Identifies and skips headers, footers, and TOC pages
- Uses font size, layout, and semantic filtering
- Outputs structured JSON with heading text and page numbers
- Works offline and reproducibly using Docker

---

## Methodology

### 1. PDF Layout Parsing
- Uses `PyMuPDF` to extract fonts, positions, and text blocks
- Analyzes each page to identify potential heading candidates

### 2. Heuristic Scoring
- Scores blocks based on:
  - Font size
  - Bold or uppercase styling
  - Centering and indentation
  - TOC numbering format (e.g., 1.1, 2.3.4)

### 3. Semantic Filtering
- Applies `all-MiniLM-L6-v2` model from `sentence-transformers`
- Filters out full sentences, footers, and unrelated elements
- Clusters similar headings to enforce consistency

### 4. Title Extraction
- Identifies the document title from the first page based on style, size, and position

### 5. Final Output
- Structured JSON with:
  - Hierarchical levels
  - Heading text
  - Page number

---

## Tech Stack

| Tool / Library           | Purpose                                  |
|--------------------------|------------------------------------------|
| PyMuPDF (`fitz`)         | PDF parsing and layout analysis          |
| sentence-transformers    | Semantic filtering with pretrained model |
| scikit-learn, numpy      | Similarity scoring and clustering        |
| Docker                   | Offline, reproducible environment        |

---

## Project Structure

```bash
PDF-Outline-Extractor/
│
├── Dockerfile                      # Docker setup
├── generate_pdf_outline.py        # Main script
├── requirements.txt               # Python dependencies
├── input/                         # Input PDF folder
├── output/                        # Output JSON folder
└── README.md                      # Documentation
```

---

## How to Use

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/PDF-Outline-Extractor.git
cd PDF-Outline-Extractor
```

### 2. Add Your PDFs

Place your input PDF files inside the `input/` folder.

### 3. Build the Docker Image

```bash
docker build -t pdf-outline-extractor .
```

### 4. Run the Container

```bash
docker run --rm -v $(pwd)/input:/input -v $(pwd)/output:/output pdf-outline-extractor
```

---

## Output

The output will be saved as a JSON file in the `output/` directory.

---

## License

MIT License
