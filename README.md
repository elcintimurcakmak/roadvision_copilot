# RoadVision Copilot

An AI-powered traffic analysis system that combines computer vision with LLM-based reasoning to detect vehicles, traffic lights and German traffic signs, look up their meaning under the StVO (Straßenverkehrs-Ordnung), and generate reports.

> WBS Coding School graduation project (Data Science & AI Bootcamp).

## My Contribution

Co-developed this project as the Computer Vision contributor. My main responsibility was designing and integrating the computer vision pipeline for traffic-scene analysis.

### Computer Vision

* YOLO11-based object detection using the COCO-trained model
* Vehicle detection and classification, including cars and trucks
* Traffic-light detection and tracking
* Multi-object tracking and traffic statistics
* Traffic sign detection using a YOLO11 model trained on the GTSDB dataset
* Traffic sign classification using an EfficientNet-B0 model
* Evaluation and validation of detection and classification performance
* Integration of computer vision outputs into the application pipeline

The LLM, RAG, ChromaDB, and report-generation components were developed by my project teammate.

## Overview

RoadVision Copilot processes traffic footage through a computer vision pipeline for vehicle, traffic-light, and traffic-sign detection and tracking. The detected information is then passed to the backend and used by the LLM-based components for StVO-related queries and report generation.

The system:

* Detects and tracks vehicles and traffic lights in traffic footage
* Detects German traffic signs using a YOLO11-based computer vision model
* Classifies detected traffic signs using an EfficientNet-B0 classifier
* Looks up the corresponding StVO regulation for detected signs
* Answers questions about traffic signs and regulations
* Generates structured reports summarizing findings

## Architecture

```text
┌─────────────┐      ┌──────────────────┐      ┌─────────────┐
│  Frontend   │ ───▶ │   Backend (API)  │ ───▶ │  ChromaDB   │
│ (Streamlit) │      │    (FastAPI)      │      │ (RAG store) │
└─────────────┘      └──────────────────┘      └─────────────┘
                             │
                             ▼
                     ┌────────────────────┐
                     │   CV Pipeline      │
                     │                    │
                     │ YOLO11             │
                     │ Object Tracking    │
                     │ Vehicles / Lights  │
                     │ Traffic Signs      │
                     └────────────────────┘
                             │
                             ▼
                     ┌────────────────────┐
                     │ Traffic Sign       │
                     │ Classification     │
                     │ EfficientNet-B0    │
                     └────────────────────┘
                             │
                             ▼
                     ┌────────────────────┐
                     │ Claude Haiku +     │
                     │ Claude Sonnet      │
                     └────────────────────┘
```

**Flow:** Traffic footage → CV pipeline detects and tracks objects → traffic signs are detected and classified → CV results are passed to the backend → backend retrieves StVO information via RAG → LLM generates sign lookups and reports.

## Tech Stack

### Computer Vision

* **YOLO11** — object detection
* **COCO dataset** — vehicle and traffic-light detection
* **GTSDB (German Traffic Sign Detection Benchmark)** — traffic sign detection
* **GTSRB (German Traffic Sign Recognition Benchmark)** — traffic sign detection
* **EfficientNet-B0** — traffic sign classification
* **ByteTrack** — multi-object tracking
* **OpenCV** — video processing and computer vision utilities

### Backend & Frontend

* **FastAPI** — backend API
* **Streamlit** — interactive frontend

### LLM & RAG

* **Claude Haiku** — traffic-sign lookups
* **Claude Sonnet** — report generation
* **LlamaIndex** — RAG orchestration
* **ChromaDB** — vector database
* **rank_bm25** — BM25-based retrieval
* Hybrid retrieval using vector search and BM25

### Infrastructure & Development

* **Docker / Docker Compose** — containerization and service orchestration
* **Ruff** — linting and formatting
* **pre-commit** — automated code quality checks
* **Git / GitHub** — version control and collaborative development

## Project Structure

```text
roadvision_copilot/
├── backend/
│   ├── data/
│   ├── llm/
│   ├── routes/
│   ├── Dockerfile
│   ├── main.py
│   ├── models.py
│   └── requirements.txt
├── frontend/
│   ├── Dockerfile
│   ├── app.py
│   └── requirements.txt
├── data/
├── .env
├── .gitignore
├── docker-compose.yml
├── ruff.toml
├── .pre-commit-config.yaml
└── README.md
```

## Getting Started

### Prerequisites

* Docker & Docker Compose
* An Anthropic API key

### Setup

1. Clone the repository:

   ```bash
   git clone git@github.com:MarvinAtorf/roadvision_copilot.git
   cd roadvision_copilot
   ```

2. Create a `.env` file in the project root with the required variables.

3. Start the application:

   ```bash
   docker compose up -d --build
   ```

4. Access the services:

   * Backend API: `http://127.0.0.1:8000`
   * API documentation (Swagger UI): `http://127.0.0.1:8000/docs`
   * Frontend (Streamlit): `http://127.0.0.1:8501`

### Environment Variables

```env
ANTHROPIC_API_KEY=your_key_here
```

### Health Check

```bash
curl http://127.0.0.1:8000/health
```

## Development

### Linting & Formatting

This project uses **Ruff** for linting and formatting, enforced automatically via pre-commit hooks.

```bash
ruff check .
ruff check . --fix
ruff format .
```

Set up the pre-commit hooks once:

```bash
pre-commit install
```

## Computer Vision Pipeline

The computer vision component consists of several stages:

```text
Traffic Video
      │
      ▼
 YOLO11 Detection
      │
      ├── Vehicles
      │
      ├── Traffic Lights
      │
      └── Traffic Signs
              │
              ▼
       Traffic Sign
       Classification
       (EfficientNet-B0)
              │
              ▼
       Tracking & Statistics
              │
              ▼
        Backend / API
```

The vehicle and traffic-light detection pipeline uses a YOLO11 model trained on the COCO dataset. Object tracking is used to maintain object identities across video frames and derive traffic statistics.

For traffic signs, a YOLO11-based detector trained on the GTSDB dataset identifies sign regions. Detected signs are subsequently classified using an EfficientNet-B0 model.

## Collaboration

This project was developed collaboratively as part of the WBS Coding School Data Science & AI Bootcamp.

Development was coordinated using Git and GitHub, including:

* Feature branches
* Pull requests
* Code reviews
* Merging and integration
* Collaborative debugging and development

My primary contribution was the **Computer Vision pipeline**, while my project teammate developed the **LLM, RAG, ChromaDB, and report-generation components**.

## License

TBD
