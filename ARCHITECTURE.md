# FitLine-Gym: Architecture & Component Diagram

## System Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                         USER BROWSER                                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │                    REACT APPLICATION                           │ │
│  │  ┌──────────────────────────────────────────────────────────┐  │ │
│  │  │ App.tsx (Main Router)                                    │  │ │
│  │  │ - Protected Routes (ProtectedRoute Component)            │  │ │
│  │  │ - Public Routes (PublicRoute Component)                  │  │ │
│  │  │ - QueryClientProvider (TanStack Query)                   │  │ │
│  │  │ - AuthProvider (Global Context)                          │  │ │
│  │  └──────────────────────────────────────────────────────────┘  │ │
│  │                                                                  │ │
│  │  ┌──────────────────────────────────────────────────────────┐  │ │
│  │  │           Pages (Route Components)                       │  │ │
│  │  ├──────────────────────────────────────────────────────────┤  │ │
│  │  │ • Landing (Public - Marketing)                           │  │ │
│  │  │ • Auth (Public - Login/Signup)                           │  │ │
│  │  │ • Setup (Protected - Profile Wizard)                     │  │ │
│  │  │ • Dashboard (Protected - Hub)                            │  │ │
│  │  │ • DailyWorkout (Protected - Selection)                   │  │ │
│  │  │ • WorkoutSession (Protected - Timer & Guidance)          │  │ │
│  │  │ • Profile (Protected - Settings & Metrics)               │  │ │
│  │  └──────────────────────────────────────────────────────────┘  │ │
│  │                                                                  │ │
│  │  ┌──────────────────────────────────────────────────────────┐  │ │
│  │  │        Components & Features                             │  │ │
│  │  ├──────────────────────────────────────────────────────────┤  │ │
│  │  │ • AICoach (Floating Chat Interface)                      │  │ │
│  │  │ • AnimatedExercise (Visual Exercise Animations)          │  │ │
│  │  │ • ExerciseVideoModal (Video Player)                      │  │ │
│  │  │ • UI Library (30+ shadcn-ui Components)                  │  │ │
│  │  └──────────────────────────────────────────────────────────┘  │ │
│  │                                                                  │ │
│  │  ┌──────────────────────────────────────────────────────────┐  │ │
│  │  │        State Management & Hooks                          │  │ │
│  │  ├──────────────────────────────────────────────────────────┤  │ │
│  │  │ AuthContext                                              │  │ │
│  │  │  ├─ user (Firebase Auth User)                            │  │ │
│  │  │  ├─ userProfile (Firestore Profile)                      │  │ │
│  │  │  ├─ loading (Initial load state)                         │  │ │
│  │  │  └─ logout(), refreshProfile()                           │  │ │
│  │  │                                                          │  │ │
│  │  │ Hooks                                                    │  │ │
│  │  │  ├─ useAuth() - Access global auth state               │  │ │
│  │  │  ├─ useToast() - Display notifications                  │  │ │
│  │  │  └─ use-mobile - Responsive detection                   │  │ │
│  │  └──────────────────────────────────────────────────────────┘  │ │
│  │                                                                  │ │
│  │  ┌──────────────────────────────────────────────────────────┐  │ │
│  │  │        Styling & Design System                           │  │ │
│  │  ├──────────────────────────────────────────────────────────┤  │ │
│  │  │ Tailwind CSS + Custom CSS                                │  │ │
│  │  │  ├─ Color Variables (Primary Orange)                     │  │ │
│  │  │  ├─ Gradient Definitions                                 │  │ │
│  │  │  ├─ Shadow & Animation Utilities                         │  │ │
│  │  │  └─ Typography (Bebas Neue + Montserrat)                │  │ │
│  │  └──────────────────────────────────────────────────────────┘  │ │
│  └──────────────────────────────────────────────────────────────┘ │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
                                  │
                ┌─────────────────┼─────────────────┐
                │                 │                 │
                ▼                 ▼                 ▼
        ┌──────────────┐   ┌─────────────┐   ┌──────────────┐
        │   Firebase   │   │  External   │   │   Device    │
        │   Services   │   │   AI API    │   │    APIs      │
        │              │   │  (Optional) │   │              │
        └──────────────┘   └─────────────┘   └──────────────┘
                │
        ┌───────┴─────────┐
        │                 │
        ▼                 ▼
    ┌─────────┐      ┌──────────────┐
    │  Auth   │      │  Firestore   │
    │  Module │      │  Database    │
    └─────────┘      └──────────────┘
        │                     │
        │              ┌──────┴──────┬─────────┐
        │              │             │         │
        │              ▼             ▼         ▼
        │         ┌────────────┐ ┌──────────┐ ┌────────────┐
        │         │  users/    │ │ completions/  │ │ progress/  │
        │         │   {uid}    │ │ {uid}/date│ │  {uid}/... │
        │         │            │ │              │ │            │
        │         │ - Profile  │ │ - Workout  │ │ - Stats    │
        │         │ - Settings │ │ - Duration │ │ - Analytics│
        │         └────────────┘ └──────────┘ └────────────┘
        │
    ┌──────────────────────┐
    │ Firebase Auth        │
    │ (Email, Google, etc) │
    └──────────────────────┘
