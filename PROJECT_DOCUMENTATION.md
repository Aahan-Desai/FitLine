# FitLine-Gym: Complete Project Documentation

## 📋 Overview

**FitLine-Gym** is a comprehensive fitness application built with modern web technologies. It's a platform designed to provide personalized workout routines, AI coaching, progress tracking, and fitness goals management. The application uses Vite, React, TypeScript, Firebase, and Tailwind CSS to deliver a responsive and engaging user experience.

**Tagline**: "No time? No problem. Personalized micro-workouts for busy lifestyles."

---

## 🏗️ Project Architecture

### Technology Stack

#### Frontend
- **Framework**: React 18.3.1 with TypeScript 5.5.3
- **Build Tool**: Vite 5.4.1
- **Styling**: Tailwind CSS 3.4.11
- **UI Library**: shadcn-ui (comprehensive component library based on Radix UI)
- **Routing**: React Router v6.26.2
- **State Management**: React Context API with AuthContext
- **Data Fetching**: TanStack React Query v5.56.2
- **Form Handling**: React Hook Form 7.53.0 with Zod validation
- **Charts & Visualization**: Recharts 2.12.7

#### Backend & Services
- **Authentication**: Firebase Authentication (Email/Password & Google OAuth)
- **Database**: Firebase Firestore (Real-time NoSQL database)
- **Hosting**: Lovable platform (with Vite deployment)
- **AI/LLM**: Optional external AI endpoint (defaults to local heuristic fallback)

#### Additional Libraries
- **Animations**: Tailwind CSS Animate, custom CSS animations
- **UI Feedback**: Sonner (toast notifications), React Hot Toast
- **Icons**: Lucide React (comprehensive icon set)
- **Utilities**: Clsx, Tailwind Merge, Date-fns
- **Carousel**: Embla Carousel React
- **Sheet UI**: Vaul
- **Keyboard Navigation**: cmdk

---

## 📁 Project Structure

```
FitLine-main/
├── public/                           # Static assets
│   └── robots.txt
├── src/
│   ├── assets/                       # Images and media
│   │   └── hero-fitness.jpg
│   ├── components/                   # Reusable React components
│   │   ├── AICoach.tsx              # AI fitness coach chatbot interface
│   │   ├── AnimatedExercise.tsx     # Exercise animations display
│   │   ├── Auth.tsx                 # Authentication UI (Login/Signup)
│   │   ├── ExerciseVideoModal.tsx   # Video player for exercise guides
│   │   └── ui/                       # shadcn-ui component library
│   │       ├── accordion.tsx
│   │       ├── alert-dialog.tsx
│   │       ├── button.tsx
│   │       ├── card.tsx
│   │       ├── dialog.tsx
│   │       ├── input.tsx
│   │       ├── select.tsx
│   │       ├── tabs.tsx
│   │       ├── toaster.tsx
│   │       ├── sonner.tsx
│   │       └── [30+ other UI components]
│   ├── contexts/
│   │   └── AuthContext.tsx          # Global authentication state management
│   ├── hooks/
│   │   ├── use-mobile.tsx           # Mobile detection hook
│   │   └── use-toast.ts             # Toast notification hook
│   ├── lib/
│   │   ├── ai.ts                    # AI coach logic and prompt engineering
│   │   ├── firebase.ts              # Firebase initialization and config
│   │   ├── workouts.ts              # Workout database and personalization
│   │   └── utils.ts                 # Utility functions (cn for class merging)
│   ├── pages/
│   │   ├── Landing.tsx              # Public landing/home page
│   │   ├── Auth.tsx                 # Authentication page (Login/Signup)
│   │   ├── Dashboard.tsx            # Main dashboard with daily recommendations
│   │   ├── DailyWorkout.tsx         # Daily workout selection and overview
│   │   ├── WorkoutSession.tsx       # Active workout timer and progression
│   │   ├── Profile.tsx              # User profile, BMI, calorie calculator
│   │   ├── Setup.tsx                # Multi-step profile setup wizard
│   │   └── NotFound.tsx             # 404 error page
│   ├── App.tsx                       # Main app component with routing
│   ├── main.tsx                      # React entry point
│   ├── index.css                     # Global styles and design system
│   ├── App.css                       # App-specific styles
│   ├── vite-env.d.ts                # Vite environment types
├── index.html                        # HTML template
├── package.json                      # Dependencies and scripts
├── tailwind.config.ts               # Tailwind CSS configuration
├── tsconfig.json                    # TypeScript configuration
├── tsconfig.app.json               # App TypeScript config
├── tsconfig.node.json              # Node TypeScript config
├── vite.config.ts                  # Vite configuration
├── eslint.config.js                # ESLint configuration
├── postcss.config.js               # PostCSS configuration
├── components.json                 # shadcn-ui configuration
└── README.md                       # Project documentation
```

