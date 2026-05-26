# 💸 Spendora — Personal Expense Tracker

একটি ব্যক্তিগত খরচ ট্র্যাকার অ্যাপ যেখানে প্রতিটি user তার নিজের income ও expense আলাদাভাবে track করতে পারে।

---

## 🌐 Live Demo

| Platform | URL |
|----------|-----|
| Frontend (Netlify) | https://my-spendora-f1d7e3.netlify.app |
| Backend (Vercel)   | https://my-spendora-server.vercel.app  |

---

## ✨ Features

- 🔐 **Firebase Authentication** — Email/Password এবং Google Login
- 👤 **Per-user Data** — প্রতিটি user শুধু তার নিজের expense দেখতে পারে
- ➕ **Expense & Income Add** — Category, date, description সহ
- 📊 **Charts & Analytics** — Monthly spending trend, category donut chart, income vs expense
- 🗂️ **Transactions Page** — Search, filter by category, delete
- 👤 **Profile Page** — Avatar upload, bio, social links, account details
- 🌙 **Dark / Light Mode** — Theme toggle
- 📱 **Fully Responsive** — Mobile, tablet, desktop সব জায়গায় কাজ করে

---

## 🛠️ Tech Stack

### Frontend
| Technology | কাজ |
|------------|-----|
| React 18 | UI framework |
| Vite | Build tool |
| React Router v6 | Client-side routing |
| Firebase Auth | Login / Logout |
| Firestore | User profile data |
| Firebase Storage | Avatar upload |
| Framer Motion | Animations |
| React Icons | Icon library |
| React Helmet Async | Page titles |

### Backend
| Technology | কাজ |
|------------|-----|
| Node.js + Express | REST API server |
| MongoDB Atlas | Database |
| Mongoose / Native Driver | DB queries |
| CORS | Cross-origin requests |
| dotenv | Environment variables |

---

## 📁 Project Structure

```
spendora/
├── client/                   # Frontend (React + Vite)
│   ├── src/
│   │   ├── components/
│   │   │   └── PrivateRoute.jsx      # Login ছাড়া ঢোকা যাবে না
│   │   ├── context/
│   │   │   ├── AuthContext.jsx       # Firebase user state
│   │   │   ├── ExpenseContext.jsx    # Expense CRUD
│   │   │   └── ThemeContext.jsx      # Dark/light theme
│   │   ├── hooks/
│   │   │   └── userProfile.js        # Profile read/write hook
│   │   ├── pages/
│   │   │   ├── dashboard/
│   │   │   │   ├── Dashboard.jsx     # Stats + charts overview
│   │   │   │   ├── Overview.jsx      # Greeting + summary cards
│   │   │   │   ├── AddExpense.jsx    # Expense/income form
│   │   │   │   ├── Transactions.jsx  # List + search + delete
│   │   │   │   ├── Charts.jsx        # Visual analytics
│   │   │   │   └── Profile.jsx       # User profile editor
│   │   │   └── DashboardLayout.jsx   # Sidebar + topbar layout
│   │   ├── constants.js              # Categories, months
│   │   └── main.jsx                  # Router + providers
│   └── .env                          # Frontend env variables
│
└── server/                   # Backend (Express)
    ├── server.js             # All API routes
    ├── vercel.json           # Vercel deployment config
    └── .env                  # Backend env variables
```

---


---

## 🚀 Local এ Run করা

### Backend
```bash
cd server
npm install
npm start
# Server চলবে http://localhost:5000
```

### Frontend
```bash
cd client
npm install
npm run dev
# App চলবে http://localhost:5173
```

---

## 🔒 Authentication Flow

```
User আসে
    ↓
Login আছে? → না → /login page-এ redirect
    ↓ হ্যাঁ
Dashboard দেখায়
    ↓
সব API call-এ uid পাঠায়
    ↓
Backend শুধু সেই uid-এর data return করে
```

প্রতিটি expense MongoDB-তে `uid` field সহ save হয়:
```json
{
  "desc": "Lunch",
  "amt": 150,
  "type": "expense",
  "cat": "Food",
  "date": "2025-05-26",
  "uid": "firebase_user_uid_here",
  "createdAt": "2025-05-26T10:00:00.000Z"
}
```

Backend সব query-তে `uid` দিয়ে filter করে, তাই একজন user আরেকজনের data দেখতে পারে না।

---

## 📡 API Endpoints

| Method | Endpoint | কাজ |
|--------|----------|-----|
| `GET` | `/expenses?uid=` | সব expense আনো |
| `GET` | `/expenses/summary/monthly?uid=` | এই মাসের summary |
| `GET` | `/expenses/recent?uid=&limit=5` | সাম্প্রতিক transactions |
| `POST` | `/expenses` | নতুন expense যোগ করো |
| `PATCH` | `/expenses/:id` | expense আপডেট করো |
| `DELETE` | `/expenses/:id` | expense মুছে ফেলো |

---

## 🚢 Deployment

### Vercel (Backend)
1. `vercel.json` root-এ রাখো
2. Vercel Dashboard → Environment Variables-এ `DB_USER`, `DB_PASS`, `CLIENT_URL` set করো
3. `git push` করলে auto deploy হবে

### Netlify (Frontend)
1. Netlify Dashboard → Environment Variables-এ `VITE_API_URL` এবং সব Firebase keys set করো
2. Build command: `npm run build`
3. Publish directory: `dist`

### MongoDB Atlas
- Network Access → `0.0.0.0/0` allow করো (Vercel-এর IP dynamic)

---

## 👨‍💻 Author

তোমার নাম এখানে লেখো
- GitHub: [Umme-Habiba123] ( https://github.com/Umme-Habiba123 )
- LinkedIn: [ Mahiya Rahman ]( https://www.linkedin.com/in/mahiya-rehman/ )

---

## 📄 License

MIT License — ইচ্ছামতো use করো।
