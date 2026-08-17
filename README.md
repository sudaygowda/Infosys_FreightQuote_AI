# Agentic AI for Maritime Freight Pricing and Route Optimization

> **Codename:** FreightQuote AI\
> **Infosys Springboard Internship --- Batch 1**\
> **Tagline:** An agentic decision-support copilot for an ocean-freight
> brokerage --- grounded routing, pricing, weather, and compliance
> answers.

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-App-red)](https://streamlit.io/)
[![SQLite](https://img.shields.io/badge/Database-SQLite-003B57)](https://www.sqlite.org/)

## 1. Program & Team Context

**Program:** Infosys Springboard Internship --- Batch 1

The supplied Milestone 4 notebook does not contain the mentor's
name/designation or the final team-member GitHub handles. Those details
are intentionally left as placeholders rather than being invented.

  Name                      Role / What They Built   GitHub handle
  ------------------------- ------------------------ ----------------
  **\[Add team member\]**   \[Add contribution\]     \[Add handle\]
  **\[Add team member\]**   \[Add contribution\]     \[Add handle\]
  **\[Add team member\]**   \[Add contribution\]     \[Add handle\]

**Mentor:** \[Add mentor full name and designation\]

## 2. Project Overview

### Problem Statement

Ocean-freight brokers and logistics operations teams need to make
decisions across routing, freight pricing, carrier selection, weather
risk, customs compliance, shipping documents, multilingual SOPs, and
internal knowledge. These decisions are normally spread across multiple
data sources and tools, making it harder to get one consistent
operational answer.

### Solution

FreightQuote AI is a Streamlit-based multi-agent maritime freight
intelligence portal. It combines SQLite operational data, specialised
analytical agents, route and pricing logic, retrieval tools, document
processing, translation, and an LLM-based copilot. The application is
designed to return grounded answers from retrieved operational facts
rather than relying only on free-form generation. It also provides
role-aware navigation, authentication, OTP password recovery,
administrative monitoring, and a PDF/RAG workbench.

### Key Differentiators

-   **Grounded generation:** answers are built from retrieved
    database/knowledge-base facts.
-   **Transparent ML benchmarking:** multiple models are compared inside
    the agent interfaces.
-   **RBAC:** the available application modules depend on the signed-in
    role.
-   **Fail-soft LLM path:** the Qwen 3B model can fall back to the 1.5B
    model when required.
-   **Operational simulators:** pricing, routing, carrier, weather,
    customs, margin and incident scenarios expose adjustable parameters.
-   **Document intelligence:** OCR-style document extraction,
    translation and PDF vector retrieval are included.

## 3. Architecture

![Architecture](docs/architecture-diagram.png)

### 3.1 Data Layer

The application initializes and seeds a local SQLite database containing
operational entities such as ports, shipments, carriers, routes, freight
quotes, customs/tariff records, weather risks and alerts.

### 3.2 Reasoning Tools Layer

Specialised modules provide routing, pricing, carrier intelligence,
weather risk, margin optimisation, customs analysis, document
processing, translation and PDF/RAG capabilities.

### 3.3 Orchestration Layer

`intent_router.py` classifies user questions into operational intents
and runs grounded queries. The project also contains Haversine-based
route calculations and freight-quote calculation logic.

### 3.4 Generation Layer

`llm_engine.py` connects the application to the Qwen model service. The
notebook's model server targets `Qwen/Qwen2.5-3B-Instruct`, with a
smaller `Qwen/Qwen2.5-1.5B-Instruct` fallback when the 3B model cannot
fit.

## 4. The 9 Specialised Agents

> The project also contains supporting modules such as Notifications,
> Digital Twin, Knowledge Graph, Data Feed Center and Anomaly Scanner.
> The nine agents below follow the README instruction document's
> specialised-agent structure.

  -----------------------------------------------------------------------------------------------
  Agent            Business function Data / knowledge    Best model / engine     Main outputs
                                     source                                      
  ---------------- ----------------- ------------------- ----------------------- ----------------
  **1. Global      Port congestion,  `ports`             Random Forest Regressor Port analytics,
  Ocean Port &     dwell and                             --- R² 0.96, RMSE 0.4   route map, delay
  Route            route/fuel                            days                    prediction,
  Intelligence**   analysis                                                      fuel/speed
                                                                                 simulation

  **2. Dynamic     Ocean spot-rate   `freight_quotes`    Random Forest Pricing   Rate analytics,
  Freight Pricing  and quote                             Regressor --- R² 0.97,  quote
  & Rate           calculation                           RMSE \$65               calculator,
  Calculator**                                                                   margin/rate
                                                                                 analysis

  **3. Carrier     Carrier           `carriers`          Random Forest Ranker    Reliability
  Performance &    reliability, cost                     --- Accuracy 0.96, F1   ranking, carrier
  Safety Audit**   and capacity                          0.95                    analytics,
                                                                                 capacity/SLA
                                                                                 simulation

  **4. Global      Port weather and  `weather_risks` +   Random Forest           Weather-risk
  Weather Risk &   storm-risk        Open-Meteo          Classifier --- Accuracy map, risk
  Harbor Safety    monitoring                            0.95, F1 0.94           ledger,
  Intelligence**                                                                 rerouting
                                                                                 simulation

  **5. Freight     Margin, revenue   `freight_quotes`    Random Forest Regressor Margin
  Margin Optimizer and carrier yield                     --- R² 0.96, RMSE \$85  analytics, yield
  & Profitability  analysis                                                      matrix,
  Intelligence**                                                                 profit/rate
                                                                                 simulation

  **6. Customs     Duty, clearance   `customs_tariffs`   Random Forest Risk      Duty analytics,
  Intelligence &   and                                   Classifier --- Accuracy hold-risk
  HS Code          regulatory-risk                       0.96, F1 0.95           prediction, duty
  Compliance**     analysis                                                      simulator

  **7. Quote       Shipping document `shipments` +       Random Forest           OCR payload,
  Document & Bill  extraction and    uploaded documents  Classifier --- Accuracy extracted
  of Lading        validation                            0.97, F1 0.96           fields,
  Generator /                                                                    document-fraud
  OCR**                                                                          analysis, BoL
                                                                                 builder

  **8. Freight     Maritime SOP and  Built-in            NLLB-200 distilled 600M Multilingual
  Document &       document          SOP/glossary                                translation, SOP
  Policy           translation       content + NLLB                              translation,
  Translation                                                                    glossary
  Engine**                                                                       

  **9. Custom PDF  Grounded Q&A over Uploaded            FAISS +                 Retrieved chunks
  Knowledge Base & uploaded freight  PDFs/TXT/MD +       sentence-transformers   and grounded
  Vector RAG       documents         vector/BM25 indexes retrieval               document answers
  Engine**                                                                       
  -----------------------------------------------------------------------------------------------

### Supporting operational modules

The notebook also implements:

-   **Real-Time Freight Incident & Alert Manager**
-   **Maritime Telemetry Anomaly & Risk Scanner**
-   **Global Ocean Freight Logistics Digital Twin**
-   **Freight Knowledge Graph**
-   **Data Feed Center**
-   **User Profile**
-   **Admin Dashboard**
-   **AI Copilot**

## 5. Agent Details

### Agent 1 --- Global Ocean Port & Route Intelligence

The Route AI module reads port data and presents congestion/dwell
analytics, route intelligence and a vessel speed/fuel-efficiency
simulator. Its benchmark table identifies the Random Forest Regressor as
the optimal model with an R² score of 0.96 and RMSE of 0.4 days.

### Agent 2 --- Dynamic Freight Pricing Engine

The pricing module reads `freight_quotes` and combines rate analytics
with an interactive spot-quote calculator. The notebook's benchmark
identifies the Random Forest Pricing Regressor as the optimal model with
R² 0.97 and RMSE of \$65.

### Agent 3 --- Carrier Performance & Capacity Intelligence

The carrier module reads carrier records and compares reliability, cost
and fleet/capacity indicators. The benchmark identifies the Random
Forest Ranker as best with Accuracy 0.96 and F1 0.95.

### Agent 4 --- Weather Risk Intelligence & Storm Telemetry

The weather module reads weather-risk data and can use live Open-Meteo
weather information. It provides a risk ledger, visual radar/map
information and a typhoon/vessel rerouting simulator. Random Forest
Classifier is the best benchmarked model at Accuracy 0.95 and F1 0.94.

### Agent 5 --- Dynamic Margin Predictor & Yield Optimizer

The margin module reads freight quotes, derives net profit and compares
carrier-level margin performance. The notebook identifies Random Forest
Regressor as the best model with R² 0.96 and RMSE \$85.

### Agent 6 --- Customs, Tariff & Regulatory Compliance

The customs module reads `customs_tariffs` and analyses duty rates,
clearance risk and required documents. It includes an 8-parameter duty
simulator. Random Forest Risk Classifier is the best benchmarked model
at Accuracy 0.96 and F1 0.95.

### Agent 7 --- Digital Bill of Lading & Document OCR Studio

The document module reads shipment information and supports
PDF/image/text upload. The notebook includes a sample Bill of Lading OCR
payload with extracted metadata such as document ID, shipper, consignee,
ports, container ID, HS code, weight and declared value. The benchmark
identifies Random Forest Classifier as best at Accuracy 0.97 and F1
0.96.

### Agent 8 --- Multilingual Maritime SOP & Document Translation Studio

The translation engine uses `facebook/nllb-200-distilled-600M` and
supports 20+ configured languages in the notebook. It provides real-time
text translation, maritime SOP translation, batch translation and a
maritime trade glossary.

### Agent 9 --- PDF SOP & Freight Document RAG Studio

The RAG workbench accepts PDF/TXT/MD documents, extracts content, chunks
it and retrieves relevant information using the project's vector/BM25
retrieval stack. This agent is intended for grounded Q&A over customs
manuals, logistics SOPs, tariff rules and carrier documents.

## 6. Authentication, OTP & RBAC

### Authentication flow

**Sign up → Login → JWT session → Forgot Password → OTP → Security
Question fallback → Password reset**

The notebook contains password hashing, progressive failed-login
lockout, JWT session state, enterprise-role registration, Gmail OTP
recovery, security-question fallback, and user profile/password
management.

### RBAC roles

  -----------------------------------------------------------------------
  Role                                Typical access
  ----------------------------------- -----------------------------------
  **Admin**                           Full application, including Admin
                                      Dashboard

  **Operations Manager**              Full operational access and Data
                                      Feed Center, excluding Admin
                                      Dashboard

  **Freight Broker**                  Operational agents and AI Copilot

  **Auditor**                         Audit/analysis-oriented access
                                      defined by RBAC configuration

  **Customer**                        Customer-facing quote/operational
                                      modules
  -----------------------------------------------------------------------

The exact allowed menu is controlled by `rbac.py`.

## 7. Admin Dashboard

The Admin Dashboard provides:

-   GPU VRAM and GPU utilisation telemetry
-   Application uptime and LLM status
-   User lifecycle management
-   Role promotion/demotion
-   Account lock/unlock controls
-   User deletion controls
-   ML model performance information
-   LLM activity monitoring
-   Alert monitoring
-   Database maintenance/re-seeding controls

![Admin Dashboard](docs/screenshots/05-admin-dashboard.png)

## 8. Screenshots

The supplied package includes screenshots based on the UI, data and
labels defined in the Milestone 4 notebook.

### Login / Authentication

![Login](docs/screenshots/01-login.png)

### Main Maritime Freight Portal

![Dashboard](docs/screenshots/02-dashboard.png)

### Agent 2 --- Dynamic Freight Pricing

![Agent 2](docs/screenshots/03-agent2-spot-quotes.png)

### AI Copilot --- Grounded Question

![AI Copilot](docs/screenshots/04-ai-copilot.png)

### Admin Dashboard

![Admin](docs/screenshots/05-admin-dashboard.png)

## 9. Technology Stack

  ------------------------------------------------------------------------
  Layer                   Technology               Purpose
  ----------------------- ------------------------ -----------------------
  UI                      Streamlit                Interactive enterprise
                                                   portal

  Navigation              streamlit-option-menu    Role-aware application
                                                   navigation

  Database                SQLite                   Local operational data
                                                   store

  Data processing         Pandas, NumPy            Data preparation and
                                                   analytics

  ML                      scikit-learn             Model benchmarking and
                                                   predictive analytics

  Visualisation           Plotly, Folium,          Interactive charts and
                          streamlit-folium         maps

  LLM                     Qwen2.5-3B-Instruct /    Grounded
                          1.5B fallback            natural-language
                                                   generation

  NLP                     NLLB-200                 Offline multilingual
                                                   translation

  RAG                     FAISS,                   Document retrieval
                          sentence-transformers,   
                          BM25                     

  Documents               pdfplumber, ReportLab,   PDF extraction and
                          FPDF                     document generation

  Authentication          bcrypt, PyJWT            Password hashing and
                                                   JWT sessions

  Weather                 Open-Meteo               Port weather context

  API service             FastAPI                  Qwen/NLLB model service

  Optional ingestion      Kaggle API               Data Feed Center
                                                   pipeline
  ------------------------------------------------------------------------

## 10. Installation & Run

The supplied notebook is designed around a Google Colab/Linux-style
environment.

### 1. Clone the repository

``` bash
git clone https://github.com/<org-or-user>/freightquote-ai.git
cd freightquote-ai
```

### 2. Create a virtual environment

``` bash
python -m venv venv
```

Windows:

``` bash
venv\Scripts\activate
```

Linux/macOS:

``` bash
source venv/bin/activate
```

### 3. Install dependencies

``` bash
pip install -r freight_app/requirements.txt
```

### 4. Configure environment variables

Create a `.env` locally and never commit it.

  Variable                           Purpose
  ---------------------------------- ---------------------------------------
  `HF_TOKEN` / `HUGGINGFACE_TOKEN`   Hugging Face model access
  `JWT_SECRET_KEY`                   JWT signing secret
  `EMAIL_ID`                         Project OTP mailbox
  `EMAIL_PASSWORD`                   Gmail App Password for OTP SMTP
  `FREIGHTQUOTE_DATA_DIR`            Optional local runtime-data directory

### 5. Seed the database

``` bash
cd freight_app
python -c "from db import init_db; from seed_data import seed_all; init_db(); seed_all()"
```

### 6. Run the Streamlit application

``` bash
streamlit run app.py
```

### Google Colab workflow

The supplied notebook creates the `freight_app/` project files, installs
dependencies, initializes/seeds SQLite, starts the AI model service and
launches Streamlit. The original notebook also contains a Cloudflare
Tunnel step for exposing the Streamlit application.

## 11. Hardware & Runtime Notes

The original notebook diagnostics recorded a successful run with:

-   PyTorch 2.11.0 + CUDA 12.8
-   CUDA available
-   NVIDIA Tesla T4
-   1 GPU device
-   4-bit/FP16-oriented model serving
-   Qwen 3B model with 1.5B fallback
-   NLLB-200 translation service

The 3B LLM and NLLB model are the heavy components. Expect several GB of
disk usage and longer first-run setup because model weights must be
downloaded.

## 12. requirements.txt Note

The supplied notebook currently writes `requirements.txt` using version
ranges such as `streamlit>=...` rather than the exact `==` pins required
by the repository instructions.

Before final submission, regenerate requirements from a clean
environment:

``` bash
pip freeze > requirements.txt
```

Then remove unused packages and test the resulting file inside a
brand-new virtual environment.

## 13. Secrets & Security

**Never commit:**

-   Hugging Face tokens
-   Kaggle credentials
-   Gmail passwords or App Passwords
-   JWT signing secrets
-   `.env` files containing real values
-   Database files containing real personal data

Use `.env.example` with variable names/placeholders only, and add
`.env`, `*.db`, `__pycache__/` and `*.ipynb_checkpoints` to
`.gitignore`.

For OTP email, use a dedicated project mailbox with Google 2-Step
Verification and an App Password. Do not use a personal mailbox for the
demo.

## 14. Maritime Glossary

  -----------------------------------------------------------------------
  Term                                Meaning
  ----------------------------------- -----------------------------------
  **BAF**                             Bunker Adjustment Factor ---
                                      fuel-related surcharge applied to
                                      freight

  **TEU**                             Twenty-foot Equivalent Unit ---
                                      standard container capacity measure

  **HS Code**                         Harmonized System code used to
                                      classify traded goods

  **Dwell Time**                      Time cargo/container remains at a
                                      port or terminal

  **Bill of Lading (BoL)**            Shipping document describing the
                                      cargo, shipment and transport terms
  -----------------------------------------------------------------------

## 15. Known Limitations

-   The notebook is primarily structured for a Colab/GPU execution
    environment.
-   Some data is seeded/synthetic rather than production maritime
    telemetry.
-   SQLite is suitable for the prototype but is not a production-scale
    multi-user database.
-   External services such as Open-Meteo, Hugging Face and Gmail require
    appropriate connectivity/credentials.
-   The Streamlit application and model service require considerably
    more resources than a simple CPU-only Python script.

## 16. Future Scope

-   Move from SQLite to PostgreSQL for production multi-user
    deployments.
-   Add real carrier/port data feeds and streaming telemetry.
-   Add production-grade model monitoring and automated retraining.
-   Deploy the model service and Streamlit frontend as scalable cloud
    services.
-   Expand document RAG with stronger access controls, citation tracing
    and enterprise document permissions.

## 17. Acknowledgements

This project was developed as part of the **Infosys Springboard
Internship --- Batch 1**.

**Mentor:** \[Add mentor name and designation\]

## 18. Repository Structure

``` text
freightquote-ai/
├── README.md
├── requirements.txt
├── .env.example
├── .gitignore
├── docs/
│   ├── architecture-diagram.png
│   ├── screenshots/
│   │   ├── 01-login.png
│   │   ├── 02-dashboard.png
│   │   ├── 03-agent2-spot-quotes.png
│   │   ├── 04-ai-copilot.png
│   │   └── 05-admin-dashboard.png
│   └── demo/
│       └── demo.mp4
├── freight_app/
│   ├── app.py
│   ├── auth.py
│   ├── rbac.py
│   ├── db.py
│   ├── seed_data.py
│   ├── intent_router.py
│   ├── llm_engine.py
│   ├── rag_engine.py
│   ├── translation_engine.py
│   └── ...
└── milestone notebooks/
```

## 19. Final Pre-Submission Checklist

-   [ ] Root-level `README.md` is present.
-   [ ] Mentor name/designation added.
-   [ ] Final team-member names, contributions and GitHub handles added.
-   [x] Architecture diagram embedded.
-   [x] All nine specialised agents documented.
-   [ ] Exact pinned `requirements.txt` generated and tested in a clean
    environment.
-   [ ] `.env.example` contains names/placeholders only.
-   [ ] `.env` is ignored by Git.
-   [ ] No tokens/passwords are present in the repository or notebook
    history.
-   [ ] Silent 2--5 minute demo video added or linked.
-   [x] Login, dashboard, core agent, grounded copilot and admin
    screenshots included.
-   [ ] OTP forgot-password flow captured in the final demo/screenshots.
