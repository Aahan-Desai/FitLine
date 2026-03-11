# FitLine-Gym: AI-Powered Fitness Application

FitLine is a professional, production-ready fitness web application that combines personalized workout management, AI-powered coaching, and real-time progress tracking.

## Live Demo

Check it out here: [https://fit-line-main.vercel.app/](https://fit-line-main.vercel.app/)

## Key Features

- AI Fitness Coach: A real-time chat interface that learns from your workout history and provides personalized advice.
- Smart Workouts: 9+ pre-designed routines ranging from "Quickie" to "Beast" mode, tailored to your experience level.
- Interactive Timer: Advanced workout timer with work/rest phases, voice guidance (Text-to-Speech), and visual feedback.
- Progress Analytics: Track your weekly completions, total sessions, weight changes, and calorie burn.
- Personalized Setup: A 5-step onboarding wizard to calculate BMI, TDEE, and customize your fitness journey.
- Secure Authentication: Email and Google OAuth integration via Firebase.
- Fully Responsive: Optimized for mobile, tablet, and desktop.

## Tech Stack

- Frontend: React 18, TypeScript, Vite
- Styling: Tailwind CSS, shadcn/ui, Lucide Icons
- Backend/Database: Firebase (Authentication & Firestore)
- State Management: React Context, TanStack Query
- Charts: Recharts

## Getting Started

1. Clone the repository:

   ```bash
   git clone https://github.com/Aahan-Desai/FitLine.git
   cd FitLine
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Environment Setup:
   Create a .env.local file with your Firebase configuration:

   ```env
   VITE_FIREBASE_API_KEY=your_api_key_here
   VITE_FIREBASE_AUTH_DOMAIN=your_project_id.firebaseapp.com
   VITE_FIREBASE_PROJECT_ID=your_project_id
   VITE_FIREBASE_STORAGE_BUCKET=your_project_id.appspot.com
   VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
   VITE_FIREBASE_APP_ID=your_app_id
   VITE_FIREBASE_MEASUREMENT_ID=your_measurement_id
   ```

4. Run development server:
   ```bash
   npm run dev
   ```

## Documentation

The project includes comprehensive guides in the root directory:

- PROJECT_DOCUMENTATION.md: Full architectural overview.
- ARCHITECTURE.md: System design and data flow diagrams.
- VERCEL_DEPLOYMENT_GUIDE.md: Step-by-Step guide for hosting.

## Author

Aahan Desai

---

Built to help people reach their fitness goals through technology.
