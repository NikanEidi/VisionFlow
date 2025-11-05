```
╔════════════════════════════════════════════════════════════════════════════╗
║                                                                            ║
║                            ██╗   ██╗██╗███████╗██╗ ██████╗ ███╗   ██╗      ║
║                            ██║   ██║██║██╔════╝██║██╔═══██╗████╗  ██║      ║
║                            ██║   ██║██║███████╗██║██║   ██║██╔██╗ ██║      ║
║                            ╚██╗ ██╔╝██║╚════██║██║██║   ██║██║╚██╗██║      ║
║                             ╚████╔╝ ██║███████║██║╚██████╔╝██║ ╚████║      ║
║                              ╚═══╝  ╚═╝╚══════╝╚═╝ ╚═════╝ ╚═╝  ╚═══╝      ║
║                                                                            ║
║                      ███████╗██╗      ██████╗ ██╗    ██╗                   ║
║                      ██╔════╝██║     ██╔═══██╗██║    ██║                   ║
║                      █████╗  ██║     ██║   ██║██║ █╗ ██║                   ║
║                      ██╔══╝  ██║     ██║   ██║██║███╗██║                   ║
║                      ██║     ███████╗╚██████╔╝╚███╔███╔╝                   ║
║                      ╚═╝     ╚══════╝ ╚═════╝  ╚══╝╚══╝                    ║
║                                                                            ║
║                                                                            ║
║                                                                            ║
╚════════════════════════════════════════════════════════════════════════════╝
```

> **Vision Flow** is an end-to-end AI automation system that autonomously generates tailored, ATS-optimized resumes for any job posting.  
> Built entirely on **n8n**, **OpenRouter (GPT-4o-mini)**, and **Google Drive**, it orchestrates the full pipeline — from job data input to a ready-to-use PDF resume.

---

## Overview

Vision Flow transforms fragmented job information into a complete professional document using a chain of intelligent automation nodes.  
It performs data normalization, AI-driven resume generation, HTML templating, PDF rendering, and cloud storage management — all inside n8n.

```
┌──────────────────────────────────────────────────────────────────────┐
│                        INPUT FLEXIBILITY                             │
├──────────────────────────────────────────────────────────────────────┤
│  ► A single job title                                                │
│  ► A full job description                                            │
│  ► A link to a job posting                                           │
│  ► Company name or position only                                     │
└──────────────────────────────────────────────────────────────────────┘
```

Regardless of the provided data, Vision Flow intelligently infers missing context and generates a full, structured resume.

---

## Core Capabilities

```
╔═══════════════════╦════════════════════════════════════════════════════════╗
║ LAYER             ║ FUNCTION                                               ║
╠═══════════════════╬════════════════════════════════════════════════════════╣
║                   ║                                                        ║
║ Input Layer       ║ Captures job details through a form or webhook and     ║
║                   ║ normalizes inconsistent data.                          ║
║                   ║                                                        ║
╠═══════════════════╬════════════════════════════════════════════════════════╣
║                   ║                                                        ║
║ AI Layer          ║ Uses OpenRouter's GPT-4o-mini to compose an            ║
║                   ║ ATS-optimized JSON resume draft.                       ║
║                   ║                                                        ║
╠═══════════════════╬════════════════════════════════════════════════════════╣
║                   ║                                                        ║
║ Template Layer    ║ Converts the AI output into a dynamic,                 ║
║                   ║ well-structured HTML resume.                           ║
║                   ║                                                        ║
╠═══════════════════╬════════════════════════════════════════════════════════╣
║                   ║                                                        ║
║ Document Layer    ║ Generates a styled, print-ready PDF via                ║
║                   ║ Gothenburg's HTML-to-PDF engine.                       ║
║                   ║                                                        ║
╠═══════════════════╬════════════════════════════════════════════════════════╣
║                   ║                                                        ║
║ Storage Layer     ║ Automatically detects or creates company-specific      ║
║                   ║ folders in Google Drive and uploads the final PDF.     ║
║                   ║                                                        ║
╚═══════════════════╩════════════════════════════════════════════════════════╝
```

---

## Architecture

