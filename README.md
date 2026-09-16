# OggyType — Typing Performance Tracker

A full-stack typing test app 

## Features

- **Test** — timed or word-count typing tests (with editable custom lengths), live WPM/accuracy, a WPM-over-time graph on results, real passages pulled from an open API with an automatic local fallback if it's unreachable.
- **Learn** — six guided lessons with a visual keyboard and an on-screen finger/hand guide, per-lesson progress tracking.
- **History** — every completed test is saved to a real database and shown back per-user.
- **Login** — a lightweight, no-password name entry that assigns each browser a unique ID, so everyone's history stays separate.

## Tech stack

- **Frontend:** plain HTML/CSS/JavaScript (no framework) — `public/`
- **Backend:** Node.js + Express — `server.js`
- **Database:** SQLite via `better-sqlite3` — `db.js`
- **Passage source:** [DummyJSON Quotes API](https://dummyjson.com/docs/quotes) (free, no key), with a local passage bank as an automatic fallback

## Running it locally

Requires [Node.js](https://nodejs.org/) (v18 or newer).

```bash
npm install
npm start
```

Then open **http://localhost:3000** in your browser. Express serves both the frontend and the API from the same address — no separate setup needed.

A `oggytype.db` SQLite file is created automatically on first run in the project folder; it's git-ignored so it won't be part of the repo.

## Project structure

```
oggytype/
├── server.js          # Express app — serves the frontend and the API
├── db.js              # SQLite setup (schema + migrations)
├── package.json
└── public/
    ├── index.html
    ├── style.css
    ├── auth.js         # login / unique id assignment
    ├── script.js        # typing test engine
    ├── passages.js       # passage fetching (API + local fallback)
    ├── lessons.js        # Learn module data (keyboard layout, finger map, lessons)
    └── learn.js          # Learn module logic
```

## API routes

| Method | Route              | Description                          |
|--------|---------------------|--------------------------------------|
| POST   | `/api/attempts`      | Save a completed test attempt        |
| GET    | `/api/attempts`      | Get this user's saved attempts       |
| DELETE | `/api/attempts/:id`  | Delete a single saved attempt        |

All requests are scoped to the caller via an `X-Client-Id` header (assigned at login), so one person's history never mixes with another's.

