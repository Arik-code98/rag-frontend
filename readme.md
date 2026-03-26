# RAG Chat Application

## Overview

A simple web-based chat interface that allows users to upload a text document and ask questions about its contents using Retrieval-Augmented Generation (RAG). Users authenticate via a registration/login system, upload a `.txt` file, and then query the document through a chat interface.

---

## Features

- User registration and login with JWT-based authentication
- Text document upload
- Ask natural language questions about uploaded documents
- Minimal, dependency-free frontend (plain HTML + JavaScript)

---

## Prerequisites

- A running backend server at `http://localhost:8000` that exposes the endpoints listed below
- A modern web browser

---

## Getting Started

### 1. Start the backend server

Ensure your backend is running at `http://localhost:8000` before opening the frontend. See your backend documentation for setup instructions.

### 2. Open the frontend

Simply open `index.html` in your browser. No build step or package manager is required.

---

## Usage

### Register
Enter a username and password in the **Register** section and click **Register**. A confirmation message will appear on success.

### Login
Enter your credentials in the **Login** section and click **Login**. A JWT token will be stored in memory for authenticating subsequent requests.

### Upload a Document
Click **Choose File**, select a `.txt` file, and click **Upload**. On success, a collection ID will be displayed — this links your questions to the uploaded document.

### Ask a Question
Type a question into the chat input and click **Ask**. The answer will appear below the input field.

---

## API Endpoints

The frontend communicates with the following backend endpoints:

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| `POST` | `/register` | Register a new user | No |
| `POST` | `/login` | Log in and receive a JWT token | No |
| `POST` | `/upload` | Upload a `.txt` document | Yes |
| `POST` | `/ask` | Ask a question about a document | Yes |

### Authentication
Login returns a JWT `access_token`, which is sent as a `Bearer` token in the `Authorization` header for protected routes.

### Request & Response Examples

**POST /register**
\`\`\`json
// Request
{ "username": "alice", "password": "secret" }
\`\`\`

**POST /login**
\`\`\`
// Request (form data)
username=alice&password=secret

// Response
{ "access_token": "<token>", "token_type": "bearer" }
\`\`\`

**POST /upload**
\`\`\`
// Request (multipart form data)
document: <file>

// Response
{ "collection_id": "abc123" }
\`\`\`

**POST /ask**
\`\`\`json
// Request
{ "collection_id": "abc123", "question": "What is the document about?" }

// Response
{ "answer": "The document is about..." }
\`\`\`

---

## Limitations & Notes

- Only `.txt` files are supported for upload.
- The JWT token is stored in a JavaScript variable and will be lost on page refresh. You will need to log in again after refreshing.
- There is no persistent UI state — the collection ID is also lost on refresh.
- The app currently supports one active document per session.
- CORS must be enabled on the backend to allow requests from the browser.