# TechFest 2026 Event Registration Portal

A full-stack MERN project following the supplied lab guide:
- React + Vite frontend (`client/`)
- Express + Node.js API (`server/`)
- MongoDB Atlas database
- GitHub source control
- Render backend deployment
- Netlify frontend deployment

## Features
- Browse TechFest events
- Register students for an event
- View registrations
- Edit and delete registrations
- Server-side validation and error handling
- Health-check endpoint at `/api/health`

## Requirements
Install Node.js LTS and Git. Create a MongoDB Atlas cluster and database user.

## 1. Run locally

### Backend
Open a terminal in the project root:

```bash
cd server
npm install
```

Copy `.env.example` to `.env` and update `MONGO_URI`:

```env
PORT=5000
MONGO_URI=mongodb+srv://<username>:<password>@<cluster-url>/techfest2026?retryWrites=true&w=majority
CLIENT_ORIGIN=http://localhost:5173
NODE_ENV=development
```

Start the API:

```bash
npm run dev
```

Check `http://localhost:5000/api/health`. Expected response: `{"status":"ok"}`.

### Frontend
Open a second terminal from the project root:

```bash
cd client
npm install
```

Copy `client/.env.example` to `client/.env`:

```env
VITE_API_URL=http://localhost:5000
```

Start React:

```bash
npm run dev
```

Open the local URL printed by Vite (normally `http://localhost:5173`).

## 2. MongoDB Atlas
1. Create a project and free-tier database deployment in MongoDB Atlas.
2. Create a database user with a strong password.
3. In **Network Access**, allow your current IP for local development.
4. Copy the Node.js driver connection string and use it as `MONGO_URI`.
5. Replace special characters in the username/password with URL-encoded values if needed.
6. Keep credentials in `server/.env`; never put them in React or commit them to Git.

## 3. GitHub
From the project root:

```bash
git init
git add .
git status
git commit -m "Initial TechFest MERN project"
git branch -M main
git remote add origin https://github.com/<USERNAME>/<REPOSITORY>.git
git push -u origin main
```

Create an empty GitHub repository first. Check `git status` and confirm no `.env` files or secrets are staged.

## 4. Deploy backend on Render
Create a **Web Service** from your GitHub repository:
- Root Directory: `server`
- Build Command: `npm install`
- Start Command: `npm start`

Add these environment variables in Render:
- `NODE_ENV` = `production`
- `MONGO_URI` = your complete MongoDB Atlas connection string
- `CLIENT_ORIGIN` = your actual Netlify site origin (set after frontend deployment)

Deploy, then verify:
`https://<your-render-service>.onrender.com/api/health`

For Atlas access, configure Network Access for the backend's outbound IP ranges where practical. Avoid leaving broad network access enabled in production.

## 5. Deploy frontend on Netlify
Import the GitHub repository as a new site:
- Base directory: `client`
- Build command: `npm run build`
- Publish directory: `dist`

Add environment variable:
- `VITE_API_URL` = `https://<your-render-service>.onrender.com` (no trailing slash)

Deploy the site. Copy its production origin, then update Render's `CLIENT_ORIGIN` to that exact origin and redeploy/restart the backend. Rebuild/redeploy the frontend after changing `VITE_API_URL`.

## 6. Verify deployment
- Netlify site loads over HTTPS.
- Render `/api/health` returns `status: ok`.
- Register a student and verify it appears in the list.
- Refresh the page; records should still be present.
- Edit and delete a registration.
- Check Render logs if the API cannot connect to Atlas.

## API routes
- `GET /api/health` — health check
- `GET /api/registrations` — list registrations
- `POST /api/registrations` — create registration
- `GET /api/registrations/:id` — get one registration
- `PUT /api/registrations/:id` — update registration
- `DELETE /api/registrations/:id` — delete registration

## Important notes
- The guide's deployment provider settings are included, but you must create your own GitHub, Atlas, Render, and Netlify accounts and enter your own URLs/credentials.
- This starter is for a student project. Before real-world use, add authentication/authorization, rate limiting, stronger validation, and privacy controls.