---

## 🎯 Core Features & Functionality

### 1. **Authentication System** (`src/contexts/AuthContext.tsx`)

**Purpose**: Manages user authentication state globally across the app.

**Features**:
- Email/password authentication via Firebase
- Google OAuth integration
- Real-time profile data synchronization using Firestore listeners
- User profile caching with automatic updates
- Logout functionality

**Key Interfaces**:
```typescript
interface UserProfile {
  uid: string;
  email: string | null;
  displayName: string | null;
  gender?: "male" | "female" | "other";
  age?: number;
  height?: number;
  weight?: number;
  fitnessGoal?: "weight_loss" | "muscle_gain" | "endurance" | "general";
  experienceLevel?: "beginner" | "intermediate" | "advanced";
  preferredWorkoutTime?: "morning" | "afternoon" | "evening";
  intensityLevel?: "quickie" | "classic" | "power" | "beast";
  createdAt: Date;
}
```

**Data Flow**:
1. Firebase Authentication detects login/logout
2. Firestore listener subscribes to user document
3. Profile data syncs in real-time
4. All pages access via `useAuth()` hook

---

### 2. **Workout Database & Personalization** (`src/lib/workouts.ts`)

**Purpose**: Comprehensive workout library with personalization logic.

**Workout Types**:
1. **Quickie** (5-10 min) - High intensity, quick sessions
   - Morning Energizer
   - Power Burst

2. **Classic** (20-30 min) - Balanced full-body workouts
   - Full Body Power
   - Strength Foundation

3. **Power** (45-60 min) - Advanced comprehensive sessions
   - Athletic Performance
   - Hypertrophy Focus

4. **Beast** (90-120+ min) - Maximum intensity challenge
   - Ultimate Challenge

**Each Workout Includes**:
```typescript
interface Workout {
  id: string;
  name: string;
  description: string;
  duration: string;
  difficulty: 1-5; // 1=Beginner, 5=Expert
  category: "cardio" | "strength" | "flexibility" | "hiit" | "mixed";
  exercises: Exercise[];
  targetMuscles: string[];
  equipment: string[];
  caloriesBurn: string;
  suitableFor: ("beginner" | "intermediate" | "advanced")[];
  goals: ("weight_loss" | "muscle_gain" | "endurance" | "general")[];
  guideLink: string;
  blogTitle: string;
}

interface Exercise {
  name: string;
  sets: number;
  reps: string;
  duration?: string;
  rest?: string;
  notes?: string;
  videoUrl?: string;
  videoThumbnail?: string;
}
```

**Personalization Logic**:
- Filters workouts by user experience level
- Matches user fitness goals
- Rotates workouts by day of week
- Recommends intensity based on experience

**Key Functions**:
- `getPersonalizedWorkout()` - Returns filtered workout for user
- `getRecommendedWorkoutType()` - Suggests intensity level
- `getDifficultyColor()` - Visual indicator for difficulty
- `getDifficultyLabel()` - Human-readable difficulty

---

### 3. **AI Fitness Coach** (`src/lib/ai.ts` & `src/components/AICoach.tsx`)

**Purpose**: Intelligent chatbot providing personalized fitness guidance.

**Architecture**:
- **System Prompt-based Design**: Constrains AI to fitness-only responses
- **Profile Context Injection**: Uses user data for personalized advice
- **Progress Context Tracking**: Leverages workout history
- **Hybrid Approach**: Upstream API fallback + local heuristic

