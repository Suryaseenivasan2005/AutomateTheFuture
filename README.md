# 🟢 WhatsApp AI Chatbot (Modernized v2 - Stateless)

This directory contains a premium, highly optimized **n8n AI Chatbot workflow** that integrates with **Twilio WhatsApp Sandbox** and the **Groq API (Llama 3.3 70B)**. It is built as a highly reliable, stateless chatbot flow that uses a direct **Webhook node** (POST `/whatsapp`) for maximum performance, minimal overhead, and compatibility with local ngrok development.

---

## 🌟 Key Features & Modernizations
* **Stateless & Robust Architecture:** Removed all complex conversational memory nodes (Redis, static data buffers) to create a lean, super-stable, zero-latency response loop.
* **Native Webhook Integration:** Replaced the legacy Twilio Trigger with a high-performance **Webhook node (POST)** to allow instant `200 OK` acknowledgment, eliminating Twilio webhook timeout warnings.
* **Advanced Error Handling:** Incorporates a robust **Extract AI Reply** code block that automatically handles downstream API timeouts or failures by serving a polite fallback response.
* **Realistic Typing Latency:** Includes a 2-second typing delay node to simulate natural human typing patterns before sending the final message.
* **Perfect Alignment & Organization:** Fully organized nodes aligned on a single horizontal grid axis with beautifully color-coded Sticky Notes.

---

## 🛠️ Step-by-Step Setup Guide

### 1. Host Environment Variables
Ensure the following environment variable is loaded in your n8n server/instance:
```bash
GROQ_API_KEY=gsk_... # Your Groq API Key
```

---

### 2. Configure Twilio WhatsApp Sandbox
1. Go to the [Twilio Console](https://console.twilio.com/).
2. Under **Messaging** ➔ **Try it out** ➔ **Send a WhatsApp Message**.
3. Activate the WhatsApp Sandbox by sending the join code from your phone to the Sandbox number (e.g., `+1 415 523 8886`).
4. Copy your **Account SID** and **Auth Token** to use in n8n.

---

### 3. Import the Workflow to n8n
1. Open your n8n workspace.
2. Click on the three dots at the top right of the canvas and select **Import from File**.
3. Select `whatsapp-ai-chatbot-groq.json`.
4. The workflow will render beautifully with color-coded **Sticky Notes** guiding you through each section.

---

### 4. Setup Twilio Credentials in n8n
1. Double-click the **Twilio Send Message** node.
2. Under **Credential to connect with**, click **Create New Credential**.
3. Paste your Twilio **Account SID** and **Auth Token**.
4. Save the credential.

---

### 5. Link the Twilio Sandbox to your n8n Webhook (via ngrok)
1. Double-click the **Webhook** node in n8n.
2. If running locally, start ngrok to expose your local n8n instance:
   ```bash
   ngrok http 5678
   ```
3. Copy the **Production URL** (or **Test URL** for manual testing) from the n8n Webhook node.
4. Replace `localhost:5678` with your ngrok forwarding domain (e.g., `https://xxxx.ngrok-free.app/webhook/whatsapp`).
5. Go to the Twilio Console Sandbox settings.
6. Under **Sandbox Configuration**, paste your n8n Webhook URL into the **"When a message comes in"** input field (ensure the method is POST).
7. Save Sandbox settings in Twilio.

---

## 🚀 Execution & Testing
1. Click **Listen for test event** on the **Webhook** node.
2. Send a WhatsApp message (e.g., `"Hello Llama!"`) to your Twilio Sandbox number.
3. The Webhook immediately fires `200 OK` back to Twilio.
4. The workflow parses the payload, sends it to Groq, extracts the answer, delays for 2 seconds, and delivers the message back to your phone via Twilio!
5. Activate the workflow to run it 24/7.
