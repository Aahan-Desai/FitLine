# FitLine – AI Powered Fitness Web Application

FitLine is a full-stack fitness web application designed to help users manage workouts, monitor progress, and receive AI-driven fitness guidance. The application combines structured workout routines, real-time workout tools, and personalized analytics to support users in achieving their fitness goals.

The platform includes an AI fitness coach, progress tracking dashboards, smart workout routines, and a guided onboarding process that calculates key health metrics such as BMI and daily calorie requirements.

Live Demo:  
https://fit-line-main.vercel.app/

---

## Features

### AI Fitness Coach

A conversational interface that analyzes workout history and provides personalized fitness guidance.

### Smart Workout Programs

Predefined workout routines designed for different experience levels, ranging from beginner sessions to advanced training modes.

### Interactive Workout Timer

A structured workout timer with configurable work and rest phases, real-time feedback, and voice guidance using text-to-speech.

### Progress Tracking and Analytics

Dashboard visualizations displaying:

- Weekly workout completion
- Total workout sessions
- Weight tracking
- Estimated calorie burn

### Personalized Onboarding

A multi-step onboarding system that collects user data and calculates:

- Body Mass Index (BMI)
- Total Daily Energy Expenditure (TDEE)
- Customized fitness recommendations

### Secure Authentication

User authentication implemented with Firebase, supporting:

- Email and password login
- Google OAuth sign-in

### Responsive Interface

Fully responsive UI optimized for mobile, tablet, and desktop devices.

---

## Tech Stack

### Frontend

- React 18
- TypeScript
- Vite

### UI and Styling

- Tailwind CSS
- shadcn/ui
- Lucide Icons

### Backend and Database

- Firebase Authentication
- Firebase Firestore

### State Management

- React Context API
- TanStack Query

### Data Visualization

- Recharts

### Deployment

- Vercel

---

## Project Architecture

The application follows a modular frontend architecture with Firebase providing backend services.

Key architectural components include:

- Component-based UI built with React
- Centralized state management using React Context
- Server state handling using TanStack Query
- Firebase for authentication and real-time database
- Modular UI components using shadcn

Detailed documentation is available in:

- `PROJECT_DOCUMENTATION.md`
- `ARCHITECTURE.md`

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/Aahan-Desai/FitLine.git
cd FitLine
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Environment Configuration

Create a `.env.local` file in the root directory and add your Firebase configuration.

```env
VITE_FIREBASE_API_KEY=your_api_key_here
VITE_FIREBASE_AUTH_DOMAIN=your_project_id.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project_id.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
VITE_FIREBASE_APP_ID=your_app_id
VITE_FIREBASE_MEASUREMENT_ID=your_measurement_id
```

### 4. Run the Development Server

```bash
npm run dev
```

The application will start on the local development server.

---

## Deployment

The project is deployed using Vercel.

Deployment instructions are available in:

- `VERCEL_DEPLOYMENT_GUIDE.md`

---

## Future Improvements

Potential improvements for the project include:

- AI-based workout plan generation
- Wearable device integration
- Nutrition tracking module
- Social fitness challenges and leaderboards

---

## Author

Aahan Desai
