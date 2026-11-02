# 🦇 The Consumer Protector: German Consumer Rights RAG Assistant

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![LlamaIndex](https://img.shields.io/badge/LlamaIndex-0.10%2B-purple.svg)](https://www.llamaindex.ai/)
[![Gradio](https://img.shields.io/badge/Gradio-6.3.0-orange.svg)](https://www.gradio.app/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> *"It's not who I am underneath, but what the BGB says I can do that defines me."*

**The Consumer Protector** is a contextual, memory-enabled Retrieval-Augmented Generation (RAG) assistant designed to shield consumers in Germany from unfair commercial practices, unlawful contract traps, and denied statutory warranty claims.

Powered by **LlamaIndex**, **Groq**, and local Hugging Face embeddings, the application blends a dual-layer knowledge base—consisting of a structured, plain-language consumer rights guide and the official 490+ page English translation of the **German Civil Code (*Bürgerliches Gesetzbuch* — BGB)**.

---

## ⚡ Highlights & Features

- **Dual-Layer Knowledge Base**: Pairs everyday practical rules (distance contracts, returns, subscriptions) with statutory legal backing directly from the BGB.
- **Conversational Memory (`ChatMemoryBuffer`)**: Tracks multi-turn dialogue, remembering details (e.g., store purchases vs. online purchases) and resolving pronouns across conversation turns.
- **Interactive WayneTech/Batman-Themed Gradio UI**:
  - Dynamic **Top-$k$ Retrieval Slider** to calibrate retrieval depth in real time.
  - One-click **Chat Export** (`.txt`) for legal audit trails or personal records.
  - Fast-action example queries for rapid demonstrations.
- **Tactical Two-Tier Output Architecture**:
  1. **🛡️ Your Shield (Your Rights)**: Plain, actionable steps on what the consumer can demand, who pays, and what deadlines apply.
  2. **⚖️ WayneTech Dossier (The Governing Law)**: Cites the exact statutory articles (e.g., § 312g, § 437, § 477 BGB) with concise justifications.

---

## 🏛️ System Architecture

```text
[ User Query (Gradio UI) ]
            │
            ▼
[ ChatMemoryBuffer ] ◄── Previews conversation history & context
            │
            ▼
[ VectorStoreIndex Retriever ] ◄── Dynamically configured via Top-K Slider
            │
      ┌─────┴─────────────────────────────────────┐
      │ Matches chunks from:                      │
      │ 1. consumer_rights_summary.txt            │
      │ 2. german_civil_code_bgb.pdf (490+ pages) │
      └─────┬─────────────────────────────────────┘
            │
            ▼ (Top Chunks + System Persona + Chat History)
[ Groq LLM: openai/gpt-oss-120b ]
            │
            ▼
[ Structured Tactical Advice (Gradio Chat Window) ]
```

---

## 🛠️ Tech Stack

- **Core Framework**: [LlamaIndex](https://www.llamaindex.ai/) (`llama-index-core`, `llama-index-readers-file`, `llama-index-llms-openai-like`)
- **LLM Provider**: [Groq](https://groq.com/) via OpenAI-compatible endpoints (`openai/gpt-oss-120b`)
- **Embeddings**: `sentence-transformers/all-MiniLM-L6-v2` (running locally via Hugging Face)
- **Document Parsing**: `pypdf`
- **UI & Deployment**: [Gradio](https://www.gradio.app/)

---

## 🚀 Getting Started

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/consumer-protector-rag.git
cd consumer-protector-rag
```

### 2. Set Up Virtual Environment
```bash
python -m venv venv
# On macOS/Linux:
source venv/bin/activate
# On Windows:
venv\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

*(Or install directly)*:
```bash
pip install llama-index-core llama-index-readers-file llama-index-embeddings-huggingface llama-index-llms-openai-like pypdf gradio python-dotenv
```

### 4. Configure Environment Variables
Create a `.env` file in the project root:
```env
GROQ_API_KEY="your_groq_api_key_here"
HF_TOKEN="your_huggingface_read_token_here"
```

### 5. Launch the Application
Run the notebook or execute the script:
```bash
python app.py
```
Open the generated local URL (`http://127.0.0.1:7860`) in your browser.

---

## 💬 Sample Interaction

**User:** *"My phone stopped working 10 months after buying it from a shop. What can I do?"*

**The Consumer Protector:**

> ### 🛡️ Your Shield (Your Rights)
> - **Demand Free Remedy:** You are legally entitled to demand that the retailer repair the phone or deliver an identical brand-new replacement free of charge. The seller covers all shipping, parts, and labor.
> - **Presumption of Defect:** Because the defect appeared within the first 12 months, the law presumes the phone was already defective when delivered. You do not have to prove what caused the fault.
> - **Claim the Seller, Not the Manufacturer:** Direct your demand to the shop that sold you the device, not the phone's maker.
>
> ### ⚖️ WayneTech Dossier (The Governing Law)
> - **§ 437 BGB (Rights of the Buyer for Defects):** Outlines buyer remedies including subsequent performance, contract cancellation, and price reduction.
> - **§ 439 BGB (Subsequent Performance):** Requires the seller to cover all costs required to remedy the defect.
> - **§ 477 BGB (Reversal of Burden of Proof):** Mandates that any defect appearing within 12 months is presumed to have been present at delivery.
> - **§ 438 BGB (Limitation of Actions):** Sets the statutory defect warranty period to two full years.

---

## 📂 Project Structure

```text
consumer-protector-rag/
├── data/
│   ├── consumer_rights_summary.txt       # Plain-language consumer law reference
│   └── german_civil_code_bgb.pdf         # Official BGB English translation (Federal Ministry)
├── embedding_model/                      # Cached MiniLM embedding weights
├── vector_index/                         # Persisted vector store & docstore JSONs
├── app.py / consumer_protector.ipynb     # Main RAG & Gradio application
├── requirements.txt                      # Project dependencies
└── README.md                             # Documentation
```

---

## ⚖️ Legal Disclaimer

*The Consumer Protector is an AI assistant developed for educational, demonstration, and preliminary informational purposes. It does not constitute formal legal representation, official legal counseling, or an attorney-client relationship under German law.*
