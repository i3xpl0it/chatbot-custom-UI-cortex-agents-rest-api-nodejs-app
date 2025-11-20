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