```
                         ┌───────────────────┐
                         │   Form Trigger    │
                         └─────────┬─────────┘
                                   │
                                   ▼
                         ┌───────────────────┐
                         │ Normalize Query   │
                         │  (Data Cleaner)   │
                         └─────────┬─────────┘
                                   │
                                   ▼
                         ┌───────────────────┐
                         │     Profile       │
                         │ (Static+Dynamic)  │
                         └─────────┬─────────┘
                                   │
                                   ▼
                ┌──────────────────┴─────────────────┐
                │                                    │
                ▼                                    ▼
    ┌───────────────────┐              ┌────────────────────┐
    │  Build AI Body    │─────────────▶│ AI Customize Resume│
    └───────────────────┘              │    (OpenRouter)    │
                                       └──────────┬─────────┘
                                                  │
                                                  ▼
                                       ┌────────────────────┐
                                       │   Merge (AI +      │
                                       │     Context)       │
                                       └──────────┬─────────┘
                                                  │
                                                  ▼
                                       ┌────────────────────┐
                                       │  Extract AI JSON   │
                                       └──────────┬─────────┘
                                                  │
                                                  ▼
                                       ┌────────────────────┐
                                       │    Build HTML      │
                                       └──────────┬─────────┘
                                                  │
                                                  ▼
                                       ┌────────────────────┐
                                       │   HTML → PDF       │
                                       │   (Gothenburg)     │
                                       └──────────┬─────────┘
                                                  │
                                                  ▼
                                       ┌────────────────────┐
                                       │   Tag PDF Meta     │
                                       └──────────┬─────────┘
                                                  │
                                                  ▼
                                       ┌────────────────────┐
                                       │  CompanyExists?    │
                                       └─────┬────────┬─────┘
                                             │        │
                                ┌────────────┘        └────────────┐
                                ▼                                  ▼
                      ┌──────────────────┐              ┌──────────────────┐
                      │   Pick Folder    │              │  Create Folder   │
                      └────────┬─────────┘              └────────┬─────────┘
                               │                                 │
                               └──────────────┬──────────────────┘
                                              ▼
                                   ┌────────────────────┐
                                   │       Merge        │
                                   └──────────┬─────────┘
                                              │
                                              ▼
                                   ┌────────────────────┐
                                   │ Ensure Binary      │
                                   │     Exists         │
                                   └──────────┬─────────┘
                                              │
                                              ▼
                                   ┌────────────────────┐
                                   │   Upload PDF       │
                                   │  (Google Drive)    │
                                   └────────────────────┘
```

**Each node performs a specific ETL or orchestration function, making the workflow modular, testable, and fault-tolerant.**

---

## Intelligent Behaviors

```
╔═══════════════════════════════════════════════════════════════════════════╗
║                                                                           ║
║  ┌─ FLEXIBLE INPUT HANDLING ───────────────────────────────────────┐      ║
║  │ Works even if the user only provides a job title or company.    │      ║
║  └─────────────────────────────────────────────────────────────────┘      ║
║                                                                           ║
║  ┌─ FALLBACK LOGIC ────────────────────────────────────────────────┐      ║
║  │ Automatically infers missing sections such as objectives,       │      ║
║  │ highlights, or skills.                                          │      ║
║  └─────────────────────────────────────────────────────────────────┘      ║
║                                                                           ║
║  ┌─ STRICT JSON PARSING ───────────────────────────────────────────┐      ║
║  │ Ensures model output is valid and structured for downstream     │      ║
║  │ nodes.                                                          │      ║
║  └─────────────────────────────────────────────────────────────────┘      ║
║                                                                           ║
║  ┌─ DYNAMIC FOLDER MANAGEMENT ─────────────────────────────────────┐      ║
║  │ Creates or reuses company-specific folders in Google Drive      │      ║
║  │ automatically.                                                  │      ║
║  └─────────────────────────────────────────────────────────────────┘      ║
║                                                                           ║
║  ┌─ ERROR-RESILIENT EXECUTION ─────────────────────────────────────┐      ║
║  │ Uses try/catch and conditional filters to prevent workflow      │      ║
║  │ crashes.                                                        │      ║
║  └─────────────────────────────────────────────────────────────────┘      ║
║                                                                           ║
║  ┌─ BINARY-SAFE FILE HANDLING ─────────────────────────────────────┐      ║
║  │ Verifies and passes only valid PDF binaries to the uploader     │      ║
║  │ stage.                                                          │      ║
║  └─────────────────────────────────────────────────────────────────┘      ║
║                                                                           ║
╚═══════════════════════════════════════════════════════════════════════════╝
```

---

## Technical Highlights

```
╔══════════════════╦════════════════════════════════════════════════════════╗
║ Platform         ║ n8n (no-code automation engine)                        ║
╠══════════════════╬════════════════════════════════════════════════════════╣
║ Language         ║ JavaScript (Code Nodes)                                ║
╠══════════════════╬════════════════════════════════════════════════════════╣
║ AI Model         ║ OpenRouter GPT-4o-mini                                 ║
╠══════════════════╬════════════════════════════════════════════════════════╣
║ Storage          ║ Google Drive API                                       ║
╠══════════════════╬════════════════════════════════════════════════════════╣
║ Renderer         ║ Gothenburg HTML-to-PDF                                 ║
╠══════════════════╬════════════════════════════════════════════════════════╣
║ Workflow Type    ║ Event-driven, modular, context-preserving              ║
╠══════════════════╬════════════════════════════════════════════════════════╣
║ Error Handling   ║ Structured fallbacks, JSON validation, binary          ║
║                  ║ filtering                                              ║
╚══════════════════╩════════════════════════════════════════════════════════╝
```