```

---

## Data Flow Diagrams

### 1. User Authentication Flow

```
Landing Page
    │
    └─ Click "Join Free" or "Sign In"
         │
         ▼
    Auth.tsx
    ├─ Email/Password Input
    └─ Google OAuth Button
         │
         ├─ Firebase Auth
         │  ├─ createUserWithEmailAndPassword()
         │  └─ signInWithPopup(GoogleAuthProvider)
         │
         ├─ Success ──────────────────────┐
         │                                 │
         │  Create Firestore User Doc     │
         │  ├─ uid                        │
         │  ├─ email                      │
         │  ├─ displayName                │
         │  └─ createdAt                  │
         │                                 │
         └────────────────────────────────┤
                                          │
                                          ▼
                    AuthContext Updates (Real-time Listener)
                    │
                    ├─ setUser (Firebase User)
                    ├─ setUserProfile (Firestore Data)
                    └─ setLoading(false)
                    │
                    ▼
            Navigate to /dashboard
            │
            ├─ If profile incomplete ──→ Show "Complete Profile" Dialog
            │                              │
            │                              └─ Save to Firestore
            │
            └─ If profile complete ──→ Display Dashboard
```

### 2. Workout Selection & Session Flow

```
Dashboard
    │
    └─ Click "Today's Workout" or "Daily Workout"
         │
         ▼
    DailyWorkout.tsx
    ├─ Load getPersonalizedWorkout()
    │  │
    │  ├─ Filter by: Experience Level
    │  ├─ Filter by: Fitness Goal
    │  └─ Rotate by: Day of Week
    │
    ├─ Display Workout Type Tabs
    │  ├─ Quickie (5-10 min)
    │  ├─ Classic (20-30 min)
    │  ├─ Power (45-60 min)
    │  └─ Beast (90+ min)
    │
    ├─ Show Exercise List
    │  ├─ Exercise Name
    │  ├─ Sets & Reps
    │  ├─ Video Thumbnail
    │  └─ Watch Guide Button
    │
    └─ Click "Start Workout" / "Mark Complete"
         │
         ├─ Mark Complete → Save to Firestore
         │  └─ users/{uid}/completions/{dateKey}
         │
         └─ Start Workout → Navigate to WorkoutSession
              │
              ▼
         WorkoutSession.tsx
         ├─ Initialize Timer (parseSeconds)
         ├─ Set Workout Start Time
         └─ Voice Guidance (optional)
         │
         ├─ Exercise Loop:
         │  ├─ Display Current Exercise
         │  ├─ Start Timer (work phase)
         │  ├─ Play Voice Announcement
         │  ├─ Auto-advance or Manual Skip
         │  ├─ Rest Phase Timer
         │  └─ Next Exercise
         │
         ├─ Track Completed Exercises
         ├─ Update Progress Bar
         │
         └─ Finish Workout
            └─ Save Completion to Firestore
