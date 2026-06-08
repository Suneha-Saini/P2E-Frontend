# Local AI Bank Statement Converter - Next.js Frontend

This is the standalone frontend web dashboard for the **Local AI Bank Statement & Document to Excel Converter**. Built with Next.js, React, Tailwind CSS, and AG Grid, it provides an interactive UI to upload bank statement documents, inspect parsing pipelines, audit transactions, edit records, and compile styled Excel workbooks.

## Features

- **Document Hub Interface**: Drag-and-drop file uploader supporting PDFs and images (PNG, JPG, TIFF, etc.).
- **Live Pipeline Tracker**: Visual step-by-step progress tracking for local OCR execution and LLM extraction.
- **AG Grid Spreadsheet Editor**: High-performance editable data grid to inspect, add, update, or delete transaction records before compiler export.
- **Audit and Verification Panel**: Built-in verification engine that audits ledger running balances row-by-row and flags offset rows or balance breaks.
- **AI Settings Configuration**: Control panel to manage API endpoints, model selections, and keys for Ollama, LM Studio, Groq, OpenAI, Claude, Gemini, etc.
- **Tauri Integration Ready**: Fully prepared structure for packaging into a standalone local desktop `.exe` app.

---

## Directory Structure

```text
frontend/
├── src/
│   ├── app/
│   │   ├── favicon.ico
│   │   ├── globals.css      # Core Tailwind CSS tokens & global designs
│   │   ├── layout.tsx       # Root layout structure
│   │   └── page.tsx         # Main interactive dashboard component (54KB)
├── public/                  # Static assets
├── eslint.config.mjs        # ESLint rule configuration
├── next.config.ts           # Next.js configuration
├── package.json             # App dependencies & run scripts
├── tsconfig.json            # TypeScript configuration
└── Dockerfile               # Production multi-stage Docker builder
```

---

## Getting Started

### Prerequisites
- **Node.js** (v18.x or v20.x recommended)
- **npm** (comes with Node.js) or **yarn** / **pnpm** / **bun**

### 1. Install Dependencies
Navigate to the `frontend/` directory in your terminal and install packages:
```bash
npm install
```

### 2. Connect to the FastAPI Backend
By default, the client is programmed to dynamically resolve the API base URL. It checks the hostname of your web browser (`window.location.hostname`) and targets port `8000` on the same host (e.g. `http://127.0.0.1:8000/api`).

- Ensure your backend server is active and running on `http://127.0.0.1:8000`.
- If you run the backend on a different port or host, update `DEFAULT_API_URL` on line 31 in [page.tsx](src/app/page.tsx).

### 3. Start the Development Server
```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## Production Build & Deployment

To compile the production bundle:
```bash
npm run build
npm run start
```

### Docker Deployment
```bash
# Build the production image
docker build -t pdf-excel-frontend .

# Run the container (binds to port 3000)
docker run -p 3000:3000 pdf-excel-frontend
```

---

## Desktop Client Packaging (via Tauri)

To wrap this web dashboard and the Python backend into a single self-contained desktop application:

1. Initialize Tauri in the `frontend/` folder:
   ```bash
   npm run tauri init
   ```
2. Build your FastAPI python backend into a single-directory sidecar using PyInstaller (see backend README instructions).
3. Place the sidecar inside the newly generated `src-tauri/bin/` folder.
4. Run the compilation build tool:
   ```bash
   npm run tauri build
   ```
   *This compiles a standalone desktop installer (`.msi` or `.exe`) inside `src-tauri/target/release/`.*
