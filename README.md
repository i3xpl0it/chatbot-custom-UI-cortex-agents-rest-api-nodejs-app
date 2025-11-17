To run your app every session in CMD:

Set your Node.js path (in every new CMD window):


set PATH=C:\Users\thiruvar\nodejs;%PATH%
Go to your project folder:


cd C:\Users\thiruvar\awesome-custom-cortex-agents-rest-api-react-app
Check Node.js and npm:


node -v
npm -v
You should see their version numbers.

To install and run the app:


npm install

npm run start:all


# Overview

This guide provides the instructions to build custom application for the data agents you have already built in your Snowflake account. The LLM orchestration, planning, thinking, deep reasoning, and execution of SQL queries, etc. (similar to [ai.snowflake.com](ai.snowflake.com)) is powered by [Snowflake Cortex Agents](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents) via the [REST API](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-agents-rest-api) and the interface is built with TypeScript and Material-UI.

🏁 How To Run and Customize the Snowflake Cortex Agents React App

1. Prerequisites
Node.js (v18+):
If you lack admin rights, download ZIP from nodejs.org, extract wherever you like, and set your terminal PATH:

text
set PATH=C:\Users\yourname\nodejs;%PATH%
npm (comes with Node.js)

Snowflake account and credentials (PAT/password, database, schema, role, warehouse)

2. Clone the Repository
text
git clone https://github.com/Snowflake-Labs/awesome-custom-cortex-agents-rest-api-react-app.git
cd awesome-custom-cortex-agents-rest-api-react-app
3. Install Dependencies
text
npm install
4. Configure Environment Variables
A. Backend (.env in project root)
Copy env.backend.example to .env. Fill in like:

text
SERVER_PORT=3001
NODE_ENV=development
ALLOWED_ORIGINS=http://localhost:3000

SNOWFLAKE_HOST=xxx.snowflakecomputing.com
SNOWFLAKE_PAT=xxx   # or SNOWFLAKE_PASSWORD=xxx if required
SNOWFLAKE_USER=xxx@xxx.com
SNOWFLAKE_ROLE=xxx
SNOWFLAKE_WAREHOUSE=xxx

SNOWFLAKE_DATABASE=xxx
SNOWFLAKE_SCHEMA=xxx
B. Frontend (.env.local in project root)
Copy env.frontend.example to .env.local and set:

text
REACT_APP_BACKEND_URL=http://localhost:3001
5. Run the Application
Set the Node.js path if not globally installed:

text
set PATH=C:\Users\yourname\nodejs;%PATH%
Start both frontend & backend together:

text
npm run start:all
Or in two terminals:

text
npm run start:server
npm run start:client
6. Access the UI
Go to: http://localhost:3000

7. Common Issues
npm not recognized: Set your PATH to Node.js for each session

503 Service Unavailable: Check .env values for typos and validity

PAT vs PASSWORD: Use the variable name required by your backend

8. Customizing the UI—Header, Branding, And Font
A. Change Logo/Text
Locate header, usually in src/components/Header.tsx or App.tsx

Replace or remove logo, company name, or tagline

B. Use Space Grotesk Font Everywhere
In public/index.html, add in <head>:

xml
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;700&display=swap" rel="stylesheet">
In Material-UI theme file (e.g. theme.ts):

typescript
import { createTheme } from '@mui/material/styles';
const theme = createTheme({
  typography: {
    fontFamily: "'Space Grotesk', Arial, Helvetica, sans-serif",
  },
  // ...other theme options
});
export default theme;
9. Build for Production (Optional)
text
npm run build
Update REACT_APP_BACKEND_URL in .env.local if backend URL changes.

10. Additional Customizations
Edit files in src/styles/ for CSS

Change layouts and colors in theme or CSS files

