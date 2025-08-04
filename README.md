# 📱 WhatsApp-Driven Google Drive Assistant (n8n Workflow)

This project lets you control your Google Drive via WhatsApp messages using **n8n**, **Twilio**, **Google OAuth2**, and **ngrok**. Send simple text commands like:
COMMANDS
- `LIST /FolderName` → List files  
- `DELETE /Folder/File.ext` → Delete file  
- `MOVE /From/File.ext to /destination` → Move file  
- `SUMMARY /FolderName` → AI-generated bullet digest using OpenAI/GPT

## Eroor Handling 
It handles mostly probable error like 
-if you enter wrong folder or file name then twilio will send you 
the message that wrong folder/file name and donot execute the action


All workflows are built using n8n and persist across Docker restarts.

---

## 🧰 Tech Stack

- **n8n**: Workflow automation
- **Twilio WhatsApp Sandbox**: Receive/send WhatsApp messages
- **Google Drive API**: Manage Drive files
- **OpenAI / GPT-4o**: File summarization
- **ngrok**: Public URL for webhook
- **Docker**: Runtime with volume for persistence

---

## 🔧 Setup Instructions

### ✅ 1. Clone the Repo


git clone https://github.com/your-username/whatsapp-gdrive-assistant.git
cd whatsapp-gdrive-assistant
🧪 2. Twilio Sandbox for WhatsApp
Go to Twilio Console

Activate Sandbox for WhatsApp

Note down:

Account SID

Auth Token

Sandbox Number (e.g., +14155238886)

Join Code (e.g., join green-cow)

On your phone, send the join code to the sandbox number to enable messaging.

🔐 3. Google OAuth2.0 Credentials
Go to Google Cloud Console

Create a new project and enable Google Drive API

Go to APIs & Services > Credentials

Create OAuth 2.0 Client ID

App type: Web

Authorized redirect URI:


Copy code
http://localhost:5678/rest/oauth2-credential/callback
Note:

Client ID

Client Secret

You'll use these either in the workflow or as environment variables in Docker.

🌐 4. ngrok Tunnel for Webhooks
Download & install ngrok

Authenticate:

ngrok config add-authtoken YOUR_AUTH_TOKEN
Start tunnel:
ngrok http 5678
Copy the https://xxxx.ngrok.io URL

Paste that URL in Twilio Sandbox as the Webhook URL for incoming messages (add /webhook/whatsapp-in if needed).

🐳 5. Docker Setup with Persistent Volume
Create a file docker-compose.yaml with this content:

yaml

version: "3.1"

services:
  n8n:
    image: n8nio/n8n
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=admin123
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - WEBHOOK_TUNNEL_URL=https://your-ngrok-url.ngrok.io
      - GENERIC_TIMEZONE=Asia/Kolkata
      - GOOGLE_CLIENT_ID=your-client-id
      - GOOGLE_CLIENT_SECRET=your-client-secret
    volumes:
      - n8n_data:/home/node/.n8n

volumes:
  n8n_data:

  
Then start n8n with:
docker-compose up -d
Your n8n workflows and credentials are now safe across restarts in the n8n_data volume.


















