# FitLine-Gym: Quick Reference Guide

## 🎯 30-Second Overview

**FitLine-Gym** is a personalized fitness web app that provides:
- 🏋️ 9 curated workouts across 4 difficulty levels
- 🤖 AI fitness coach with real-time guidance
- 📊 Progress tracking & analytics
- ⏱️ Interactive workout timer with voice guidance
- 👤 Smart personalization based on user profile
- 📱 Fully responsive mobile design

Built with **React 18 + TypeScript + Firebase + Tailwind CSS**

---

## 📁 File Organization Quick Map

```
src/
├── components/
│   ├── AICoach.tsx ..................... Floating chatbot widget
│   ├── AnimatedExercise.tsx ............ Exercise visualizations
│   ├── Auth.tsx ........................ Login/signup forms
│   ├── ExerciseVideoModal.tsx .......... Video player modal
│   └── ui/ ............................ 30+ UI component library
│
├── contexts/
│   └── AuthContext.tsx ................ Global auth state management
│
├── lib/
│   ├── ai.ts .......................... AI coach intelligence
│   ├── firebase.ts .................... Firebase configuration
│   ├── workouts.ts .................... Workout database & algorithms
│   └── utils.ts ....................... Utility functions
│
├── pages/
│   ├── Landing.tsx .................... Public home page
│   ├── Auth.tsx ....................... Authentication page
│   ├── Dashboard.tsx .................. Main hub (protected)
│   ├── Setup.tsx ...................... 5-step profile wizard
│   ├── DailyWorkout.tsx ............... Workout selection page
│   ├── WorkoutSession.tsx ............. Active workout with timer
│   ├── Profile.tsx .................... Settings & metrics
│   └── NotFound.tsx ................... 404 page
│
├── hooks/
│   ├── use-mobile.tsx ................. Mobile detection
│   └── use-toast.ts ................... Notification hook
│
├── App.tsx ........................... Main router & layout
├── main.tsx .......................... React entry point
├── index.css ......................... Global styles & design system
└── App.css ........................... App-specific styles
```

---

## 🚀 Quick Start Commands

```bash
# Development
npm install              # Install dependencies
npm run dev             # Start dev server (localhost:8080)
npm run lint            # Run ESLint

# Production
npm run build           # Build for production
npm run preview         # Preview production build

# Environment Setup
# Create .env.local with:
# VITE_FIREBASE_API_KEY=...
# VITE_FIREBASE_AUTH_DOMAIN=...
# VITE_FIREBASE_PROJECT_ID=...
# ... (6 total Firebase vars)
# VITE_AI_ENDPOINT=... (optional)
```

---

## 🔐 Authentication Flow

| Step | Action | Result |
|------|--------|--------|
| 1 | User visits `/auth` | Auth page (email or Google) |
| 2 | Submit credentials | Firebase Auth processes |
| 3 | Success | Firestore user doc created |
| 4 | AuthContext updates | Real-time listener detects |
| 5 | Auto-redirect | Navigate to `/dashboard` |
| 6 | Dashboard loads | Check profile completeness |

---

## 🏋️ Workout System

### 4 Intensity Levels:

| Type | Duration | Difficulty | Use Case |
|------|----------|-----------|----------|
| **Quickie** | 5-10 min | ⭐⭐ | Busy days, quick bursts |
| **Classic** | 20-30 min | ⭐⭐⭐ | Daily routine, balanced |
| **Power** | 45-60 min | ⭐⭐⭐⭐ | Serious sessions |
| **Beast** | 90+ min | ⭐⭐⭐⭐⭐ | Maximum intensity |

### Personalization Algorithm:

```
User Profile + Workout Type
    ↓
Filter by Experience (beginner/intermediate/advanced)
    ↓
Filter by Goal (weight_loss/muscle_gain/endurance/general)
    ↓
Rotate by Day of Week (ensures variety)
    ↓
Return Personalized Workout
```

---

## 🤖 AI Coach System

### Two-Tier AI Approach:

**Tier 1: External API** (if `VITE_AI_ENDPOINT` set)
- POST request with full message context
- Supports any LLM backend

**Tier 2: Local Heuristic** (fallback, always available)
- Pattern matching on user queries
- Template responses
- No API key needed
- Zero latency

### What AI Can Help With:

- ✅ Last workout details
- ✅ Next workout suggestions
- ✅ Progress analytics
- ✅ Beginner guidance
- ✅ Goal-specific training
- ✅ Injury recovery tips
- ✅ Motivation & encouragement

---

## 📊 Database Schema (Firestore)

### Users Collection

