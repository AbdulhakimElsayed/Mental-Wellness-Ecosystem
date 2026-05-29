# Mental Wellness Ecosystem

![Mental Wellness Ecosystem Banner](https://via.placeholder.com/1200x300/007bff/ffffff?text=Mental+Wellness+Ecosystem)

## Table of Contents

- [About the Project](#about-the-project)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Backend Setup](#backend-setup)
  - [Dashboard Setup](#dashboard-setup)
  - [Mobile App Setup](#mobile-app-setup)
- [Testing the Full Flow](#testing-the-full-flow)
- [Deployment Suggestions](#deployment-suggestions)
- [Important Notes](#important-notes)
- [Future Improvements](#future-improvements)
- [License](#license)
- [Author](#author)

## About the Project

The Mental Wellness Ecosystem is a comprehensive suite designed to help users track and improve their mental well-being through mood logging, sleep tracking, and cognitive training. It comprises a robust backend, an intuitive web dashboard, and a user-friendly mobile application, all working in harmony to provide a holistic view of mental health data.

## Features

This ecosystem offers a rich set of features to support mental wellness:

-   **User Authentication:** Secure registration and login functionalities using JSON Web Tokens (JWT).
-   **Mood Logging:** Users can log their daily mood with optional text notes. The system automatically performs sentiment analysis on these notes using a Hugging Face model (`finiteautomata/bertweet-base-sentiment-analysis`) to provide deeper insights.
-   **Sleep Tracking:** Record sleep duration and fatigue levels (on a scale of 1-10) to understand sleep patterns and their impact on well-being.
-   **N-back Cognitive Game:** An integrated n-back game for working memory training, tracking performance metrics such as score, reaction time, and accuracy.
-   **Interactive Dashboard (Next.js):**
    -   **Calendar Heatmap:** Visualizes mood scores over time with a color-coded calendar.
    -   **Sleep vs. Fatigue Chart:** A dual Y-axis line chart comparing sleep duration against fatigue levels.
    -   **Recent Mood Entries:** A list displaying recent mood entries, including sentiment emojis (😊😞😐).
    -   **Cognitive Game Results Table:** A detailed table showing game date, type, score/rounds, accuracy, and reaction time.
-   **Offline-First Mobile App:** The mobile application is designed to work offline, storing data locally using SQLite and synchronizing with the backend when an internet connection is available.
-   **Secure Token Storage:** Authentication tokens are securely stored using `flutter_secure_storage` in the mobile app and cookies in the dashboard.
-   **API Endpoints:**
    -   **Auth:**
        -   `POST /api/auth/register`: User registration.
        -   `POST /api/auth/login`: User login.
    -   **Mood CRUD:**
        -   `POST /api/mood`: Create a new mood entry with automatic sentiment analysis.
        -   `GET /api/mood`: Retrieve all mood entries.
        -   `PUT /api/mood/:id`: Update an existing mood entry.
        -   `DELETE /api/mood/:id`: Delete a mood entry.
    -   **Sleep CRUD:**
        -   `POST /api/sleep`: Create a new sleep log.
        -   `GET /api/sleep`: Retrieve all sleep logs.
        -   `PUT /api/sleep/:id`: Update an existing sleep log.
        -   `DELETE /api/sleep/:id`: Delete a sleep log.
    -   **Games:**
        -   `POST /api/games`: Save n-back game results.
        -   `GET /api/games`: Retrieve all game results.

## Tech Stack

The project leverages a modern and robust tech stack across its three main components:

### Backend

-   **Runtime:** Node.js
-   **Framework:** Express.js
-   **Database:** PostgreSQL (`pg`)
-   **Authentication:** JWT (`jsonwebtoken`), Password Hashing (`bcrypt`)
-   **Environment Management:** `dotenv`
-   **CORS:** `cors`
-   **HTTP Client:** `axios` (for Hugging Face API integration)

### Dashboard

-   **Framework:** Next.js 14 (App Router)
-   **Language:** TypeScript
-   **Styling:** Tailwind CSS
-   **Charting:** Recharts
-   **Calendar:** React Calendar
-   **HTTP Client:** `axios`
-   **Cookies:** `cookies-next`

### Mobile App

-   **Framework:** Flutter (Dart)
-   **State Management:** Provider
-   **Local Database:** `sqflite`
-   **HTTP Client:** `http`
-   **Secure Storage:** `flutter_secure_storage`
-   **Preferences:** `shared_preferences`
-   **Path Management:** `path`

## Project Structure

The repository is organized into three main sub-projects:

```
mental-wellness-ecosystem/
├── mental-wellness-backend/
│   ├── src/
│   │   ├── controllers/
│   │   ├── routes/
│   │   ├── middleware/
│   │   ├── services/ (aiService.js)
│   │   └── utils/
│   ├── database/ (schema.sql)
│   ├── .env
│   └── package.json
├── mental-wellness-dashboard/
│   ├── app/
│   │   ├── components/ (CalendarView, SleepChart, RecentEntries, GameResultsTable)
│   │   ├── dashboard/
│   │   ├── login/
│   │   ├── lib/ (api.ts, auth.ts)
│   │   └── types/
│   ├── .env.local
│   └── package.json
└── mental_wellness_app/
    ├── lib/
    │   ├── screens/ (nback_game_screen.dart)
    │   ├── services/ (api_service.dart, local_db_service.dart)
    │   ├── providers/ (auth_provider.dart)
    │   └── models/
    ├── pubspec.yaml
    └── assets/
```

## Getting Started

Follow these instructions to set up and run the Mental Wellness Ecosystem locally.

### Prerequisites

Before you begin, ensure you have the following installed:

-   Node.js (LTS version recommended)
-   npm or Yarn
-   PostgreSQL
-   Flutter SDK
-   Git

### Backend Setup

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/AbdulhakimElsayed/mental-wellness-ecosystem.git
    cd mental-wellness-ecosystem/mental-wellness-backend
    ```
2.  **Install dependencies:**
    ```bash
    npm install
    # or yarn install
    ```
3.  **Set up PostgreSQL:**
    *   Ensure your PostgreSQL server is running.
    *   Create a new database for the project.
    *   Run the schema migration from `database/schema.sql` to create the necessary tables.
4.  **Configure environment variables:**
    *   Create a `.env` file in the `mental-wellness-backend/` directory.
    *   Add the following variables, replacing placeholders with your actual values:
        ```env
        PORT=5000
        DATABASE_URL="postgresql://user:password@host:port/database_name"
        JWT_SECRET="your_jwt_secret_key"
        HUGGING_FACE_API_KEY="YOUR_HUGGING_FACE_API_KEY"
        ```
    *   You can obtain a Hugging Face API key from [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens).
5.  **Start the backend server:**
    ```bash
    npm run dev
    # or yarn dev
    ```
    The backend server will typically run on `http://localhost:5000`.

### Dashboard Setup

1.  **Navigate to the dashboard directory:**
    ```bash
    cd ../mental-wellness-dashboard
    ```
2.  **Install dependencies:**
    ```bash
    npm install
    # or yarn install
    ```
3.  **Configure environment variables:**
    *   Create a `.env.local` file in the `mental-wellness-dashboard/` directory.
    *   Add the following variable, pointing to your backend API URL:
        ```env
        NEXT_PUBLIC_API_URL="http://localhost:5000/api"
        ```
        *Note: If your backend is deployed, use its public URL.*
4.  **Start the dashboard:**
    ```bash
    npm run dev
    # or yarn dev
    ```
    The dashboard will typically run on `http://localhost:3000`.

### Mobile App Setup

1.  **Navigate to the mobile app directory:**
    ```bash
    cd ../mental_wellness_app
    ```
2.  **Get Flutter dependencies:**
    ```bash
    flutter pub get
    ```
3.  **Update API URL:**
    *   Open `lib/constants.dart` (or similar file) and update the `BASE_URL` or `API_URL` constant to point to your backend server.
    *   **Important for local testing on a real device:** If testing on a physical mobile device connected to your local network, you must use your PC's local IP address (e.g., `http://192.168.1.X:5000/api`) instead of `localhost`. Ensure both your PC and mobile device are on the same Wi-Fi network.
4.  **Run the Flutter app:**
    ```bash
    flutter run
    ```
    Choose your desired emulator or connected device.

## Testing the Full Flow

To ensure all components are working together, follow these steps:

1.  **Start all services:** Ensure the Backend, Dashboard, and Mobile App are all running as per their setup instructions.
2.  **Register a new user:** Use the mobile app's registration screen to create a new account.
3.  **Log in:** Log in to the mobile app with the newly created credentials.
4.  **Add Mood and Sleep entries:** Use the mobile app to add several mood entries (with notes for sentiment analysis) and sleep logs.
5.  **Play N-back game:** Play the n-back cognitive game and save your results.
6.  **Check Dashboard:** Log in to the Next.js dashboard with the same user credentials. Verify that:
    *   Your mood entries appear on the calendar heatmap.
    *   The sleep vs. fatigue chart displays your logged data.
    *   Recent mood entries show up with sentiment emojis.
    *   Your cognitive game results are listed in the table.

## Deployment Suggestions

Here are some recommendations for deploying each part of the ecosystem:

-   **Backend:** [Render.com](https://render.com/) or [Railway](https://railway.app/) for easy Node.js deployment with PostgreSQL integration.
-   **Dashboard:** [Vercel](https://vercel.com/) for seamless Next.js deployment.
-   **Mobile App:**
    -   **Android:** Generate an APK using `flutter build apk` and distribute it.
    -   **iOS:** Use TestFlight for beta testing and distribute via the Apple App Store.

## Important Notes

-   **Security:** Never commit sensitive files like `.env`, `.env.local`, `node_modules`, `.next/`, or `build/` to your version control system. These are typically ignored by `.gitignore` files.
-   **Hugging Face Token:** A valid Hugging Face API token is required for the sentiment analysis feature to function correctly. Obtain one from [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens).
-   **Local Mobile Testing:** When testing the mobile app on a physical device locally, ensure your device and development machine are on the same Wi-Fi network. Use your PC's local IP address instead of `localhost` for API calls.

## Future Improvements

This project is designed with scalability in mind. Here are some potential future enhancements:

-   **Push Notifications:** Implement reminders for logging mood, sleep, or playing cognitive games.
-   **Voice Analysis:** Integrate `wav2vec2` or similar models for speech emotion recognition or vocal fatigue detection.
-   **PDF Export:** Add functionality to generate and export PDF reports of user data from the dashboard.
-   **Advanced Analytics:** Introduce more sophisticated data visualizations and predictive analytics.
-   **Community Features:** Explore options for anonymous data sharing or community challenges.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

[AbdulhakimElsayed](https://github.com/AbdulhakimElsayed)
