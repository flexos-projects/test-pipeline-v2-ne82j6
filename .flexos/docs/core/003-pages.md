---
id: pages
title: Pages, Journeys & Navigation
description: The sitemap, page inventory, key user journeys, and navigation structure for Test Pipeline v2.
type: doc
subtype: core
status: draft
sequence: 3
tags:
  - pages
  - ux
  - navigation
createdAt: 2026-02-06T23:19:34.328Z
updatedAt: 2026-02-06T23:19:34.328Z
---

## 1. Site Map

The application is composed of a small set of public and protected pages to facilitate the core user flows. 

*   **Public Routes (No Authentication Required)**
    *   `/` - **Landing Page:** The main entry point for the application.
    *   `/login` - **Login Page:** Form for existing users to sign in.
    *   `/signup` - **Sign Up Page:** Form for new users to create an account.

*   **Protected Routes (Authentication Required)**
    *   `/dashboard` - **Dashboard:** The main application interface for managing tasks.

## 2. Page Inventory

This section details the purpose and key components of each page.

### `landing` (Route: `/`)
*   **Purpose:** To provide a clear entry point and direct users to authentication actions. It serves as the root for unauthenticated users.
*   **Key Components:**
    *   **Header:** App Name ("Test Pipeline v2") and tagline.
    *   **Call to Action:** Two primary buttons: "Login" (links to `/login`) and "Sign Up" (links to `/signup`).
    *   **Description:** A brief text block explaining the project's purpose: "A simple todo app for testing the generation pipeline."

### `login` & `signup` (Routes: `/login`, `/signup`)
*   **Purpose:** Standard authentication forms to handle user sessions. These may be a single page with tabs or two separate pages.
*   **Key Components:**
    *   Email Input Field
    *   Password Input Field
    *   Submit Button ("Login" or "Sign Up")
    *   Link to toggle between the Login and Sign Up views.
    *   Error message display area.

### `dashboard` (Route: `/dashboard`)
*   **Purpose:** The core workspace for authenticated users to manage their tasks. This page is the primary focus for E2E testing.
*   **Key Components:**
    *   **Header:** Displays the app name and a "Logout" button.
    *   **Task Creation Form:** An input field to type a new task title and an "Add Task" button.
    *   **Task List:** A dynamic list of the user's tasks. If no tasks exist, an empty state message ("You have no tasks.") will be shown.
    *   **Task Item:** Each item in the list will display:
        *   A checkbox to toggle the task's `status` between 'pending' and 'completed'.
        *   The task `title`. A completed task's title will have a strikethrough style.
        *   A delete button (e.g., a trash can icon) to remove the task.

## 3. User Journeys

These journeys describe the step-by-step path a user will take to accomplish key goals. These will form the basis of our E2E tests.

### Journey 1: New User Registration and First Task Creation
1.  **Start:** User arrives at the `Landing Page` (`/`).
2.  **Action:** Clicks the "Sign Up" button.
3.  **Navigate:** Is taken to the `Sign Up Page` (`/signup`).
4.  **Action:** Fills in their email and password and clicks "Sign Up".
5.  **System:** The system creates a new user account, logs them in, and redirects them.
6.  **Navigate:** User is automatically redirected to the `Dashboard` (`/dashboard`).
7.  **View:** Sees an empty task list.
8.  **Action:** Types "Test the pipeline" into the task creation input and clicks "Add Task".
9.  **View:** The new task appears in the list.
10. **End:** The user has successfully created an account and their first task.

### Journey 2: Existing User Session and Task Management
1.  **Start:** An existing user arrives at the `Landing Page` (`/`).
2.  **Action:** Clicks the "Login" button.
3.  **Navigate:** Is taken to the `Login Page` (`/login`).
4.  **Action:** Enters their credentials and clicks "Login".
5.  **Navigate:** Is redirected to the `Dashboard` (`/dashboard`).
6.  **View:** Sees their pre-existing list of tasks.
7.  **Action:** Clicks the checkbox next to the "Test the pipeline" task.
8.  **View:** The task's title now has a strikethrough.
9.  **Action:** Clicks the delete icon next to an old task.
10. **View:** The old task is removed from the list.
11. **Action:** Clicks the "Logout" button in the header.
12. **Navigate:** Is redirected back to the `Landing Page` (`/`).
13. **End:** The user has successfully managed their tasks and ended their session.