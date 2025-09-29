# Summary Generator Extension

## Overview
This project is a browser extension that generates concise summaries of various content types using AI. It can summarize:

- The text content of the current active web page  
- The content of uploaded PDF files  
- The transcripts of YouTube videos  

The backend is built with Node.js and Express, leveraging Google's Gemini AI model (`gemini-2.0-flash`) for summarization. The browser extension frontend is developed using React and Vite.

## Features
- **Web Page Summarization:** Generates a summary from the textual content of the currently active browser tab.  
- **PDF Summarization:** Allows users to upload PDF files and receive a summary of their content.  
- **YouTube Video Summarization:** Fetches the transcript of a YouTube video (if available) and summarizes it.  
- **AI-Powered Summaries:** Utilizes Google's Gemini AI for intelligent and context-aware text summarization.  
- **Chunked Processing:** For long texts, the content is split into smaller chunks, summarized individually, and then combined into a final summary to handle API limits and improve the summarization of extensive documents.  
- **User-Friendly Interface:** A simple and intuitive popup interface within the browser extension.  

## Tech Stack

**Backend:**
- Node.js  
- Express.js  
- Google GenAI SDK (`@google/genai`)  
- Multer (for handling PDF uploads)  
- pdf-parse (for extracting text from PDFs)  
- Puppeteer-core (for fetching YouTube transcripts via Browserless.io)  
- dotenv (for environment variables)  

**Frontend (Browser Extension):**
- React  
- Vite  
- Tailwind CSS  
- Web Extensions API (using `chrome.tabs`, `chrome.scripting`, `chrome.runtime`)  

**Deployment:**  
Backend API can be deployed on platforms like Vercel.  

## Project Structure
summary-generator-extension/
├── index.js # Backend Express server entry point
├── package.json # Backend dependencies and scripts
├── vercel.json # Vercel deployment configuration
├── controller/
│ └── generateResponse.js # Core logic for text processing and AI summarization
└── frontend/
├── index.html # Base HTML for the extension popup
├── package.json # Frontend dependencies and scripts
├── vite.config.js # Vite configuration for frontend build
├── public/
│ ├── content.js # Content script
│ ├── index.html # HTML template for the popup
│ └── manifest.json # Chrome extension manifest
└── src/
├── App.jsx # Main React component
├── index.css # CSS (with Tailwind)
└── main.jsx # React app entry point

arduino
Copy code

## API Endpoint

**POST /text** – Accepts text, a PDF file, or a YouTube URL and returns an AI-generated summary.  

**Form Fields:**
- `text` (String, optional): Direct text to summarize  
- `file` (File, optional): PDF file  
- `url` (String, optional): YouTube video URL  

**Responses:**  

**Success (200 OK):**
```json
{
  "success": true,
  "message": "Generated summary text in point format..."
}
Error (e.g., 404, 500):

json
Copy code
{
  "success": false,
  "message": "Error message detailing the issue."
}
How it Works
Input: User chooses to summarize a webpage, upload a PDF, or summarize a YouTube video.

Text Extraction (Backend):

Web Page: document.body.innerText is extracted from the active tab.

PDF: Text extracted using pdf-parse.

YouTube URL: Puppeteer fetches the transcript (via Browserless.io).

Text Preprocessing: Cleans the extracted text.

Chunking: Text is divided into smaller chunks (default max 4000 characters).

Initial Summarization: Each chunk is sent to the Gemini AI model (gemini-2.0-flash) for summarization.

Final Summarization: Chunk summaries are combined and reprocessed to create a concise, point-wise summary.

Display: The final summary is returned and displayed in the extension popup.

Setup and Installation
Prerequisites:

Node.js and npm/yarn

Google GenAI API Key

Browserless.io API Key

Backend Setup:

bash
Copy code
git clone https://github.com/kappa-main/summary-generator-extension.git
cd summary-generator-extension
npm install
Create a .env file in the root directory:

ini
Copy code
APIKEY=YOUR_GOOGLE_GENAI_API_KEY
BROWSERLESS_API_KEY=YOUR_BROWSERLESS_IO_API_KEY
PORT=3000
Start the server:

bash
Copy code
npm start
Frontend Setup:

bash
Copy code
cd frontend
npm install
Create a .env file in frontend/:

ini
Copy code
VITE_API_URL=http://localhost:3000
Build the extension:

bash
Copy code
npm run build
Load in Chrome:

Open chrome://extensions/

Enable "Developer mode"

Click "Load unpacked"

Select frontend/dist folder

Usage
Click the Summary Generator icon in your browser toolbar.

Choose one option:

Summarize Current Webpage

Summarize PDF

Summarize YouTube Video

Wait for the summary to load.

View the generated summary in the popup.