```

### 3. AI Coach Data & Response Flow

```
User Opens Chat (AICoach.tsx)
    │
    └─ Send Message
         │
         ├─ Fetch Progress Data (Async)
         │  │
         │  ├─ Firestore Query: Last Completed Workout
         │  │  └─ Query: ordered by date, limit 1
         │  │
         │  ├─ Firestore Query: Weekly Completions
         │  │  ├─ Get Monday of Current Week
         │  │  └─ Count completions in range
         │  │
         │  ├─ Calculate Next Suggestion
         │  │  └─ getRecommendedWorkoutType() + getPersonalizedWorkout()
         │  │
         │  └─ Build ProgressContext
         │     ├─ lastCompleted
         │     ├─ nextSuggestion
         │     ├─ weeklyCompletions
         │     ├─ totalCompletions
         │     └─ weightProgress (if available)
         │
         ├─ Build Message Payload
         │  │
         │  ├─ System Prompt (AI Coach Role)
         │  ├─ Profile Context (User Data)
         │  ├─ Progress Context (Workout History)
         │  ├─ Chat History (Previous Messages)
         │  └─ Current User Input
         │
         ├─ Call askAICoach()
         │  │
         │  ├─ If VITE_AI_ENDPOINT set:
         │  │  │
         │  │  └─ POST to External AI Service
         │  │     │
         │  │     └─ Expected Response: { reply: string }
         │  │
         │  └─ Fallback to Local Heuristic
         │     │
         │     ├─ Pattern Match User Input
         │     │  ├─ Last workout query
         │     │  ├─ Next workout query
         │     │  ├─ Progress query
         │     │  ├─ Beginner advice
         │     │  └─ Goal-specific guidance
         │     │
         │     ├─ Look up Progress Data
         │     └─ Format Response with Context
         │
         ▼
    Display AI Response in Chat
    └─ Add to Message History
```

### 4. Profile Completion & Personalization

```
New User (No Profile Data)
    │
    ▼
Dashboard.tsx
├─ Check if fitnessGoal or experienceLevel missing
└─ Show "Complete Profile" Dialog
   │
   ├─ Save Quick Basics
   │  ├─ fitnessGoal (dropdown)
   │  ├─ experienceLevel (dropdown)
   │  └─ Update Firestore (doc update)
   │
   └─ Navigate to Setup.tsx (Optional Detailed Setup)
      │
      ├─ Step 1: Personal Info (Name, Age, Gender)
      ├─ Step 2: Physical Metrics (Height, Weight)
      ├─ Step 3: Fitness Goal & Experience
      ├─ Step 4: Preferences (Time, Intensity)
      ├─ Step 5: Review & Confirm
      │
      ├─ Save All Data to Firestore
      │  ├─ setDoc or updateDoc
      │  └─ Set merge: true to preserve existing data
      │
      ├─ Trigger refreshProfile()
      │  └─ AuthContext listener updates userProfile
      │
      └─ Navigate to /dashboard
         │
         ▼
    Personalization Active
    ├─ getRecommendedWorkoutType() uses intensityLevel
    ├─ getPersonalizedWorkout() filters by goal & experience
    └─ Dashboard shows tailored recommendations
