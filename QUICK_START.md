# 🚀 FitLine-Gym: Quick Start Guide

## Prerequisites

- **Node.js** (v18 or higher) - [Download here](https://nodejs.org/)
- **npm** (comes with Node.js) or **bun** (optional, faster)
- **Firebase Account** - [Get one here](https://firebase.google.com/)

---

## 📦 Step 1: Install Dependencies

Open your terminal in the project directory and run:

```bash
npm install
```

Or if you prefer bun (faster):

```bash
bun install
```

---

## 🔥 Step 2: Set Up Firebase

1. **Create a Firebase Project:**
   - Go to [Firebase Console](https://console.firebase.google.com/)
   - Click "Add Project"
   - Follow the setup wizard

2. **Enable Authentication (IMPORTANT!):**
   - In Firebase Console, go to **Authentication** → **Sign-in method**
   - Click on **Email/Password**
   - Toggle **Enable** to ON
   - Click **Save**
   - Click on **Google**
   - Toggle **Enable** to ON
   - Enter your support email (or project email)
   - Click **Save**
   
   **⚠️ If you skip this step, you'll get "operation-not-allowed" errors!**

3. **Set Up Firestore:**
   - Go to **Firestore Database** → **Create database**
   - Start in **test mode** (for development)
   - Choose your region

4. **Get Your Firebase Config:**
   - Go to **Project Settings** (gear icon) → **General** tab
   - Scroll to "Your apps" → Click the **Web** icon (`</>`)
   - Register your app and copy the config values

---

## ⚙️ Step 3: Create Environment File

Create a file named `.env.local` in the project root:

```bash
# Create the file
touch .env.local
```

Add your Firebase configuration:

```env
VITE_FIREBASE_API_KEY=your_api_key_here
VITE_FIREBASE_AUTH_DOMAIN=your_project_id.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project_id.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id
VITE_FIREBASE_MEASUREMENT_ID=your_measurement_id

# Optional: AI Endpoint (for enhanced AI coach)
# VITE_AI_ENDPOINT=https://your-ai-api.com/chat
```

**Replace all `your_*` values with your actual Firebase config values.**

---

## 🏃 Step 4: Run the Development Server

```bash
npm run dev
```

The app will start at: **http://localhost:5173** (or the port shown in terminal)

You should see:
```
  VITE v5.x.x  ready in xxx ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
```

---

## 🎯 Step 5: Test the Application

1. **Open your browser** to `http://localhost:5173`
2. **Sign up** with email/password or Google OAuth
3. **Complete your profile** in the Setup wizard
4. **Start a workout** and test all features!

---

## 📝 Available Commands

| Command | Description |
|---------|-------------|
| `npm run dev` | Start development server with hot reload |
| `npm run build` | Build for production |
| `npm run preview` | Preview production build locally |
| `npm run lint` | Check code for errors |

---

## 🐛 Troubleshooting

### Port Already in Use
```bash
# Kill process on port 5173
lsof -ti:5173 | xargs kill -9

# Or use a different port
npm run dev -- --port 3000
```

### Firebase Errors
- ✅ Check that `.env.local` exists and has correct values
- ✅ Verify Firebase Authentication is enabled
- ✅ Ensure Firestore is created and in test mode
- ✅ Check browser console for specific error messages

### Module Not Found Errors
```bash
# Clear cache and reinstall
rm -rf node_modules package-lock.json
npm install
```

### TypeScript Errors
```bash
# Check for type errors
npx tsc --noEmit
```

---

## 🚀 Production Build

To build for production:

```bash
npm run build
```

The output will be in the `dist/` folder. You can deploy this to:
- **Vercel** (recommended)
- **Netlify**
- **Firebase Hosting**
- Any static hosting service

---

## 📚 Next Steps

- ✅ Complete your profile in the app
- ✅ Try the AI Coach (bottom-right chat icon)
- ✅ Complete a workout session
- ✅ Check your progress on the Dashboard
- ✅ Explore the Profile page for nutrition plans

---

## 💡 Tips

- The AI Coach works **without** an external API (uses local fallback)
- All workout data is saved to Firestore in real-time
- Water intake tracker uses localStorage (per browser)
- Dark mode toggle coming soon! 🌙

---

**Need Help?** Check the `ALL-IN-ONE-DOCUMENTATION.md` for complete feature documentation.