**Key Features**:
1. **Profile-Aware Responses**: Tailors advice to user's age, gender, goal, experience
2. **Progress Analytics**: Analyzes workout completion, weekly targets, weight changes
3. **Smart Intent Detection**:
   - Last workout queries
   - Next workout suggestions
   - Progress tracking
   - Beginner workout plans
   - Injury/safety concerns
   - Muscle gain/fat loss guidance
4. **Conversation History**: Maintains chat context for coherent responses

**Safety Mechanisms**:
- Enforces data-only constraint (no random advice)
- Recommends professional consultation for medical issues
- Provides motivational tips based on actual progress

**UI Component** (`AICoach.tsx`):
- Floating chat bubble (toggleable)
- Real-time message display
- Smooth animations and transitions
- Auto-fetches progress data from Firestore

---

### 4. **Authentication Pages**

#### **Landing Page** (`src/pages/Landing.tsx`)
**Purpose**: Public marketing/onboarding page

**Sections**:
1. **Hero Section**
   - Bold headline: "No Excuses. Just Results."
   - Subheading: "No time? No problem. Daily fitness in minutes with your AI Coach."
   - CTA buttons (Sign In / Join Free)

2. **Features Section** (4 feature cards)
   - Quick Workouts (5-60 min)
   - AI Coach (24/7 guidance)
   - Daily Goals (consistency tracking)
   - Track Progress (visual insights)

3. **Testimonials Section**
   - Success stories from community
   - User avatars and quotes

4. **CTA Band**
   - Final call-to-action

5. **Footer**
   - Copyright and branding

**Design Elements**:
- Fixed header with navigation
- Hero image background with gradient overlay
- Responsive grid layouts
- Glassmorphism effects (backdrop blur)
- Custom animations (floating icons, scrolling)

---

#### **Auth Page** (`src/components/Auth.tsx`)
**Purpose**: Unified login/signup interface

**Features**:
- Tabbed interface (Login / Sign Up)
- Email/password authentication
- Google OAuth integration
- Password visibility toggle
- Error handling and display
- Loading states
- Email validation
- Automatic navigation to dashboard post-auth

**Firestore Integration**:
- Creates user document on signup
- Stores: uid, email, displayName, createdAt

---

### 5. **Dashboard** (`src/pages/Dashboard.tsx`)

**Purpose**: Main hub for user's fitness journey

**Components**:
1. **Profile Completion Dialog**
   - Prompts missing fitness goal/experience level
   - Saves to Firestore, triggers refresh

2. **Daily Quote Section**
   - Motivational fitness quotes

3. **Weekly Progress Card**
   - Shows completions vs. 7-day goal
   - Visual progress bar
   - Counts stored completions in Firestore

4. **Today's Workout Card**
   - Personalized recommendation based on day/profile
   - Quick-start button to WorkoutSession
   - Displays difficulty, duration, target muscles

5. **Quick Links**
   - Daily Workout (select from all types)
   - Profile settings
   - Logout button

**Firestore Queries**:
- Fetches weekly completions (Monday-Sunday)
- Loads last completed workout
- Counts total workout sessions

---

### 6. **Daily Workout Selection** (`src/pages/DailyWorkout.tsx`)

**Purpose**: Browse and select workouts before starting session

**Features**:
1. **Workout Type Tabs**
   - Switch between Quickie, Classic, Power, Beast

2. **Workout Details**
   - Name, description, duration
   - Difficulty badge with color coding
   - Target muscles list
   - Equipment requirements
   - Estimated calorie burn
   - Category icon

3. **Exercise List**
   - Each exercise with:
     - Sets and reps/duration
     - Rest periods
     - Video thumbnail (clickable)
     - Watch guide button

4. **Action Buttons**
   - Start Workout (navigates to WorkoutSession)
   - Mark as Completed (saves to Firestore)
   - Watch Exercise Videos

5. **Exercise Video Modal**
   - Embedded YouTube player
   - Playback controls
   - Exercise animations

---

### 7. **Workout Session** (`src/pages/WorkoutSession.tsx`)

**Purpose**: Active workout timer with guidance and tracking

**Complex Features**:
1. **Exercise Progression**
   - Multi-set tracking
   - Current exercise index
   - Set counter
   - Visual progress bar (workout %)