```

---

## Component Tree

```
App.tsx (Root)
├─ QueryClientProvider
├─ AuthProvider (AuthContext)
│  └─ useAuth Hook Available
└─ BrowserRouter
   ├─ TooltipProvider
   ├─ Toaster (Toast Notifications)
   ├─ Sonner (Toast Notifications)
   ├─ AICoach (Floating Chat Widget - Rendered Last)
   │  └─ Fetches Progress Data
   │     ├─ askAICoach() Logic
   │     ├─ Profile Context
   │     └─ Chat History
   │
   └─ Routes
      │
      ├─ Route: "/" → Landing
      │  └─ Sections:
      │     ├─ Header (Navigation)
      │     ├─ Hero Section
      │     ├─ Features Section (4 Cards)
      │     ├─ Testimonials Section
      │     ├─ CTA Band
      │     └─ Footer
      │
      ├─ Route: "/auth" → PublicRoute → Auth
      │  └─ Components:
      │     ├─ Card (shadcn)
      │     ├─ Tabs (shadcn)
      │     ├─ Input (shadcn)
      │     ├─ Button (shadcn)
      │     ├─ Alert (shadcn)
      │     └─ Icons (Lucide)
      │
      ├─ Route: "/setup" → ProtectedRoute → Setup
      │  └─ Components:
      │     ├─ Multi-step Form
      │     ├─ Progress Bar
      │     ├─ Select Dropdowns (shadcn)
      │     ├─ Input Fields (shadcn)
      │     └─ Navigation Buttons
      │
      ├─ Route: "/dashboard" → ProtectedRoute → Dashboard
      │  └─ Components:
      │     ├─ Dialog (Complete Profile Prompt)
      │     ├─ Card (Quote Section)
      │     ├─ Card (Weekly Progress)
      │     ├─ Card (Today's Workout)
      │     ├─ Progress Bar (shadcn)
      │     ├─ Badge (Difficulty)
      │     └─ Links/Buttons
      │
      ├─ Route: "/daily-workout" → ProtectedRoute → DailyWorkout
      │  └─ Components:
      │     ├─ Header
      │     ├─ Tabs (Workout Types) (shadcn)
      │     ├─ TabsContent (Workout Details)
      │     │  └─ Cards (shadcn)
      │     │     ├─ Workout Info
      │     │     ├─ Exercise List
      │     │     ├─ AnimatedExercise (Exercise Icon)
      │     │     └─ ExerciseVideoModal
      │     ├─ Badge (Category)
      │     └─ Buttons (Play, Mark Complete)
      │
      ├─ Route: "/workout-session" → ProtectedRoute → WorkoutSession
      │  └─ Components:
      │     ├─ Header
      │     ├─ Timer Display (Large Font)
      │     ├─ Exercise Name & Info
      │     ├─ Progress Bar (Overall & Current)
      │     ├─ AnimatedExercise (Visual)
      │     ├─ Controls
      │     │  ├─ Play/Pause Button
      │     │  ├─ Skip Button
      │     │  ├─ Restart Button
      │     │  └─ Voice Toggle
      │     ├─ Voice Status Indicator
      │     └─ ExerciseVideoModal
      │
      ├─ Route: "/profile" → ProtectedRoute → Profile
      │  └─ Components:
      │     ├─ Header
      │     ├─ Tabs (Profile, Metrics, Preferences) (shadcn)
      │     ├─ Profile Tab
      │     │  ├─ Editable Fields (Input shadcn)
      │     │  ├─ Select Dropdowns (shadcn)
      │     │  └─ Save Button
      │     ├─ Metrics Tab
      │     │  ├─ BMI Calculator
      │     │  │  ├─ Input Fields
      │     │  │  ├─ Calculated Display
      │     │  │  └─ Badge (Status)
      │     │  └─ Calorie Calculator
      │     │     ├─ Input Fields
      │     │     └─ Calculated Recommendations
      │     └─ Preferences Tab
      │
      └─ Route: "*" → NotFound
         └─ 404 Error Page


UI Library (shadcn-ui Components Used Throughout):
├─ button.tsx
├─ card.tsx
├─ input.tsx
├─ select.tsx
├─ dialog.tsx
├─ tabs.tsx
├─ badge.tsx
├─ progress.tsx
├─ alert.tsx
├─ label.tsx
├─ form.tsx
├─ toast/toaster.tsx
├─ sonner.tsx
└─ 20+ more...
```

---

## State Management Diagram

```
AuthContext (Global State)
│
├─ user: Firebase User | null
│  ├─ uid
│  ├─ email
│  ├─ displayName
│  └─ metadata
│
├─ userProfile: UserProfile | null
│  ├─ uid
│  ├─ email
│  ├─ displayName
│  ├─ age
│  ├─ gender
│  ├─ height
│  ├─ weight
│  ├─ fitnessGoal
│  ├─ experienceLevel
│  ├─ preferredWorkoutTime
│  ├─ intensityLevel
│  └─ createdAt
│
├─ loading: boolean (Initial auth check)
│
└─ Functions:
   ├─ logout() → signOut(auth), clear profile
   └─ refreshProfile() → Force Firestore fetch


Local Component States:
│
├─ Dashboard
│  ├─ showCompleteProfile
│  ├─ goal, experience (form state)
│  ├─ weeklyProgress
│  └─ saving
│
├─ DailyWorkout
│  ├─ selectedWorkout (dropdown)
│  ├─ savingCompletion
│  └─ selectedExercise (for video modal)
│
├─ WorkoutSession
│  ├─ exerciseIndex
│  ├─ setIndex
│  ├─ isRunning
│  ├─ secondsLeft
│  ├─ isRestPhase
│  ├─ voiceEnabled
│  ├─ completedExercises (array)
│  ├─ workoutStartTime
│  └─ manualMode
│
├─ AICoach
│  ├─ isOpen (chat visibility)
│  ├─ messages (bubble array)
│  ├─ message (input text)
│  ├─ isThinking
│  └─ progress (user's progress context)
│
└─ Profile
   ├─ isEditing
   ├─ isSaving
   └─ userProfile (form state - mirrors auth)
```

---

## Database Schema Visualization

```
Firestore Database Structure:
│
└─ users/ (Collection)
   │
   └─ {uid}/ (Document - One per user)
      │
      ├─ Fields:
      │  ├─ uid: string
      │  ├─ email: string
      │  ├─ displayName: string
      │  ├─ age: number
      │  ├─ gender: "male" | "female" | "other"
      │  ├─ height: number (cm)
      │  ├─ weight: number (kg)
      │  ├─ fitnessGoal: "weight_loss" | "muscle_gain" | "endurance" | "general"
      │  ├─ experienceLevel: "beginner" | "intermediate" | "advanced"
      │  ├─ preferredWorkoutTime: "morning" | "afternoon" | "evening"
      │  ├─ intensityLevel: "quickie" | "classic" | "power" | "beast"
      │  ├─ createdAt: Timestamp
      │  └─ updatedAt: Timestamp
      │
      └─ completions/ (Subcollection - Workout logs)
         │
         └─ {dateKey}/ (Document - YYYY-MM-DD format, one per day)
            │
            └─ Fields:
               ├─ date: Timestamp
               ├─ dateKey: string (YYYY-MM-DD)
               ├─ workoutType: "quickie" | "classic" | "power" | "beast"
               ├─ workoutId: string (e.g., "quickie-1")
               ├─ duration: string (e.g., "25 min")
               ├─ calories: string (e.g., "250-350 calories")
               └─ completed: boolean
```

---

## Workout Data Structure (In-Memory)

```
workoutTypes (Object)
│
├─ quickie
│  ├─ name: "Quickie"
│  ├─ duration: "5-10 min"
│  ├─ description: string
│  ├─ color: "bg-green-500"
│  └─ workouts: Array[Workout]
│     ├─ quickie-1: Morning Energizer
│     └─ quickie-2: Power Burst
│
├─ classic
│  ├─ name: "Classic"
│  ├─ duration: "20-30 min"
│  ├─ description: string
│  ├─ color: "bg-primary"
│  └─ workouts: Array[Workout]
│     ├─ classic-1: Full Body Power
│     └─ classic-2: Strength Foundation
│
├─ power
│  ├─ name: "Power"
│  ├─ duration: "45-60 min"
│  ├─ description: string
│  ├─ color: "bg-orange-500"
│  └─ workouts: Array[Workout]
│     ├─ power-1: Athletic Performance
│     └─ power-2: Hypertrophy Focus
│
└─ beast
   ├─ name: "Beast Mode"
   ├─ duration: "1.5+ hrs"
   ├─ description: string
   ├─ color: "bg-red-500"
   └─ workouts: Array[Workout]
      └─ beast-1: Ultimate Challenge


Workout Object Structure:
{
  id: "quickie-1"
  name: "Morning Energizer"
  description: string
  duration: "5-10 min"
  difficulty: 1 (1-5 scale)
  category: "cardio" | "strength" | "flexibility" | "hiit" | "mixed"
  exercises: [
    {
      name: "Jumping Jacks"
      sets: 1
      reps: "30 seconds"
      rest: "10 seconds"
      videoUrl: "https://youtube.com/embed/..."
      videoThumbnail: "https://img.youtube.com/..."
    },
    ...
  ]
  guideLink: string
  blogTitle: string
  targetMuscles: ["Full Body", "Core", ...]
  equipment: ["None", "Dumbbells", ...]
  caloriesBurn: "50-80 calories"
  suitableFor: ["beginner", "intermediate", "advanced"]
  goals: ["weight_loss", "endurance", "general"]
}
```

---

## Routing & Navigation Map

```
Landing (Public)
    ↓ Click Sign In / Join Free
    └─→ Auth (Public)
         ├─ Login Tab
         │  └─ Email/Password or Google
         │
         └─ Signup Tab
            └─ Create account
            │
            └─ On Success: Navigate to /dashboard
                 │
                 ├─ Check: Is profile complete?
                 │
                 ├─ If No: Show "Complete Profile" Dialog
                 │   └─ Quick Save → Stay on Dashboard
                 │
                 └─ If Yes: Show Dashboard
                     │
                     ├─ Click "Complete Your Profile" → /setup
                     │  └─ 5-Step Wizard
                     │     └─ On Complete: Return to /dashboard
                     │
                     ├─ Click "Daily Workout" → /daily-workout
                     │  └─ Select workout
                     │     └─ Click "Start" → /workout-session
                     │        └─ Finish → Return to /daily-workout
                     │
                     ├─ Click "Profile" → /profile
                     │  └─ View/Edit profile & metrics
                     │     └─ Return to /dashboard
                     │
                     └─ Click "Logout" → Return to /auth

Protected Routes (Require Auth):
├─ /setup (Setup Wizard)
├─ /dashboard (Hub)
├─ /daily-workout (Workout Selection)
├─ /workout-session (Active Workout)
└─ /profile (Profile Management)

Public Routes:
├─ / (Landing)
├─ /auth (Authentication)
└─ * (404 Not Found)
```

---

## Development Workflow

```
1. Code Changes
   └─ Edit .tsx, .ts, .css files

2. Hot Module Replacement (HMR)
   └─ Vite detects changes
      └─ Browser auto-refresh
         └─ State often preserved

3. Type Checking
   └─ TypeScript compiler
      └─ Catches errors before runtime

4. Linting
   └─ ESLint runs
      └─ Checks code quality
         └─ React hooks rules

5. Build Process
   └─ npm run build
      └─ Vite bundle
         └─ Minify & optimize
            └─ Output to dist/

6. Deployment
   └─ Lovable platform
      └─ GitHub integration
         └─ Auto-deploy on push
```

---

## Performance Optimizations

```
1. Code Splitting
   └─ React Router lazy load pages

2. Image Optimization
   └─ Hero image in Landing (lazy load)

3. Caching Strategies
   └─ Firebase real-time listeners (subscriptions)
   └─ TanStack Query caching

4. Memoization
   └─ useMemo for expensive computations
   └─ useCallback for event handlers

5. Bundle Size
   └─ Tailwind CSS purging (unused styles)
   └─ Tree-shaking of dependencies

6. Lazy Loading
   └─ Video modals (on demand)
   └─ AI coach data (on open)
```

---

This architecture provides a solid foundation for a modern, scalable fitness application with clear separation of concerns, real-time data synchronization, and user-centric design.