```typescript
users/{uid}
├── uid: string
├── email: string
├── displayName: string
├── age: number
├── gender: "male" | "female" | "other"
├── height: number (cm)
├── weight: number (kg)
├── fitnessGoal: "weight_loss" | "muscle_gain" | "endurance" | "general"
├── experienceLevel: "beginner" | "intermediate" | "advanced"
├── preferredWorkoutTime: "morning" | "afternoon" | "evening"
├── intensityLevel: "quickie" | "classic" | "power" | "beast"
├── createdAt: Timestamp
└── updatedAt: Timestamp

// Subcollection: Workout Completions
completions/{dateKey}
├── date: Timestamp
├── dateKey: string (YYYY-MM-DD)
├── workoutType: string
├── workoutId: string
├── duration: string
├── calories: string
└── completed: boolean
```

---

## 🎨 Design System Colors

| Element | Color | Usage |
|---------|-------|-------|
| Primary | Orange (#FF7A00) | Buttons, accents, hero |
| Background | Light Grey (#FAF9F7) | Page background |
| Foreground | Dark Grey (#161616) | Text |
| Success | Green | BMI normal, progress |
| Warning | Orange | BMI overweight |
| Danger | Red | BMI obese, countdown |

---

## ⏱️ Workout Session Features

### Timer System:
- ✅ Work phase (exercise duration)
- ✅ Rest phase (between sets)
- ✅ Auto-advance (configurable)
- ✅ Voice announcements (optional)
- ✅ Manual controls (skip, previous, restart)

### Exercise Display:
- ✅ Current exercise name & index
- ✅ Sets and reps/duration
- ✅ Animated exercise visualization
- ✅ Video guide link
- ✅ Progress bar (overall & per-exercise)

### Key Controls:
| Button | Action |
|--------|--------|
| ▶️ Play/Pause | Start/pause timer |
| ⏭️ Skip | Next exercise |
| ⏮️ Previous | Go back one exercise |
| 🔄 Restart | Restart current exercise |
| 📹 Video | Open exercise guide |
| 🔊 Voice | Toggle audio guidance |

---

## 📈 Profile Metrics

### BMI Calculator
```
Formula: weight(kg) / height(m)²

Categories:
- < 18.5: Underweight (blue)
- 18.5-24.9: Normal (green)
- 25-29.9: Overweight (yellow)
- ≥ 30: Obese (red)
```

### Calorie Calculator (Harris-Benedict)
```
Male BMR: 88.362 + (13.397 × weight) + (4.799 × height) - (5.677 × age)
Female BMR: 447.593 + (9.247 × weight) + (3.098 × height) - (4.330 × age)

TDEE: BMR × 1.55 (moderate activity)

Recommendations:
- Maintenance: TDEE
- Weight loss: TDEE - 500
- Muscle gain: TDEE + 300
```

---

## 🛣️ Route Map

```
/ ......................... Landing (Public)
  └─ Hero, features, testimonials

/auth ..................... Authentication (Public)
  ├─ Login Tab
  └─ Signup Tab

/setup .................... Profile Setup (Protected)
  └─ 5-step wizard

/dashboard ................ Main Hub (Protected)
  └─ Weekly progress, recommendations

/daily-workout ............ Workout Selection (Protected)
  └─ Browse & select workouts

/workout-session .......... Active Timer (Protected)
  └─ Full exercise-by-exercise guidance

/profile .................. Settings & Metrics (Protected)
  ├─ Profile editor
  ├─ BMI calculator
  └─ Calorie calculator

/* ....................... 404 Not Found
```

---

## 🔑 Key Hooks & Utilities

### Custom Hooks:

```typescript
// Global authentication
const { user, userProfile, loading, logout, refreshProfile } = useAuth();

// Toast notifications
const { toast } = useToast();

// Mobile detection
const isMobile = useMobile();
```

### Utility Functions:

```typescript
// Class merging (tailwind + custom)
cn("px-4", condition && "bg-primary")

// Workout personalization
getPersonalizedWorkout(profile, type, dayOfWeek)
getRecommendedWorkoutType(profile)

// Formatting
getDifficultyColor(difficulty)
getDifficultyLabel(difficulty)

// AI Coach
askAICoach(input, history, profile, progress)
```

---

## 🎯 Component Dependencies

### Top-Level Providers:
```tsx
<QueryClientProvider>
  <AuthProvider>
    <TooltipProvider>
      <BrowserRouter>
        {routes}
        <AICoach />
      </BrowserRouter>
    </TooltipProvider>
  </AuthProvider>
</QueryClientProvider>
```

### Auth Protection:
```tsx
<ProtectedRoute>
  {children}  // Only renders if user authenticated
</ProtectedRoute>

<PublicRoute>
  {children}  // Redirects to dashboard if authenticated
</PublicRoute>
```

---

## 🔄 Data Flow Summary

```
Landing Page
    ↓
User Signup/Login (Firebase Auth)
    ↓
Firestore User Document Created
    ↓
AuthContext Listener Detects Change
    ↓
Dashboard (Real-time Profile Sync)
    ↓
Profile Setup (if needed)
    ↓
Personalized Recommendations
    ↓
Select Workout → DailyWorkout
    ↓
Start Session → WorkoutSession
    ↓
Complete Workout → Save to Firestore
    ↓
Dashboard Updates (Weekly Stats)
    ↓
View Progress → Profile & AI Coach
```

---

## 🐛 Common Issues & Solutions

| Issue | Cause | Solution |
|-------|-------|----------|
| Firebase not initializing | Missing .env.local | Create with required keys |
| Auth state stuck loading | Listener not set up | Check AuthContext useEffect |
| UI not updating | Real-time listener inactive | Verify Firestore security rules |
| AI Coach no response | API endpoint down | Falls back to local heuristic |
| Workout not saving | Network issue | Check Firestore rules & connectivity |
| Timer not advancing | Browser mute | Unmute for voice OR disable voice |

---

## 🚀 Deployment Checklist

- [ ] All Firebase keys in .env.local
- [ ] Environment vars not in git
- [ ] App builds successfully (`npm run build`)
- [ ] No console errors in dev tools
- [ ] Authentication works (email + Google)
- [ ] Workouts display correctly
- [ ] Timer functionality works
- [ ] Real-time updates work
- [ ] AI Coach responds
- [ ] Mobile responsive
- [ ] Accessibility tested

---

## 📚 Key Technologies Overview

| Tech | Purpose | Version |
|------|---------|---------|
| React | UI Framework | 18.3.1 |
| TypeScript | Type Safety | 5.5.3 |
| Vite | Build Tool | 5.4.1 |
| Firebase | Auth & Database | 12.0.0 |
| Tailwind CSS | Styling | 3.4.11 |
| React Router | Navigation | 6.26.2 |
| Recharts | Charts/Graphs | 2.12.7 |
| Sonner | Notifications | 1.5.0 |
| Lucide React | Icons | 0.462.0 |

---

## 💡 Best Practices Implemented

✅ **Component Organization**
- Functional components with hooks
- Custom hooks for reusability
- Proper prop typing

✅ **State Management**
- Context API for global state
- Local state for component-specific data
- Real-time listeners for data sync

✅ **Performance**
- Code splitting with routes
- Lazy loading of modals/images
- Efficient re-renders

✅ **Security**
- Environment variables for secrets
- Firestore security rules
- User-scoped data access

✅ **Accessibility**
- Semantic HTML
- ARIA labels
- Keyboard navigation
- Color contrast

✅ **Code Quality**
- TypeScript throughout
- ESLint configuration
- Consistent naming conventions
- Clear documentation

---

## 🎓 Learning Path

**New to the codebase? Follow this path:**

1. **Start with**: `src/App.tsx` (main structure)
2. **Then read**: `src/contexts/AuthContext.tsx` (state management)
3. **Check out**: `src/lib/workouts.ts` (workout logic)
4. **Explore**: `src/pages/Dashboard.tsx` (main page)
5. **Deep dive**: `src/components/AICoach.tsx` (advanced features)
6. **Reference**: `src/lib/ai.ts` (AI logic)

---

## 🔗 Important Files by Feature

| Feature | Key Files |
|---------|-----------|
| **Auth** | AuthContext.tsx, components/Auth.tsx, lib/firebase.ts |
| **Workouts** | lib/workouts.ts, pages/DailyWorkout.tsx, pages/WorkoutSession.tsx |
| **AI Coach** | components/AICoach.tsx, lib/ai.ts |
| **Profile** | pages/Profile.tsx, pages/Setup.tsx, contexts/AuthContext.tsx |
| **UI/Design** | src/index.css, tailwind.config.ts, components/ui/* |
| **Routing** | App.tsx, vite.config.ts |

---

## 📞 Support & Debugging

### Enable Debug Logs:
```typescript
// In your component
console.log('Debug:', userProfile);
```

### Check Firebase:
```typescript
// Firebase console
import { auth, db } from '@/lib/firebase';
console.log(auth.currentUser);
```

### Browser DevTools:
- Check Network tab for API calls
- Check Console for errors
- Check Storage for Firebase keys
- Use React DevTools for component state

---

## 🎉 Success Metrics

Your FitLine-Gym implementation is working if:

✅ Users can sign up with email or Google  
✅ Profile auto-completes on dashboard  
✅ Workout selection shows 4 types  
✅ Timer starts and counts down  
✅ Voice announces exercises  
✅ Completion saves to Firestore  
✅ Dashboard shows weekly progress  
✅ AI Coach responds to queries  
✅ Profile metrics calculate correctly  
✅ Everything looks great on mobile  

---

## 🏁 Summary

**FitLine-Gym** is a feature-rich, production-ready fitness application demonstrating:
- Modern React patterns
- Real-time data sync
- Smart personalization
- Professional UI/UX
- Scalable architecture

Perfect for beginners learning full-stack development, or advanced developers showcasing their skills.

---

**Happy Coding! 💪**

For detailed information, see:
- `PROJECT_DOCUMENTATION.md` - Complete feature list
- `ARCHITECTURE.md` - System design & diagrams
- `FEATURES_AND_IMPLEMENTATION.md` - Detailed feature breakdown
