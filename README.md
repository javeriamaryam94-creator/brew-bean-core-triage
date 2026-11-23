# Brew & Bean Core Triage Workflow (n8n)

Automated customer message triage workflow built in n8n for Brew & Bean.

## Files
- brew-bean-core-triage.json: Exported n8n workflow with Webhook, AI Agent, Structured Output Parser, and Error-handling file logger.
- error_logs.json: Sample JSON error log generated when the AI node fails.
- README.md: Setup and testing instructions.

## Setup Instructions
1. Run n8n locally using npx n8n or Docker and open http://localhost:5678.
2. Import brew-bean-core-triage.json via the top-right menu (Import from File).
3. Open the Groq Chat Model (or Google Gemini Chat Model) sub-node connected to AI Agent and add your API key credential.
4. Ensure Require Specific Output Format is enabled on AI Agent with Structured Output Parser1 connected.
5. Click Publish so POST http://localhost:5678/webhook/brew-bean-triage is active.

## Testing
Send POST requests to http://localhost:5678/webhook/brew-bean-triage with JSON body {"name": "...", "message": "..."} covering Order, Complaint, and Info messages.
