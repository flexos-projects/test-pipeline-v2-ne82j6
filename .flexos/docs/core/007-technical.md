---
id: technical
title: Technical Architecture
description: The technical stack, architecture, API design, deployment process, and other engineering details.
type: doc
subtype: core
status: draft
sequence: 7
tags:
  - architecture
  - tech-stack
  - deployment
createdAt: 2026-02-06T23:19:34.328Z
updatedAt: 2026-02-06T23:19:34.328Z
---

## 1. Architecture Overview

"Test Pipeline v2" will be implemented using a **Jamstack/Serverless architecture**. This approach decouples the frontend from the backend, allowing for independent development, scaling, and deployment. The frontend will be a static-first Single Page Application (SPA) that communicates directly with Backend-as-a-Service (BaaS) providers for data and authentication. This minimizes infrastructure overhead, making it an ideal choice for a focused test application.

## 2. Technology Stack

*   **Frontend Framework:** **React 18+** with **Vite**. Vite is chosen for its extremely fast development server and optimized build process.
*   **Language:** **TypeScript**. For type safety and improved developer experience, preventing common bugs.
*   **Styling:** **Tailwind CSS**. A utility-first CSS framework that enables rapid development of UIs consistent with our [Design System](./006-design.md) without writing custom CSS files.
*   **Backend-as-a-Service (BaaS):** **Firebase**. We will leverage two core Firebase services:
    *   **Authentication:** Firebase Authentication will handle all user management (sign-up, login, session management) using the email/password provider.
    *   **Database:** **Cloud Firestore** will serve as our NoSQL document database. Its real-time capabilities and robust security rules engine are key features we aim to test.
*   **Deployment & Hosting:** **Vercel**. Vercel provides a seamless Git-based workflow. It will automatically build, deploy, and host our React application.
*   **End-to-End Testing:** **Playwright**. To be run within the Vercel deployment pipeline to validate the application's core functionality.

## 3. Frontend Structure

The React application will be structured by feature for modularity.

```
/src
|-- /components       # Reusable UI components (Button, Input, etc.)
|-- /features         # Feature-specific logic and components
|   |-- /auth         # Login, SignUp forms, auth hooks
|   |-- /tasks        # TaskList, TaskItem, task management hooks
|-- /hooks            # Generic custom hooks (e.g., useAuth)
|-- /lib              # Third-party service initializations (e.g., firebase.ts)
|-- /pages            # Page components mapping to routes
|-- App.tsx           # Main application component with routing
|-- main.tsx          # Application entry point
```

## 4. API & Data Flow

There will be **no custom backend server or API routes**. All communication with backend services will be handled on the client-side via the official Firebase JS SDK.

*   **Firebase SDK Initialization:** A single configuration file (`src/lib/firebase.ts`) will initialize the Firebase app using environment variables (`VITE_FIREBASE_*`). This file will export the initialized `auth` and `firestore` service instances.

*   **Data Abstraction:** We will use custom React hooks to encapsulate all Firebase logic, making components cleaner and logic reusable.
    *   `useAuth()`: A hook that provides the current user's authentication state, along with `login`, `signup`, and `logout` methods.
    *   `useTasks()`: A hook that subscribes to a real-time Firestore query for the current user's tasks. It returns the task list, loading state, and any errors.
    *   This hook-based approach makes it easy to manage component state and side effects related to backend data.

## 5. Deployment and CI/CD

The deployment process is fully automated via Vercel's integration with GitHub.

1.  **Source Control:** The codebase is hosted in a GitHub repository.
2.  **Trigger:** A `git push` or merged Pull Request to the `main` branch automatically triggers a new deployment on Vercel.
3.  **Build Step:** Vercel executes the `build` command defined in `package.json` (`vite build`). This transpiles the TypeScript/React code into optimized static HTML, CSS, and JavaScript assets.
4.  **Test Step:** After a successful build, Vercel will run the E2E tests (`npx playwright test`). The deployment will only proceed if all tests pass.
5.  **Deployment:** If tests pass, Vercel deploys the built assets to its global CDN. The production domain (`test-pipeline-v2.vercel.app` or a custom domain) is atomically updated to point to the new deployment, ensuring zero downtime.

## 6. Environment Variables

All sensitive keys and environment-specific configurations will be managed using Vercel's Environment Variables settings. They will not be hardcoded in the repository.

*   `VITE_FIREBASE_API_KEY`
*   `VITE_FIREBASE_AUTH_DOMAIN`
*   `VITE_FIREBASE_PROJECT_ID`
*   `VITE_FIREBASE_STORAGE_BUCKET`
*   `VITE_FIREBASE_MESSAGING_SENDER_ID`
*   `VITE_FIREBASE_APP_ID`

These variables are prefixed with `VITE_` to be exposed to the client-side application by Vite during the build process.