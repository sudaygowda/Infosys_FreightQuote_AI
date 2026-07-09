# Infosys Technical Internship Portal - Milestone 1

A security-hardened, production-ready user authentication gateway built with **Streamlit** and Python. This application serves as the verification environment framework for the internal **Infosys Technical Internship Training Program (Milestone 1 Objectives)**.

The interface utilizes a polished **Minimalist Tech Blue-Steel Theme** featuring an asynchronous layout split-panel card framework, responsive form elements, and full sidebar-free navigation flows to deliver a premium user experience.

---

## 📍 Milestone 1 Core Objectives

The architecture fulfills the following core task requirements under the technical training roadmap:
* **Task 1: Monolithic Multi-Page Layout Architecture** – Implemented complete single-file state-driven routing spanning Login, Signup, Forgot Password, and an interactive Admin/User Dashboard workspace.
* **Task 2: Cryptographic Password Passkey Hashing** – Integrated industry-standard safety hashing checks utilizing `bcrypt` dynamic password blocks salting engine algorithms to completely protect user passkeys.
* **Task 3: Persistent Audit Logs & Directory Storage** – Created a localized `SQLite` relational database engine managing state tracking variables, rate-limiting variables check streams, and an immutable real-time security logging trail system.
* **Task 4: Asynchronous Transactional Email OTP Pipelines** – Configured automated generation protocols dispatching 6-digit verification codes rendered inside custom, anti-spam responsive HTML transactional templates.

---

## 🛡️ Security Engineering Features

### 1. Form Validation Matrix
* **Mandatory Constraints Check:** Enforces strict non-empty parameters verification checks across all entry input cards.
* **System Email Regular Expressions:** Screens inputs using automated regex filtering checking syntax structure shapes systematically (`r"^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$"`).
* **Password Complexity Policies:** Enforces strict security bounds filters requiring a minimum of 8 characters containing upper/lowercase streams, digits, and special characters.

### 2. Standalone Account Recovery Methods Channels
* **Cryptographic Question Pathway:** Fallback validation framework matching localized cryptographically encrypted response hash streams.
* **Temporal OTP Key Pipeline:** Relays verification codes through automated email infrastructure containing an integrated 5-minute lifecycle expiration window.

### 3. Role-Based Identity Access Control (RBAC)
* **Admin Console Security Gate:** Configured an entirely separate, hardcoded administrator clearance panel (`admin` / `Admin@1234`) providing access to:
    * Active User Registry Directory Tables (excluding sensitive password block columns).
    * System Audit Logging reconciliation tables detailing connection events (`AUTH_SUCCESS`, `REGISTRATION`, etc.).
* **User Workspaces:** Protected environment nodes secured via state tracking JSON Web Tokens (`JWT`) signed under private HMAC SHA-256 algorithm strings.

---

## 🚀 Technical Architecture Stack

* **Frontend User Interface:** Streamlit (Python-driven Reactive Web Framework)
* **Styling Engine:** Custom Responsive HTML/CSS Injection Layouts
* **Security Session Tracking:** PyJWT (JSON Web Tokens)
* **Cryptographic Engine:** Bcrypt (SHA-256 Salting Passwords Core)
* **Database Infrastructure:** SQLite3 (Local Persistent Relational Storage)
* **Tunneling System:** Pyngrok (Secure External WAN Edge Tunnel Gateway)

---

## ⚙️ Installation & Deployment Configuration

### 1. Prerequisites (Google Colab Environment)
Ensure you configure your **Colab Secrets** tab (left sidebar keys icon) with the following environment variable keys to grant the workspace connectivity parameters securely:
* `JWT_SECRET`: Any random alphanumeric string signature key.
* `NGROK_AUTHTOKEN`: Your static infrastructure auth key from dashboard.ngrok.com.
* `EMAIL_ADDRESS`: The registered Google account email address serving as dispatch relay.
* `EMAIL_PASSWORD`: A 16-character Gmail **App Password** created via your Google Account Security dashboard.

### 2. Execution Run Command
Simply load the cell inside your Jupyter Notebook workspace and trigger execution:
```bash
# Run the deployment cells pipeline to compile app.py and spin up the tunneling gateway
