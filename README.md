# Brew & Bean Core Triage Workflow (n8n)

Automated customer message triage workflow built in **n8n** using **Groq** (`llama-3.3-70b-versatile`) and a **Structured Output Parser** with an explicit JSON Schema. Incoming POST requests are classified into `Order`, `Complaint`, or `Info` with guaranteed top-level output fields (`category` and `items`), while any AI/credential errors are caught and logged to `error_logs.json`.

## Files
- `brew-bean-core-triage.json`: Exported n8n workflow with Webhook, AI Agent, Structured Output Parser (explicit JSON Schema), Guarantee Output Fields node, and Error-handling file logger.
- `error_logs.json`: Sample JSON error log generated when the AI node fails.
- `README.md`: Setup, credential, webhook host configuration, and testing instructions.

## Setup Instructions

### 1. Obtain a Groq API Key
1. Sign up or log in at the **GroqCloud Console**: https://console.groq.com/keys
2. Navigate to **API Keys -> Create API Key**, name your key (e.g., `n8n-brew-bean`), and copy the generated key (`gsk_...`).
3. In n8n, go to **Credentials -> Add Credential -> Groq API**, paste your `gsk_...` API key, and click **Save**.

### 2. Import & Activate the Workflow
1. Run n8n locally using `npx n8n` (or Docker) and open `http://localhost:5678`.
2. Import `brew-bean-core-triage.json` via the top-right menu (**`...` -> Import from File**).
3. Open the **Groq Chat Model** sub-node connected to **AI Agent** and select your saved Groq API credential.
4. Confirm **Structured Output Parser** is set to **Define using JSON Schema** (requiring `category` and `items`) and that the **Guarantee Output Fields** (`Edit Fields`) node sits between **AI Agent** and **Respond Success**.
5. Click **Publish** so `POST http://localhost:5678/webhook/brew-bean-triage` is active.

### 3. Webhook URL & Host Configuration (Local vs. Production)
- **Local Development (Default):**
  By default, n8n serves the webhook on localhost at:
  `http://localhost:5678/webhook/brew-bean-triage`
- **Deploying Beyond `localhost` (Reverse Proxy, Docker, Cloud VM, or Tunnel):**
  When hosting n8n behind a custom domain, reverse proxy (Nginx/Caddy), cloud VM, or tunnel (such as **ngrok** or **Cloudflare Tunnel**), set the `WEBHOOK_URL` environment variable before starting n8n so n8n registers and listens on your public host instead of `localhost`:
  ```bash
  export WEBHOOK_URL="[https://your-public-domain-or-tunnel.app/](https://your-public-domain-or-tunnel.app/)"
  npx n8n
