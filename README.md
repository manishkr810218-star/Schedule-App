# 🎬 Shadows in the Machine — Production Schedule App

Weekly YouTube production command center for Manish's documentary channel.

---

## 🚀 Deploy in 3 Ways

### Option 1 — Netlify (Easiest, Free)
1. Go to [netlify.com](https://netlify.com) → Sign up free
2. Click **"Add new site"** → **"Deploy manually"**
3. Run these commands on your computer:
   ```
   npm install
   npm run build
   ```
4. Drag the `dist/` folder into Netlify → Live in 30 seconds
5. Your URL: `https://your-site-name.netlify.app`

---

### Option 2 — Vercel (Free, Auto-deploy)
1. Go to [vercel.com](https://vercel.com) → Sign up free
2. Install Vercel CLI:
   ```
   npm install -g vercel
   ```
3. In this folder, run:
   ```
   npm install
   vercel
   ```
4. Follow the prompts → Live instantly
5. Your URL: `https://your-project.vercel.app`

---

### Option 3 — GitHub Pages (Free)
1. Push this folder to a GitHub repo
2. Run: `npm install && npm run build`
3. Go to repo Settings → Pages → Deploy from `dist/` branch
4. Your URL: `https://yourusername.github.io/repo-name`

---

## 💻 Run Locally
```bash
npm install
npm run dev
```
Opens at: `http://localhost:5173`

---

## 📁 File Structure
```
schedule-deploy/
├── index.html          ← Entry point
├── package.json        ← Dependencies
├── vite.config.js      ← Build config
├── src/
│   ├── main.jsx        ← React mount
│   └── App.jsx         ← Full app
└── README.md           ← This file
```

---

## ✅ Requirements
- Node.js 18+ installed on your computer
- Download from: https://nodejs.org

---

Built for: Manish · Shadows in the Machine Documentary Channel