---

## Example Flow Summary

```
   ╔═══════════════════════════════════════════════════════════════════╗
   ║                                                                   ║
   ║  ┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓     ║
   ║  ┃  STEP 1: INPUT FORM SUBMISSION                           ┃     ║
   ║  ┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛     ║
   ║  The user submits a job title, company, link, or description.     ║
   ║                                                                   ║
   ╚═══════════════════════════════╦═══════════════════════════════════╝
                                   │
                                   ▼
   ╔═══════════════════════════════════════════════════════════════════╗
   ║                                                                   ║
   ║  ┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓     ║
   ║  ┃  STEP 2: NORMALIZATION                                   ┃     ║
   ║  ┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛     ║
   ║  Raw text is cleaned, deduplicated, and mapped to a consistent    ║
   ║  schema.                                                          ║
   ║                                                                   ║
   ╚═══════════════════════════════╦═══════════════════════════════════╝
                                   │
                                   ▼
   ╔═══════════════════════════════════════════════════════════════════╗
   ║                                                                   ║
   ║  ┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓     ║
   ║  ┃  STEP 3: AI PROCESSING                                   ┃     ║
   ║  ┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛     ║
   ║  A dynamic prompt is constructed and sent to OpenRouter's         ║
   ║  GPT-4o-mini, which returns a structured JSON resume.             ║
   ║                                                                   ║
   ╚═══════════════════════════════╦═══════════════════════════════════╝
                                   │
                                   ▼
   ╔═══════════════════════════════════════════════════════════════════╗
   ║                                                                   ║
   ║  ┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓     ║
   ║  ┃  STEP 4: RENDERING & CONVERSION                          ┃     ║
   ║  ┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛     ║
   ║  The AI output is converted into an HTML document and then into   ║
   ║  a PDF.                                                           ║
   ║                                                                   ║
   ╚═══════════════════════════════╦═══════════════════════════════════╝
                                   │
                                   ▼
   ╔═══════════════════════════════════════════════════════════════════╗
   ║                                                                   ║
   ║  ┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓     ║
   ║  ┃  STEP 5: SMART STORAGE                                   ┃     ║
   ║  ┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛     ║
   ║  The PDF is uploaded to the correct Google Drive folder —         ║
   ║  creating one if needed — and tagged with metadata for easy       ║
   ║  retrieval.                                                       ║
   ║                                                                   ║
   ╚═══════════════════════════════════════════════════════════════════╝
```

---

## Reliability & Scalability

```
┌───────────────────────────────────────────────────────────────────────────┐
│                                                                           │
│  ✓  Fully deterministic prompt structure guarantees repeatable results.   │
│                                                                           │
│  ✓  Works seamlessly with n8n's queue or webhook triggers for scaling.    │
│                                                                           │
│  ✓  Designed for single-user.                                             │
│                                                                           │
│  ✓  Can integrate with Sheets, Gmail, or Notion for advanced tracking.    │
│                                                                           │
└───────────────────────────────────────────────────────────────────────────┘
```

---

## Why Vision Flow Matters

```
╔════════════════════════════════════════════════════════════════════════════╗
║                                                                            ║
║  Vision Flow demonstrates how automation, AI, and cloud orchestration      ║
║  can merge into a cohesive, production-grade system without traditional    ║
║  backend infrastructure.                                                   ║
║                                                                            ║
║  It acts as a self-contained AI agent — analyzing input, generating        ║
║  tailored professional content, formatting it, and archiving the           ║ 
║  result — entirely autonomously.                                           ║
║                                                                            ║
╚════════════════════════════════════════════════════════════════════════════╝
```

---

## Author

```
╔════════════════════════════════════════════════════════════════════════════╗
║                                                                            ║
║                              NIKAN EIDI                                    ║
║                  Full-Stack Developer & Systems Engineer                   ║
║                                                                            ║
╠════════════════════════════════════════════════════════════════════════════╣
║                                                                            ║
║    GitHub     │  https://github.com/NikanEidi                              ║
║    Portfolio  │  https://nikanvision.dev                                   ║
║                                                                            ║
╚════════════════════════════════════════════════════════════════════════════╝
```

**Connect:**  
[GitHub](https://github.com/NikanEidi) • [Portfolio](https://nikanvision.dev)

---

## License

```
┌───────────────────────────────────────────────────────────────────────────┐
│                                                                           │
│  This project is proprietary and designed for demonstration purposes      │
│  only. All workflows, prompts, and automation logic are owned by the      │
│  author and not open-sourced.                                             │
│                                                                           │
└───────────────────────────────────────────────────────────────────────────┘
```
