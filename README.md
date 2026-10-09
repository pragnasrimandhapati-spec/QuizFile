# Quizloom

A responsive quiz management app built with HTML, CSS, Bootstrap 5, vanilla JavaScript, Firebase Authentication, and Cloud Firestore.

## Run the demo

Open `index.html` in a modern browser. The app starts in demo mode when no Firebase API key is configured. Register a student or admin account from **Join free**. Demo accounts are stored in the browser and the signed-in role is kept in `sessionStorage`; demo mode is for UI exploration and is not secure authentication or shared storage. Use a distinct email for each role.

For Firebase persistence, serve the folder from a local web server (for example, `python -m http.server 8000`) and visit `http://localhost:8000`.

## Connect Firebase

1. Create a Firebase project and register a Web app.
2. In **Authentication → Sign-in method**, enable **Email/Password**.
3. Create a **Cloud Firestore** database.
4. Copy the Firebase web config into `js/firebase-config.js`.
5. Publish the rules from `firestore.rules` in Firestore → Rules.

The UI allows student and admin self-registration, with the selected role attached to the user's profile. For a real public deployment, restrict admin creation: remove admin self-registration from the UI and assign admin roles only from a trusted server/Admin SDK. Client-selected roles are not an authorization boundary.

## Features

- Role-specific student/admin sign-up and sign-in; duplicate email registration is rejected by Firebase or the local demo account registry.
- Firebase Auth session persistence uses browser session scope.
- Browse Python, Java, SQL, C++, and .NET quizzes; answer timed multiple-choice questions and see scores.
- One completed attempt per student and quiz. Firestore attempt documents use a stable student/quiz ID; a transaction-based 30-minute active-attempt lease prevents concurrent sessions.
- Admin quiz creation, title editing, and deletion; quizzes sync through Firestore.
- Upcoming quiz reminder inbox and responsive desktop/mobile layout.

## Data model

- `users/{uid}`: email, displayName, role, createdAt
- `quizzes/{quizId}`: topic, title, desc, level, time, questions, published, createdBy
- `attempts/{uid_quizId}`: uid, quizId, score, correct, total, answers, submittedAt
- `activeAttempts/{uid_quizId}`: uid, quizId, startedAt, expiresAt

Use `firestore.rules` as a starting point and review it for your deployment requirements. Never put service-account keys in this static app.
