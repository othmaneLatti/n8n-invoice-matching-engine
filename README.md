# AI Document Reconciliation & Invoice Matching (n8n)

An enterprise-grade ETL pipeline built in n8n that automates the ingestion, parsing, and strict mathematical reconciliation of purchase invoices against physical delivery notes. 

<img width="1536" height="1024" alt="ChatGPT Image Aug 18, 2026, 07_20_16 PM" src="https://github.com/user-attachments/assets/bc1fe405-27ea-4462-8e71-91aabd005753" />


## 📌 Project Overview
Matching messy PDF delivery notes to XML invoices is a manual nightmare, especially with OCR typos and partial deliveries. I built a resilient n8n automation to solve this. It uses Levenshtein fuzzy logic to pre-filter candidates, followed by AI semantic matching to link differently named items. To prevent AI math hallucinations, a custom JavaScript engine strictly handles all quantity arithmetic. Approved documents are auto-archived in structured Drive folders, while mismatches trigger dynamic HTML email alerts. The system features full database logging and global error monitoring.

---

## 🛑 The Problem
In the HVAC and supply chain industry, document reconciliation is a chaotic, manual nightmare. 

Purchase invoices arrive as structured XML files in an email inbox, while the corresponding delivery notes are often crumpled, coffee-stained physical papers scanned into a Drive folder. Matching them manually takes hours. Worse, suppliers frequently deliver partial orders (e.g., billing for 10 copper pipes but delivering 8). 

Companies are forced to choose between wasting expensive administrative hours matching line items by hand, or blindly paying invoices without verifying if the materials actually arrived on site.

<img width="1365" height="767" alt="ETL pipeline" src="https://github.com/user-attachments/assets/9821323c-d91a-4997-af8e-2fe009502c04" />

*Ingestion Engine: Automates email/Drive monitoring, extracting data from messy PDFs via OCR and parsing XML invoices seamlessly.*

<img width="1365" height="767" alt="matching pipeline" src="https://github.com/user-attachments/assets/b0c52dda-3bfc-4290-b001-f959416f79ef" />

*Match Engine: Uses custom JS to pre-filter documents, then applies strict AI semantic matching to link line items accurately.*

---

## 💡 The Solution
I engineered a fully autonomous n8n processing engine that acts as a digital accounting assistant. 

It automatically ingests documents from multiple sources, performs OCR, and cross-references them using a hybrid approach of traditional logic and AI. Instead of just guessing, it mathematically proves whether a delivery note matches an invoice, updating databases and organizing files in the background without a single human click.

<img width="1365" height="767" alt="invoice metadata index" src="https://github.com/user-attachments/assets/a4b10648-5d40-4197-882c-3e17fe4a68c2" />

*The Ledger: A lightweight, easily searchable Google Sheets index tracking high-level document metadata and real-time matching status.*

<img width="1365" height="767" alt="FULL inv json" src="https://github.com/user-attachments/assets/f1d222d9-4a53-46b3-bb61-844c0acd66ca" />

*Deep Storage: Safely logs heavy, raw JSON line-item payloads in a separate database, keeping the system fast while preserving data.*

---

## ⚙️ How It Works
To make this system enterprise-ready and cost-effective, I built it with strict guardrails:

*   **Deterministic Pre-Filtering:** Before using expensive AI tokens, a custom JavaScript Levenshtein-distance algorithm filters hundreds of documents into a narrow candidate pool based on supplier names and dates.
*   **Semantic AI Matching:** A strictly constrained LLM reads the messy OCR text and links it to the structured XML. It understands that a delivered "Compressor 3kW ART-1007" is the exact same product as a billed "3kW Compressor Unit."
*   **Strict Mathematical Routing:** AI is terrible at math, so it isn't allowed to do any. A custom Node.js script takes the AI's semantic matches and performs the actual arithmetic, calculating exactly how many items were billed versus delivered. 
*   **Automated File Management:** Matched documents are renamed and archived into clean cold-storage folders automatically.

<img width="1365" height="767" alt="structured invoice folder" src="https://github.com/user-attachments/assets/64c893d9-90a3-40e3-a14a-e2be892ac3a6" />

*Standardized Routing: Automatically renames processed documents with a strict convention and archives them into cold-storage folders.*

<img width="1365" height="767" alt="mismatched" src="https://github.com/user-attachments/assets/6b4da869-42a2-42e4-9898-1ba36c8226d7" />

*Exception Isolation: Flags partial deliveries or incorrect items and routes the physical files into dedicated folders for manual review.*

---

## ⚠️ Human-in-the-Loop Exception Handling
If the math doesn't add up, the system doesn't just crash or blindly approve the invoice. It isolates the mismatched files and immediately dispatches a dynamic HTML alert to the administration team. The email translates the raw JSON data into a plain-English explanation (e.g., "Billed 10, Delivered 8") and provides direct links to the files so a human can resolve the discrepancy in seconds.

<img width="1365" height="767" alt="email mismatch" src="https://github.com/user-attachments/assets/ae5513ad-3537-453f-ba30-3a7a636b47d3" />

*Dynamic Alerts: Generates responsive HTML emails for mismatched items, showing exact mathematical discrepancies with links to files.*

<img width="1365" height="767" alt="uncertain email" src="https://github.com/user-attachments/assets/6b75233b-0851-4d12-9ec3-75145e39370e" />

*Manual Review Prompts: Flags ambiguous AI extractions and instantly emails the team an HTML alert to verify physical document quantities.*

---

## 🛡️ Global Error Monitoring
A true production pipeline must monitor itself. I integrated a secondary, global error-handling workflow. If an external API times out or a credential expires, this safety net catches the exact point of failure. It uses AI to translate the technical stack trace into a simple summary, logs it to a database, and alerts the developer team instantly.

<img width="1365" height="767" alt="error workflow" src="https://github.com/user-attachments/assets/476c550e-3e51-4de1-9c86-1ad73061b7d7" />

*Global Error Handler: A dedicated n8n workflow that catches API timeouts or connection drops, triggering an AI diagnostic process.*

<img width="1365" height="606" alt="sheet error logs" src="https://github.com/user-attachments/assets/107db38f-a92c-41c6-9af4-b28f45d0b486" />

*Diagnostic Logs: The error workflow records the exact failing node and timestamp, paired with the AI's plain-English root cause summary.*

<img width="1365" height="767" alt="error email" src="https://github.com/user-attachments/assets/37c792e5-6f9a-4acc-821f-770691165499" />

*Admin Safety Net: Instantly sends a premium HTML alert to administrators detailing the workflow failure, complete with a direct fix link.*

---

## 🐳 Self-Hosting & Deployment
This architecture is built for privacy and scale. Included in this repository is a `docker-compose.yml` file configured for deploying the n8n environment on a private VPS. Utilizing Docker and Node.js ensures the workflow engine remains entirely self-hosted, keeping sensitive financial data (invoices and supplier details) strictly within your own infrastructure.

## 🤝 Let's Connect
I built this architecture to demonstrate high-level ETL and AI integration capabilities. If you want to discuss automation strategy, have questions about the codebase, or need an architect to build a custom operational pipeline for your business, feel free to reach out.

[Invite me on Upwork](https://www.upwork.com/freelancers/~0177c0ee0c70f91e7c?mp_source=share)
