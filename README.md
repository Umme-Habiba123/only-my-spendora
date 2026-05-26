# Spendora — Personal Expense Tracker

Spendora is a full-stack personal finance app that lets each user track their own income and expenses in one clean, organized dashboard. Every user sees only their own data — nothing shared, nothing mixed.

![React](https://img.shields.io/badge/React-18-61dafb?style=flat-square&logo=react)
![Node.js](https://img.shields.io/badge/Node.js-Express-339933?style=flat-square&logo=node.js)
![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-47A248?style=flat-square&logo=mongodb)
![Firebase](https://img.shields.io/badge/Firebase-Auth-FFCA28?style=flat-square&logo=firebase)

---

## Live Demo

| | URL |
|---|---|
| Frontend | https://my-spendora-f1d7e3.netlify.app |
| Backend API | https://my-spendora-server.vercel.app |

---

## Features

- **Authentication** — Sign up and log in with Email/Password or Google via Firebase
- **Per-user isolation** — Each user's expenses are stored with their unique `uid`; no one can see anyone else's data
- **Add transactions** — Log expenses or income with a description, amount, category, and date
- **Dashboard overview** — Balance, monthly spending, total income, and transaction count at a glance
- **Charts & analytics** — Monthly spending trend, category donut breakdown, income vs. expense comparison
- **Transaction management** — Search by keyword, filter by category, and delete entries
- **Profile page** — Update display name, bio, occupation, location, social links, and avatar
- **Dark / Light mode** — Persisted theme toggle
- **Fully responsive** — Works on mobile, tablet, and desktop

---

## Tech Stack

### Frontend

| Technology | Purpose |
|---|---|
| React 18 | UI framework |
| Vite | Build tooling |
| React Router v6 | Client-side routing |
| Firebase Auth | User authentication |
| Firestore | Profile data storage |
| Firebase Storage | Avatar image uploads |
| Framer Motion | Page and component animations |
| React Icons | Icon library |
| React Helmet Async | Dynamic page titles |

### Backend

| Technology | Purpose |
|---|---|
| Node.js + Express | REST API server |
| MongoDB Atlas | Cloud database |
| MongoDB Node Driver | Database queries |
| CORS | Cross-origin request handling |
| dotenv | Environment variable management |

---

## Project Structure

```
spendora/
├── client/                        # React + Vite frontend
│   ├── src/
│   │   ├── components/
│   │   │   └── PrivateRoute.jsx   # Redirects unauthenticated users to /login
│   │   ├── context/
│   │   │   ├── AuthContext.jsx    # Firebase auth state provider
│   │   │   ├── ExpenseContext.jsx # Expense CRUD operations
│   │   │   └── ThemeContext.jsx   # Dark/light theme provider
│   │   ├── hooks/
│   │   │   └── userProfile.js     # Read and write user profile data
│   │   ├── pages/
│   │   │   ├── DashboardLayout.jsx
│   │   │   └── dashboard/
│   │   │       ├── Overview.jsx       # Financial summary + greeting
│   │   │       ├── Dashboard.jsx      # Stats cards + charts
│   │   │       ├── AddExpense.jsx     # Add expense / income form
│   │   │       ├── Transactions.jsx   # Full transaction list
│   │   │       ├── Charts.jsx         # Visual analytics
│   │   │       └── Profile.jsx        # User profile editor
│   │   ├── constants.js           # Category definitions, month names
│   │   └── main.jsx               # App entry — router + context providers
│   └── .env
│
└── server/                        # Node.js + Express backend
    ├── server.js                  # All API route definitions
    ├── vercel.json                # Vercel serverless config
    └── .env
```

---

## Getting Started

### Prerequisites

- Node.js 18+
- A MongoDB Atlas cluster
- A Firebase project with Auth and Firestore enabled

### 1. Clone the repository

```bash
git clone https://github.com/Umme-Habiba123/spendora.git
cd spendora
```

### 2. Set up the backend

```bash
cd server
npm install
```

Create a `.env` file in `/server`:

```env
DB_USER=your_mongodb_username
DB_PASS=your_mongodb_password
CLIENT_URL=http://localhost:5173
PORT=5000
```

```bash
npm start
# API running at http://localhost:5000
```

### 3. Set up the frontend

```bash
cd client
npm install
```

Create a `.env` file in `/client`:

```env
VITE_API_URL=http://localhost:5000

VITE_apiKey=your_firebase_api_key
VITE_authDomain=your_project.firebaseapp.com
VITE_projectId=your_project_id
VITE_storageBucket=your_project.appspot.com
VITE_messagingSenderId=your_sender_id
VITE_appId=your_app_id
```

```bash
npm run dev
# App running at http://localhost:5173
```

---

## How Per-User Data Works

Every expense is saved to MongoDB with the authenticated user's Firebase `uid`:

```json
{
  "desc": "Lunch",
  "amt": 150,
  "type": "expense",
  "cat": "Food",
  "date": "2025-05-26",
  "uid": "firebase_uid_abc123",
  "createdAt": "2025-05-26T10:00:00.000Z"
}
```

Every API query filters by `uid`, so users can only ever read and modify their own records.

```
User visits the app
        ↓
Firebase checks auth state
        ↓
Not logged in? → Redirect to /login
        ↓
Logged in? → Load dashboard
        ↓
All API requests include uid
        ↓
Server filters MongoDB by uid
        ↓
User sees only their own data
```

---

## API Reference

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/expenses?uid=` | Fetch all expenses for a user |
| `GET` | `/expenses/summary/monthly?uid=` | Current month income, expense, and count |
| `GET` | `/expenses/recent?uid=&limit=5` | Most recent transactions |
| `POST` | `/expenses` | Create a new expense or income entry |
| `PATCH` | `/expenses/:id` | Update an existing entry |
| `DELETE` | `/expenses/:id` | Delete an entry |

---

## Deployment

### Backend — Vercel

Add a `vercel.json` file to the server root:

```json
{
  "version": 2,
  "builds": [{ "src": "server.js", "use": "@vercel/node" }],
  "routes": [{ "src": "/(.*)", "dest": "server.js" }]
}
```

Set these environment variables in your Vercel project settings:

```
DB_USER        → your MongoDB username
DB_PASS        → your MongoDB password
CLIENT_URL     → your Netlify frontend URL
```

### Frontend — Netlify

Set these environment variables in your Netlify site settings:

```
VITE_API_URL             → your Vercel backend URL
VITE_apiKey              → Firebase API key
VITE_authDomain          → Firebase auth domain
VITE_projectId           → Firebase project ID
VITE_storageBucket       → Firebase storage bucket
VITE_messagingSenderId   → Firebase messaging sender ID
VITE_appId               → Firebase app ID
```

Build settings:
- **Build command:** `npm run build`
- **Publish directory:** `dist`

### MongoDB Atlas

Go to **Network Access** and allow connections from anywhere (`0.0.0.0/0`) since Vercel uses dynamic IP addresses.

---

## Author

**Mahiya Rahman**
- GitHub: [@Umme-Habiba123](https://github.com/Umme-Habiba123)
- LinkedIn: [Mahiya Rahman](https://www.linkedin.com/in/mahiya-rehman/)

---

## License

MIT License — free to use, modify, and distribute.
