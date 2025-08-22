InsightIQ Talent Match — AI-Powered Hiring Assistant
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
💡 What It Does (Exactly)

Scrapes a live job post URL into plain text.

Extracts structured JSON (role_type, description, skills_required) with a Groq LLM.

Loads candidate portfolios (from a CSV) into ChromaDB for persistent lookup.

Matches job skills to candidate tech stacks and gathers the best-fit portfolio links.

Generates a polished cold email to the client (subject + body) that references those portfolios.

Outputs the email for sending (or piping to your CRM/ATS).
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

🏃 How It Works (Step-by-Step)

Load & Scrape

WebBaseLoader(url).load() → returns the full job posting text.

LLM Extraction (Groq)

PromptTemplate → ask for JSON with role_type, description, skills_required.

ChatGroq(..., model_name="llama3-70b-8192") → invoke() → parse with JsonOutputParser.

Portfolio Store (ChromaDB)

Read sample_portfolio_links.csv.

Add rows into Chroma with documents=[Techstack], metadatas={"links": Links}.

Skill Matching

Simple heuristic: intersect skills_required with portfolio Techstack text.

(Optional) Swap for semantic search / embeddings later.

Email Generation (Groq)

Feed job JSON + bullet list of matched links into a structured email prompt.

Output: client-ready email (subject + body).

<img width="2652" height="1426" alt="image" src="https://github.com/user-attachments/assets/86e87dae-b64d-4420-bf55-fd2b6725c57a" />

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Quick Start:

pip install langchain-groq langchain-core langchain-community chromadb pandas
export GROQ_API_KEY="your_key_here"
python main.py

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
🔮 Extensibility Ideas

RAG on resumes: embed full resumes and query via semantic search.

ATS/CRM hooks: auto-push generated emails and candidate matches.

Scoring: implement a weighted skill-score + recency + seniority signals.

Guardrails: add JSON schema validation + retry prompts.

