<div align="center">

# Playtube

A full-stack video platform for **watching, uploading, and managing content** — channels, playlists, studio tools, library, and community posts — built with **React + Vite** and **Node.js + Express + MongoDB**.

[![Live App](https://img.shields.io/badge/Live_App-playtube--six.vercel.app-ef4444?style=for-the-badge)](https://playtube-six.vercel.app/)
[![API](https://img.shields.io/badge/API-Render-46E3B7?style=for-the-badge)](https://playtube-7nj8.onrender.com/api/v1/healthcheck)

[![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-6-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-8-646CFF?logo=vite&logoColor=white)](https://vitejs.dev/)
[![Node.js](https://img.shields.io/badge/Node.js-20+-339933?logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express-5-000000?logo=express&logoColor=white)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-47A248?logo=mongodb&logoColor=white)](https://www.mongodb.com/)

</div>

> **Note:** This is an independent **portfolio / learning** project. It is not affiliated with YouTube or any commercial video service. Upload only content you have the **rights** to share.

---

## Why this project exists

Playtube was built to practice **end-to-end full-stack development**: cookie-based auth with refresh, REST APIs, file uploads to cloud storage, paginated feeds, and a production-minded React UI (toasts, lazy routes, accessibility basics). It demonstrates how a modern SPA and a Node API work together with **MongoDB Atlas** and **Cloudinary** in a real deploy setup (**Vercel + Render**).

---

## Demo

| | URL |
|---|-----|
| **Live app (frontend)** | [https://playtube-six.vercel.app/](https://playtube-six.vercel.app/) |
| **API (backend)** | [https://playtube-7nj8.onrender.com](https://playtube-7nj8.onrender.com) |
| **Health check** | [https://playtube-7nj8.onrender.com/api/v1/healthcheck](https://playtube-7nj8.onrender.com/api/v1/healthcheck) |

The API only defines routes under **`/api/v1/...`**. Opening the API root may show `Cannot GET /` — that is normal. Use the health check link to verify the server.

### Screenshots


#### Home feed

![Home feed](./frontend/public/readme/home.png)

#### Sign in / Register

![Authentication](./frontend/public/readme/registerr.png)

#### Watch page

![Watch page](./frontend/public/readme/watch.png)

#### Studio page

![Studio page](./frontend/public/readme/studio.png)

#### Channel page

![Channel page](./frontend/public/readme/channel.png)



---

## Features

- **Authentication** — Register, login, logout, JWT in HTTP-only cookies, automatic refresh on `401`
- **Home** — Paginated video feed, infinite scroll, search on loaded videos
- **Watch** — HTML5 player, likes, subscribe, comments, watch history, save to playlist
- **Channel** — Profile, banner, videos, playlists, subscribe, **Community** posts
- **Studio** — Dashboard stats, upload, publish toggle, edit and delete videos
- **Playlists** — Create, view, edit; add/remove videos; owner delete
- **Library** — Watch history, liked videos, subscriptions + merged feed
- **Settings** — Profile, password, avatar and cover (multipart)

---

## Tech stack

### Frontend

- React 19, React Router 7, TypeScript
- Vite 8, ESLint
- `react-helmet-async` (per-page titles)

### Backend

- Node.js 20+, Express 5
- Mongoose 9, JWT, Multer, CORS, `cookie-parser`
- Cloudinary (media storage)

### Database & hosting

- **MongoDB Atlas** (database name `playtube` appended by the API)
- **Frontend:** [Vercel](https://vercel.com)
- **Backend:** [Render](https://render.com)

---

## Architecture

```mermaid
flowchart TB
  subgraph browser [Browser]
    SPA[React SPA on Vercel]
  end
  subgraph api [Backend on Render]
    EX[Express /api/v1]
  end
  subgraph services [Services]
    MDB[(MongoDB Atlas)]
    CLD[Cloudinary]
  end
  SPA -->|HTTPS + cookies| EX
  EX --> MDB
  EX --> CLD
```

**Auth flow (summary):** Login sets `accessToken` and `refreshToken` cookies on the API origin. The client uses `fetch` with `credentials: 'include'`. On `401`, the frontend calls refresh, then retries. Cross-origin production (Vercel + Render) uses `COOKIE_SAMESITE=none` and matching `CORS_ORIGIN`.

---

## Project structure

```bash
playtube/
├── backend/                 # Express API
│   ├── src/
│   │   ├── controllers/
│   │   ├── models/
│   │   ├── routes/          # Mounted under /api/v1/*
│   │   ├── middlewares/
│   │   └── app.js
│   ├── .env.sample
│   └── package.json
├── frontend/                # Vite + React SPA
│   ├── public/readme/       # README screenshots (add your PNGs here)
│   ├── src/
│   │   ├── api/             # API client + endpoint helpers
│   │   ├── components/
│   │   ├── context/         # Auth, toasts
│   │   └── pages/
│   ├── .env.example
└── README.md
```

---

## Installation

### Prerequisites

- Node.js 20+
- MongoDB (local or [Atlas](https://www.mongodb.com/atlas))
- [Cloudinary](https://cloudinary.com/) account

### Clone the repository

```bash
git clone https://github.com/Neha-Codes295/playtube.git
cd playtube
```

### Forking (optional)

1. Open [github.com/Neha-Codes295/playtube](https://github.com/Neha-Codes295/playtube).
2. Click **Fork**.
3. Clone your fork: `git clone https://github.com/<your-username>/playtube.git`

### Install dependencies

**Backend**

```bash
cd backend
npm install
```

**Frontend**

```bash
cd ../frontend
npm install
```

---

## Environment variables

Never commit real `.env` files or paste secrets in issues or screenshots.

### Backend — `backend/.env`

Copy from [`backend/.env.sample`](backend/.env.sample):

```env
PORT=8001
NODE_ENV=development
MONGODB_URI=mongodb+srv://USER:PASSWORD@cluster.mongodb.net
CORS_ORIGIN=http://localhost:5173
COOKIE_SAMESITE=lax
ACCESS_TOKEN_SECRET=your_long_random_secret
REFRESH_TOKEN_SECRET=your_other_long_random_secret
ACCESS_TOKEN_EXPIRY=1d
REFRESH_TOKEN_EXPIRY=10d
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=
```

| Variable | Notes |
|----------|--------|
| `MONGODB_URI` | **No** `/playtube` suffix — the app appends `/playtube` |
| `CORS_ORIGIN` | Exact SPA origin (local: `http://localhost:5173`; prod: `https://playtube-six.vercel.app`) |
| `COOKIE_SAMESITE` | `lax` locally; **`none`** on Render when UI is on Vercel |

### Frontend — `frontend/.env` (optional locally)

See [`frontend/.env.example`](frontend/.env.example):

```env
# Local dev: usually leave empty; Vite proxies /api → localhost:8001
# VITE_DEV_API_PROXY=http://localhost:8001

# Production build (Vercel sets this in the dashboard):
# VITE_API_URL=https://playtube-7nj8.onrender.com
```

---

## Run locally

Use **two terminals**.

**Terminal 1 — API**

```bash
cd backend
npm run dev
```

→ [http://localhost:8001](http://localhost:8001)  
→ Health: [http://localhost:8001/api/v1/healthcheck](http://localhost:8001/api/v1/healthcheck)

**Terminal 2 — UI**

```bash
cd frontend
npm run dev
```

→ [http://localhost:5173](http://localhost:5173)

Vite proxies `/api` to the backend. All API calls use `credentials: 'include'` for cookies.

---

## Usage

1. Open the app → **Register** (avatar required) or **Sign in**.
2. **Home** — browse videos; scroll to load more; use search on loaded items.
3. **Watch** — play video; like, comment, subscribe, save to playlist (when signed in).
4. **Upload** / **Studio** — publish and manage your videos (owner).
5. **Channel** — your public page: videos, playlists, community tab.
6. **Library** — history, liked, subscriptions.
7. **Settings** — profile, password, avatar, cover.

---

## API endpoints

Base URL (local): `http://localhost:8001`  
Base URL (production): `https://playtube-7nj8.onrender.com`  

All routes are prefixed with **`/api/v1`**. Auth routes marked 🔒 require a valid session cookie (or Bearer token where supported).

### Health

| Method | Path | Description |
|--------|------|-------------|
| GET | `/healthcheck` | Service health |

### Users & auth

| Method | Path | Description |
|--------|------|-------------|
| POST | `/users/register` | Register (multipart: avatar, optional cover) |
| POST | `/users/login` | Login |
| POST | `/users/logout` | 🔒 Logout |
| POST | `/users/refresh-token` | Refresh access token |
| GET | `/users/current-user` | 🔒 Current user |
| PATCH | `/users/update-account` | 🔒 Update profile |
| POST | `/users/change-password` | 🔒 Change password |
| PATCH | `/users/avatar` | 🔒 Upload avatar |
| PATCH | `/users/cover-image` | 🔒 Upload cover |
| GET | `/users/c/:username` | Channel profile |
| GET/POST | `/users/history` | 🔒 Watch history |

### Videos

| Method | Path | Description |
|--------|------|-------------|
| GET | `/videos` | Public feed (paginated) |
| GET | `/videos/:videoId` | Video detail |
| POST | `/videos` | 🔒 Upload (multipart) |
| GET | `/videos/my` | 🔒 Creator’s videos |
| PATCH | `/videos/:videoId` | 🔒 Update metadata |
| PATCH | `/videos/:videoId/publish` | 🔒 Toggle publish |
| DELETE | `/videos/:videoId` | 🔒 Delete video |

### Comments, likes, playlists, subscriptions, tweets, dashboard

| Area | Highlights |
|------|------------|
| **Comments** | `GET/POST /comments/:videoId`, `PATCH/DELETE /comments/c/:commentId` |
| **Likes** | `POST /likes/v/:videoId`, `GET /likes/videos`, comment/tweet likes |
| **Playlists** | `POST /playlists`, `GET /playlists/u/:userId`, `GET/PATCH/DELETE /playlists/:id`, add/remove videos |
| **Subscriptions** | `GET /subscriptions`, `POST /subscriptions/:channelId` |
| **Tweets** | `GET /tweets/u/:userId`, `POST /tweets`, `PATCH/DELETE /tweets/:tweetId` |
| **Dashboard** | `GET /dashboard/stats` 🔒 |

---

## Deployment

| Layer | Platform | URL |
|-------|----------|-----|
| Frontend | Vercel | [playtube-six.vercel.app](https://playtube-six.vercel.app/) |
| Backend | Render | [playtube-7nj8.onrender.com](https://playtube-7nj8.onrender.com) |
| Database | MongoDB Atlas | Connection string in Render env |

**Order:** deploy **API first** → set **`VITE_API_URL`** on Vercel → set **`CORS_ORIGIN`** on Render to your Vercel URL → redeploy API if needed.

Full checklist: **[`docs/DEPLOY.md`](docs/DEPLOY.md)**

---

## Scripts

| Directory | Command | Description |
|-----------|---------|-------------|
| `backend/` | `npm run dev` | Development (nodemon) |
| `backend/` | `npm start` | Production start |
| `frontend/` | `npm run dev` | Vite dev server |
| `frontend/` | `npm run build` | Production build → `dist/` |
| `frontend/` | `npm run preview` | Preview production build |
| `frontend/` | `npm run lint` | ESLint |

---

## Challenges faced

- **Cross-origin cookies** — Vercel (UI) and Render (API) require `CORS_ORIGIN`, `COOKIE_SAMESITE=none`, and `credentials: 'include'` on the client.
- **Media uploads** — Multipart video + thumbnail through Multer to Cloudinary; balancing payload size and free-tier cold starts on Render.
- **Feed pagination** — Merging pages on the client without duplicate cards; infinite scroll vs explicit “load more” on filtered home search.
- **Optional auth on public routes** — Channel and video detail use `optionalAuth` so guests can browse while signed-in users get subscription hints.

---

## Contributing

Pull requests are welcome. For large changes, open an issue first to discuss what you would like to change.

1. Fork the repository  
2. Create a feature branch (`git checkout -b feature/amazing-feature`)  
3. Commit your changes  
4. Push to the branch and open a Pull Request  

Please do not commit `.env` files or production secrets.

---

## License

ISC — see [`backend/package.json`](backend/package.json). Add a root `LICENSE` file if you redistribute the project.

---

## Author

Built by [Neha-Codes295](https://github.com/Neha-Codes295).
