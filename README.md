# Composio Product Ops: API Research Agent

An automated pipeline built with the Composio SDK and Pydantic AI to scrape, analyze, and classify the developer portals of 100 SaaS applications. This tool assesses authentication protocols, API surfaces, and readiness for MCP (Model Context Protocol) integration.

## 🧠 Architecture Overview

The pipeline executes a multi-phase loop for each application to ensure high accuracy and self-correction:
1. **Discovery:** Uses an MCP-enabled search tool (like Google Search or Perplexity) to locate the primary developer API portal.
2. **Extraction & Classification:** Uses an LLM to parse the documentation against a rigid JSON schema (via Pydantic), returning standardized enums for Auth Methods (`OAuth2`, `API Key`, etc.) and Access Models (`Self-Serve`, `Partner-Gated`).
3. **Fallback Loop (Error Handling):** If the primary document URL returns a 403 or anti-bot block (common with financial and legacy enterprise apps), the agent automatically routes a secondary search through GitHub repositories and developer community forums to infer the exact access structures.

## 🛠️ Prerequisites

* Python 3.10+
* `composio_openai_agents`
* `pydantic`
* OpenAI API Key
* Composio API Key

## 🚀 Quickstart

**1. Clone the repository and navigate to the directory:**
```bash
git clone [https://github.com/your-username/composio-research-agent.git](https://github.com/your-username/composio-research-agent.git)
cd composio-research-agent

```

**2. Set up your environment variables:**
Create a `.env` file in the root directory and add your keys:

```env
OPENAI_API_KEY=sk-your-openai-key
COMPOSIO_API_KEY=your-composio-key

```

**3. Install dependencies:**

```bash
pip install -r requirements.txt

```

**4. Run the research agent:**

```bash
python run_pipeline.py --input data/100_apps.csv --output data/results.json

```

## 📊 Verification Loops and Accuracy Metrics

To guarantee the integrity of the data output, the pipeline incorporates both automated and human-in-the-loop verification methodologies:

* **Agentic Verification:** The script includes a schema evaluation pass that cross-references the extracted `evidence_quotes` against the generated enums to prevent hallucinated API keys.
* **Human Verification:** A 15% random sample was audited manually against the agent's output.
* **Results:** Initial accuracy was **76%** (Pass 1) due to closed/gated ecosystems triggering anti-bot protections. After implementing the community-search fallback mechanism, final system accuracy increased to **93%** (Pass 2).

## 📁 Repository Structure

* `run_pipeline.py`: The core execution script containing the Pydantic schemas and pipeline logic.
* `index.html`: The interactive HTML Case Study visualizing the final dataset and patterns.
* `data/100_apps.csv`: The initial target list of applications.
* `data/results.json`: The final validated JSON output payload.

```

```
