# ScamShield 🛡️

ScamShield is a modern, AI-powered full-stack application designed to proactively identify and flag fraudulent activities, with a specialized focus on fake job listings and malicious emails.

The platform utilizes machine learning to parse text, extract information from uploaded PDFs and images via OCR, and securely scan authentic Gmail inboxes for potentially harmful phishing attempts using a unified, user-friendly interface.

## 🚀 Key Features
* **Google Inbox Scanning:** Secure Google OAuth 2.0 integration allows users to permissionlessly scan their last 10 received emails for scam/phishing indicators.
* **Smart Job Listing Analysis:** Copy-paste job descriptions into the terminal interface to cross-reference integrity signals against our SCAM/SAFE machine learning classifiers.
* **Document Upload & OCR:** Upload standard documents, screenshots (PNG, JPG), or PDFs natively. The engine uses `EasyOCR` and `pdfplumber` to extract text and analyze it in seconds.
* **Secure CORS Proxying:** End-to-end integration designed securely with Vercel API path rewrites to completely bypass complex CORS infrastructure.

## 🏗️ Architecture Stack
This repository is formatted as a monorepo consisting of three primary modules:

1. **`ScamShieldFrontend`**: A lightning-fast Single Page Application (SPA) built with React and Vite. It serves a glassmorphism "Cyber Terminal" UI.
2. **`ScamShieldBackend`**: A robust, asynchronous API driven by FastAPI and Python. It manages ML models (`scikit-learn`), Google API bindings, and computationally heavy OCR tasks.
3. **`ScamShieldExtension`**: A supplemental browser extension package designed to accompany the web infrastructure.

---

## 💻 Local Development Setup

### 1. ScamShield Backend (FastAPI)
The backend requires Python 3.12+ and uses a virtual environment.

```bash
cd ScamShieldBackend
# Create and activate a Virtual Environment
python -m venv venv
.\venv\Scripts\Activate  # On Windows

# Install the dependencies
pip install -r requirements.txt

# Start the Development Server
uvicorn app:app --reload
```
**Environment Variables (`ScamShieldBackend/.env`):**
You must configure a `.env` file with your Google Cloud Console credentials for OAuth to work locally:
```env
FRONTEND_URL=http://localhost:5173
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
SESSION_SECRET_KEY=supersecretkey_change_me
```

### 2. ScamShield Frontend (React + Vite)
The frontend requires Node.js (v18+).

```bash
cd ScamShieldFrontend

# Install NPM dependencies
npm install

# Start the Vite development server
npm run dev
```

---

## ☁️ Deployment Instructions

This application is optimized for cloud deployment using **Vercel** and **Render**.

### Backend (Render)
1. Link your GitHub repository in your Render Dashboard and create a new **Web Service**.
2. **Build Command:** `pip install -r requirements.txt`
3. **Start Command:** `uvicorn app:app --host 0.0.0.0 --port $PORT`
4. Make sure to add the exact `FRONTEND_URL` to your Render Environment Variables once you know your deployed frontend URL (e.g. `https://scam-shield-nine.vercel.app`).

### Frontend (Vercel)
1. Import the repository into your Vercel Dashboard.
2. Important: Set the **Root Directory** setting to `ScamShieldFrontend`.
3. Vercel will automatically detect Vite and configure the `npm run build` step.
4. Open the `vercel.json` file to ensure the destination rewrite points directly to your deployed Render URL (e.g. `https://scamshield-xvcs.onrender.com/:path*`).
5. Deploy.

---

**Note on Google OAuth Deployment:**
In order for users to log in on your live Vercel URL, you must add both the `FRONTEND_URL` and `FRONTEND_URL/api/auth/callback` to the *Authorized JavaScript origins* and *Authorized redirect URIs* respectively inside the **Google Cloud Console**. When transitioning from the `Testing` phase, make sure to formally Publish your OAuth App so that external users can utilize the inbox scanner.