2. **Timer System**
   - Work phase timer (exercise duration)
   - Rest phase timer (between sets)
   - Automatic advancement
   - Manual skip/previous controls
   - Color-coded timer (green → orange → red)

3. **Audio/Voice Guidance**
   - Text-to-speech countdowns
   - Exercise name announcements
   - Voice toggle switch
   - Prevents overlapping audio

4. **Manual Mode**
   - Toggle timer automation
   - Manual exercise progression
   - Flexible pacing

5. **Exercise Display**
   - Current exercise name
   - Sets/reps information
   - Estimated duration
   - Animated exercise visualization
   - Video guide button

6. **Completion Tracking**
   - Tracks completed exercises
   - Overall workout progress
   - Duration tracking (start time)
   - Prevents double-counting

7. **UI Elements**
   - Pause/Play controls
   - Skip/Previous navigation
   - Restart current exercise
   - Finish workout button
   - Voice status indicator
   - Time countdown display

**Technical Highlights**:
- Uses `useRef` to prevent race conditions
- Debounces advancement (prevents rapid clicking)
- Handles timer parsing from various formats (seconds, minutes, reps)
- Responsive animation states

---

### 8. **Setup Wizard** (`src/pages/Setup.tsx`)

**Purpose**: Multi-step profile completion on first login

**5-Step Process**:
1. **Personal Info**
   - Name, age, gender

2. **Physical Metrics**
   - Height (cm), weight (kg)

3. **Fitness Profile**
   - Primary goal (dropdown)
   - Experience level (dropdown)

4. **Preferences**
   - Workout time preference
   - Intensity level preference

5. **Summary**
   - Review entered data
   - Confirm and save

**Features**:
- Progress bar (visual step indicator)
- Next/Previous navigation
- Input validation
- Loading state during save
- Auto-redirect to dashboard on completion
- Firestore integration (setDoc with merge)
- Profile refresh trigger

---

### 9. **Profile Page** (`src/pages/Profile.tsx`)

**Purpose**: Comprehensive profile management and fitness metrics

**Sections**:
1. **Profile Editor**
   - Editable fields: name, age, gender, height, weight, goals, experience
   - Save changes to Firestore
   - Loading indicator during save
   - Toast notifications (success/error)

2. **BMI Calculator**
   - Real-time calculation
   - Category badge (Underweight/Normal/Overweight/Obese)
   - Color-coded status
   - Formula: weight(kg) / height(m)²

3. **Calorie Calculator**
   - Harris-Benedict BMR calculation
   - Gender-based formulas
   - Activity factor (1.55 for moderate)
   - Three recommendations:
     - Maintenance calories
     - Deficit (for weight loss) = maintenance - 500
     - Surplus (for muscle gain) = maintenance + 300

4. **Tabs Interface**
   - Profile tab
   - Metrics tab
   - Preferences tab

5. **Data Persistence**
   - Two-way sync with Firestore
   - Real-time profile listener
   - Timestamp tracking (updatedAt)

---

### 10. **Design System** (`src/index.css`)