## Table of Contents
- [Prerequisites](#prerequisites)
- [Setup Steps](#setup-steps)
    - [1. Clone Repo](#1-clone-repo)
    - [2. Install Dependencies](#2-install-dependencies)
    - [3. Configure Environment Variables](#3-configure-environment-variables)
- [Launch Application](#launch-application)
    - [Demo](#demo)
    - [Usage](#usage)
    - [Implemented Features](#implemented-features)
    - [Future Enhancements](#future-enhancements)
    - [Common Issues](#common-issues)
- [Optimized Build](#optimized-build)
- [Questions](#questions)

---

## Prerequisites

- **Node.js** >= 18.0.0
- **npm** >= 9.0.0
- **Snowflake Account** 
    - With ACCOUNTADMIN role 
    - Personal Access Token (PAT) created for authentication
    - Access to Snowflake Intelligence and at least one Agent

        NOTE: If you have not created an agent or do not have access to one, follow this [step-by-step guide](https://quickstarts.snowflake.com/guide/getting-started-with-snowflake-intelligence/index.html#0). You can then use the same account and interact with that agent in this application.

## Setup Steps

### 1. Clone Repo

Clone this repository to get the required files.

* Folders
    - `src/` 
    - `server/` 
 
* Files
    - `craco.config.js`
    - `package.json`
    - `tsconfig.json`
    - `env.backend.example`
    - `env.frontend.example`

    > #### Expand to see the folder structure and files details.
    <details>

    ```
    ├── src/                                   # Frontend React application
    │   ├── components/                        # UI Components
    │   │   ├── Main.tsx                       # Main chat interface
    │   │   ├── ChartVisualization.tsx         # Chart rendering with Recharts
    │   │   ├── ThemeToggle.tsx                # Dark/Light mode toggle
    │   │   └── chat/                          # Chat-specific components
    │   │       ├── AnnotationsSection.tsx     # Annotations display section
    │   │       ├── ChatHeader.tsx             # Header with agent selector
    │   │       ├── ChatInput.tsx              # Message input with voice support
    │   │       ├── ChatMessage.tsx            # Message display with markdown
    │   │       ├── ChartsSection.tsx          # Chart visualization section
    │   │       ├── EmptyState.tsx             # Empty chat state
    │   │       ├── MarkdownFormatter.tsx      # Markdown rendering
    │   │       ├── SqlQueriesSection.tsx      # SQL query display
    │   │       ├── StarterQuestions.tsx       # Suggested questions
    │   │       ├── ThinkingSteps.tsx          # Agent thinking visualization
    │   │       └── index.ts                   # Component exports
    │   │
    │   ├── hooks/                             # Custom React hooks
    │   │   ├── useAccordionState.ts           # Accordion state management
    │   │   ├── useAgentConfig.ts              # Agent configuration & loading
    │   │   ├── useChatMessages.ts             # Message handling & streaming
    │   │   └── useSpeechRecognition.ts        # Voice input (Web Speech API)
    │   │
    │   ├── services/                          # API services
    │   │   └── snowflakeAgentsApi.ts          # Backend proxy API calls
    │   │
    │   ├── types/                             # TypeScript type definitions
    │   │   ├── chat.ts                        # Chat message types
    │   │   └── chart.ts                       # Chart data types
    │   │
    │   ├── utils/                             # Utility functions
    │   │   └── chatUtils.ts                   # Chat helper functions
    │   │
    │   ├── contexts/                          # React contexts
    │   │   └── ThemeContext.tsx               # Theme state management
    │   │
    │   ├── constants/                         # App constants
    │   │   └── textConstants.ts               # UI text & messages
    │   │
    │   ├── theme/                             # MUI theme configuration
    │   │   └── theme.ts                       # Dark/Light theme definitions
    │   │
    │   ├── config/                            # App configuration
    │   │   └── env.ts                         # Environment variable validation
    │   │
    │   └── index.tsx                          # App entry point with ErrorBoundary
    │
    ├── server/                                # Backend proxy server (Node.js/Express)
    │   ├── server.js                          # Express server (PAT secured here)
    │   └── constants.js                       # Server constants & error messages
    │
    ├── public/                                # Static assets
    │   ├── images/                            # Logos and icons
    │   │   ├── icons/                         # App icons
    │   │   │   └── dash_snowboard_512.png     # Alternative app icon
    │   │   ├── logos/                         # Brand logos
    │   ├── index.html                         # HTML template
    │   ├── manifest.json                      # PWA manifest
    │   ├── robots.txt                         # SEO configuration
    │   └── _headers                           # Security headers for deployment
    │
    ├── package.json                           # Dependencies & npm scripts
    ├── tsconfig.json                          # TypeScript configuration
    │
    ├── env.backend.example                    # Backend environment template
    ├── env.frontend.example                   # Frontend environment template
    │
    ├── README.md                              # Main documentation (you are here!)
    ```
    </details>

### 2. Install Dependencies

Browse to the cloned repo folder on your local machine. Then, run the following command in a terminal window.

```bash
npm install
```

This creates the `node_modules/` folder with all required packages.

### 3. Configure Environment Variables

* Copy `env.backend.example` → `.env`

    > NOTE: Update `.env` and add your Snowflake account details

* Copy `env.frontend.example` → `.env.local`

## Launch Application

Browse to the cloned repo folder on your local machine. Then, run the following command in a terminal window to launch both the frontend application and the backend server.

```bash
npm run start:all
```

The application will automatically launch on [http://localhost:3000](http://localhost:3000) in a browser window

### Demo

xxx upload video here 

### Usage 

* Click on one of the starter questions (if any)
* Type in a question/prompt in the text input field in the footer
* To enable voice dictation, click the microphone icon. You will need to grant the necessary audio permissions to your browser. Once permissions are active, you can begin speaking your prompt.

### Implemented Features

* Agent selector
* Example questions (if any) for the selected agent 
* Streaming responses
* Charts and tables
* Citations
* Executed SQL(s) with "Answer accuracy verified by agent owner" indicated when applicable 
* Audio prompt (requires microphone access in the browser)
* Copy-to-clipboard and/or ask the same question
* Copy-to-clipboard the final response
* Light and dark themes

### Future Enhancements

* Threading

### Common Issues

1. **Cannot connect to backend** - Make sure backend server is running: `npm run start:server`

2. **HTTP 400 Bad Request** - Check your SNOWFLAKE_DATABASE and/or SNOWFLAKE_SCHEMA in the backend `.env` file. Make sure they exist and you have access to them. After making changes, restart the development server.

3. **HTTP 401 (Unauthorized) or 403 (Forbidden)** - Check your SNOWFLAKE_PAT (Personal Access Token) in the backend `.env` file. Make sure it's valid and not expired. After making changes, restart the development server.

4. **HTTP 503 Service Unavailable** - Check your SNOWFLAKE_HOST in the backend `.env` file. Make sure it's correct and accessible (format: account.snowflakecomputing.com). After making changes, restart the development server. 

5. **Connection lost during streaming** - The backend server stopped or crashed, network connection was interrupted, or the backend server is no longer running.

6. **CORS errors** - Verify `ALLOWED_ORIGINS` in backend `.env` includes your frontend URL

7. **Voice input not working** - Voice input requires:
   - Chrome, Edge, or Safari browser (not supported in Firefox)
   - Microphone permissions granted to the browser
   - HTTPS connection (required by browser for security)

8. **Port Already in Use**

    If you get an error that port 3000 or 3001 is already in use:

    - Kill process on port 3000 (Frontend):

        ```bash
        # macOS/Linux
        lsof -ti:3000 | xargs kill -9

        # Windows (PowerShell)
        Get-Process -Id (Get-NetTCPConnection -LocalPort 3000).OwningProcess | Stop-Process -Force
        ```

    - Kill process on port 3001 (Backend):

        ```bash
        # macOS/Linux
        lsof -ti:3001 | xargs kill -9

        # Windows (PowerShell)
        Get-Process -Id (Get-NetTCPConnection -LocalPort 3001).OwningProcess | Stop-Process -Force
        ```

    - Kill both ports at once (macOS/Linux):

        ```bash
        lsof -ti:3000,3001 | xargs kill -9
        ```

## Optimized Build

Browse to the cloned repo folder on your local machine. Then, run the following command in a terminal window.

```bash
npm run build
```

This creates an optimized build in the `/build` folder with:

- Minified JavaScript bundles
- No source maps (GENERATE_SOURCEMAP=false)
- Optimized assets
- Security headers (_headers file)

If you're going to run the backend on a different service/server/platform, then build using the following:

```bash
REACT_APP_BACKEND_URL=<YOUR_BACKEND_URL> npm run build
```

Examples:

```bash
REACT_APP_BACKEND_URL=https://my-service.onrender.com npm run build
REACT_APP_BACKEND_URL=https://my-api.herokuapp.com npm run build
REACT_APP_BACKEND_URL=https://api.railway.app npm run build
REACT_APP_BACKEND_URL=https://my-function.vercel.app npm run build
```

## Questions

If you have any questions, comments or feedack, reach out to [Dash DesAI](https://www.linkedin.com/in/dash-desai/).