**Color Scheme**:
- **Primary**: Orange (#FF7A00) - Energetic, motivational
- **Background**: Light grey (#FAF9F7) - Clean, readable
- **Foreground**: Dark grey (#161616) - High contrast

**CSS Variables**:
```css
--primary: 24 100% 50%; /* Orange */
--gradient-primary: Linear gradient (orange)
--gradient-hero: Hero background (orange to darker orange)
--gradient-card: Card backgrounds (subtle white gradient)
--shadow-hero: Large, glowing shadow
--shadow-card: Subtle card shadow
```

**Typography**:
- **Heading Font**: Bebas Neue (bold, uppercase)
- **Body Font**: Montserrat (clean, readable)

**Custom Utilities**:
- `animate-float`: Floating icon animation (4s loop)
- `marquee`: Horizontal scrolling animation
- `transition-smooth`: 0.3s cubic-bezier easing
- `gradient-primary`, `shadow-hero`, etc.

**Responsive Design**:
- Mobile-first approach
- Tailwind breakpoints (sm, md, lg, xl, 2xl)
- Flexible grid layouts
- Hidden elements on mobile (responsive nav)

---

## 🔄 Data Flow & State Management

### Authentication Flow
```
User -> Landing/Auth Page
  ↓
Firebase Auth (email or Google)
  ↓
Firestore User Document Created
  ↓
AuthContext updates (real-time listener)
  ↓
Dashboard (protected route)
  ↓
Setup or Dashboard (based on completion)
```

### Workout Data Flow
```
WorkoutDatabase (workouts.ts)
  ↓
User Profile (goals, experience)
  ↓
Personalization (filter + rotate)
  ↓
Daily Workout Page (display options)
  ↓
Workout Session (timer & progression)
  ↓
Firestore Completion Document
  ↓
Dashboard Stats (weekly count)
```

### AI Coach Data Flow
```
User Input (message)
  ↓
Fetch Progress Data (Firestore)
  ↓
Build Context (profile + progress)
  ↓
Call askAICoach()
  ↓
Try External API (if VITE_AI_ENDPOINT set)
  ↓
Fallback Local Heuristic
  ↓
Display Response
```

---

## 📦 Dependencies Breakdown

### Core Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| react | 18.3.1 | UI framework |
| typescript | 5.5.3 | Type safety |
| vite | 5.4.1 | Build tool |
| react-router-dom | 6.26.2 | Routing |
| firebase | 12.0.0 | Auth + Database |
| tailwindcss | 3.4.11 | Styling |
| @hookform/resolvers | 3.9.0 | Form validation |
| @tanstack/react-query | 5.56.2 | Data fetching |
| recharts | 2.12.7 | Charts/graphs |
| lucide-react | 0.462.0 | Icons |
| sonner | 1.5.0 | Toast notifications |
| zod | 3.23.8 | Schema validation |
| date-fns | 3.6.0 | Date utilities |

### shadcn-ui Components (30+)
All radix-ui based, fully customizable components for UI building.

---

## 🔐 Security & Environment Variables

**Required Environment Variables** (`.env.local`):
```
VITE_FIREBASE_API_KEY=
VITE_FIREBASE_AUTH_DOMAIN=
VITE_FIREBASE_PROJECT_ID=
VITE_FIREBASE_STORAGE_BUCKET=
VITE_FIREBASE_MESSAGING_SENDER_ID=
VITE_FIREBASE_APP_ID=
VITE_FIREBASE_MEASUREMENT_ID=
VITE_AI_ENDPOINT= (optional)
```

**Security Practices**:
- Environment variables loaded via `import.meta.env`
- Firebase configuration validated on startup
- Protected routes using AuthContext
- Firestore security rules enforced server-side
- User documents scoped by UID

---

## 🚀 Deployment & Build

### Build Process
```bash
npm run build
# Outputs to dist/
```

### Development Server
```bash
npm run dev
# Runs on localhost:8080
```

### Linting
```bash
npm run lint
# ESLint configuration with React hooks
```

### Deployment Platforms
- **Lovable**: Primary platform with auto-deployment from git
- **Vite Preview**: Local preview of production build
- **Custom Deploy**: Any static hosting (Vercel, Netlify, etc.)

---

## 📊 Database Schema (Firestore)

### Collection: `users/{uid}`
```typescript
{
  uid: string;
  email: string;
  displayName: string;
  age: number;
  gender: "male" | "female" | "other";
  height: number; // cm
  weight: number; // kg
  fitnessGoal: "weight_loss" | "muscle_gain" | "endurance" | "general";
  experienceLevel: "beginner" | "intermediate" | "advanced";
  preferredWorkoutTime: "morning" | "afternoon" | "evening";
  intensityLevel: "quickie" | "classic" | "power" | "beast";
  createdAt: Timestamp;
  updatedAt: Timestamp;
}
```

### Subcollection: `users/{uid}/completions/{dateKey}`
```typescript
{
  date: Timestamp;
  dateKey: string; // YYYY-MM-DD format
  workoutType: "quickie" | "classic" | "power" | "beast";
  workoutId: string;
  duration: string;
  calories: string;
  completed: boolean;
}
```

---

## 🎨 UI/UX Highlights

### Visual Design
- **Bold Typography**: Bebas Neue for headings, uppercase
- **Motivational Colors**: Orange primary with gradients
- **Smooth Animations**: Transitions, floats, bounces
- **Glassmorphism**: Backdrop blur effects on cards

### Accessibility
- Semantic HTML
- ARIA labels on buttons
- Keyboard navigation support
- High contrast ratios
- Focus states on interactive elements

### Responsiveness
- Mobile-first design
- Breakpoints for tablet/desktop
- Flexible grid layouts
- Touch-friendly buttons (48px minimum)

---

## 🔗 Routing Map

```
/ (Landing)
  ├─ /auth (Auth - Public Route)
  ├─ /setup (Setup - Protected)
  ├─ /dashboard (Dashboard - Protected)
  ├─ /daily-workout (Daily Workout - Protected)
  ├─ /workout-session (Workout Session - Protected)
  ├─ /profile (Profile - Protected)
  └─ /404 (Not Found)
```

---

## 🎯 Key Algorithms & Logic

### 1. Workout Personalization
```typescript
function getPersonalizedWorkout(profile, type, dayOfWeek) {
  let workouts = workoutTypes[type].workouts;
  
  // Filter by experience level
  workouts = workouts.filter(w => w.suitableFor.includes(profile.experienceLevel));
  
  // Filter by goal
  workouts = workouts.filter(w => w.goals.includes(profile.fitnessGoal));
  
  // Rotate by day (ensures variety)
  const index = dayOfWeek % workouts.length;
  return workouts[index];
}
```

### 2. Timer Parsing
```typescript
function parseSeconds(text) {
  // Matches: "30 seconds", "1 minute", "10 reps", etc.
  // Returns normalized seconds for timer
}
```

### 3. BMI Calculation
```typescript
function calculateBMI(height, weight) {
  return weight / (height / 100) ** 2;
}
```

### 4. BMR (Basal Metabolic Rate)
```typescript
// Harris-Benedict Formula
if (gender === "male") {
  bmr = 88.362 + (13.397 * weight) + (4.799 * height) - (5.677 * age);
} else {
  bmr = 447.593 + (9.247 * weight) + (3.098 * height) - (4.330 * age);
}
totalCalories = bmr * 1.55; // Activity factor
```

---

## 🧪 Testing & Quality

### ESLint Configuration
- React plugin with hooks rules
- React Refresh plugin
- Global variables awareness
- JavaScript config format

### Type Safety
- Strict TypeScript interfaces
- Optional chaining (?.)
- Null checks
- Discriminated unions for workout types

---

## 🔮 Future Enhancement Possibilities

1. **Social Features**
   - User profiles and following
   - Leaderboards
   - Community challenges

2. **Advanced Analytics**
   - Detailed progress charts
   - Strength progressions
   - Body composition tracking

3. **Integrations**
   - Wearable device sync (Apple Watch, Fitbit)
   - Calendar integration
   - Calendar sync for workout schedules

4. **Advanced AI**
   - Workout plan generation
   - Form correction via video analysis
   - Personalized nutrition planning

5. **Monetization**
   - Premium workout library
   - Premium AI coaching
   - Custom meal plans
   - Personal trainer booking

6. **Offline Mode**
   - Service Worker caching
   - Offline workout access
   - Sync on reconnect

---

## 📝 Summary

**FitLine-Gym** is a full-featured fitness application combining:

✅ **Modern Tech Stack**: React + TypeScript + Vite + Tailwind
✅ **Comprehensive Workouts**: 9 pre-designed routines across 4 intensity levels
✅ **AI Coach**: Context-aware fitness guidance
✅ **Progress Tracking**: Weekly goals, completion stats, metrics
✅ **User Personalization**: Profile-based recommendations
✅ **Real-time Updates**: Firebase Firestore listeners
✅ **Responsive Design**: Mobile-optimized interface
✅ **Authentication**: Email + Google OAuth
✅ **Interactive Timers**: With voice guidance
✅ **Fitness Calculations**: BMI, calorie needs, macros

The project demonstrates professional full-stack development practices with clean architecture, type safety, and user-centric design.

---

**Project Status**: Fully functional with room for enhancement in social features, advanced analytics, and integrations.
