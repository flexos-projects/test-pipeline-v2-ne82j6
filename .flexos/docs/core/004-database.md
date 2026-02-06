---
id: database
title: Database Schema & Rules
description: Detailed schema for all collections, their relationships, required indexes, and security rules.
type: doc
subtype: core
status: draft
sequence: 4
tags:
  - database
  - schema
  - security
createdAt: 2026-02-06T23:19:34.328Z
updatedAt: 2026-02-06T23:19:34.328Z
---

## 1. Overview

This document defines the data architecture for "Test Pipeline v2." We will use a NoSQL document database (specifically, Firestore) due to its flexible schema and powerful, user-based security model. The architecture is designed to be simple yet robust enough to thoroughly test our pipeline's interaction with a backend data store.

## 2. Collections

There is only one primary data collection required for the MVP scope.

### `tasks`

This collection stores all the task documents created by all users.

*   **Purpose:** To persist the core data entity of the application.
*   **Structure:** Each document within this collection represents a single task.

## 3. Document Schemas

### Schema: `tasks` document

This defines the structure of a single document within the `tasks` collection.

| Field       | Type      | Description                                                                                             | Constraints / Default               |
|-------------|-----------|---------------------------------------------------------------------------------------------------------|-------------------------------------|
| `userId`    | `string`  | The UID of the user who owns this task. This is the foreign key linking to the authentication user.        | Required, Immutable                 |
| `title`     | `string`  | The text content of the task.                                                                           | Required, Max 280 chars             |
| `status`    | `string`  | The current state of the task.                                                                          | Required, Enum: `pending`, `completed`. Default: `pending` |
| `createdAt` | `timestamp` | Server-generated timestamp indicating when the task was created.                                        | Required, Server-written            |
| `updatedAt` | `timestamp` | Server-generated timestamp indicating the last time the task was modified.                              | Required, Server-written            |

**Example Document:**
```json
{
  "userId": "aBcDeFgHiJkLmNoPqRsT12345",
  "title": "Validate the deployment script",
  "status": "pending",
  "createdAt": "2026-02-10T10:00:00Z",
  "updatedAt": "2026-02-10T10:00:00Z"
}
```

## 4. Relationships

*   **Users ↔ Tasks:** A **one-to-many** relationship exists between a user (managed by the Authentication service) and the documents in the `tasks` collection. One user can have many tasks, but each task belongs to exactly one user. This relationship is enforced by the `userId` field in each task document and is critical for the security rules.

## 5. Indexes

To ensure efficient querying, especially as the number of tasks grows, the following composite indexes will be configured for the `tasks` collection:

1.  **Primary Query Index:**
    *   **Fields:** `userId` (Ascending), `createdAt` (Descending)
    *   **Purpose:** This is the main index used by the `/dashboard` page to fetch all tasks for the logged-in user, sorted with the newest tasks appearing first. This is a critical performance optimization.

2.  **Filtering Index (for P1 Feature):**
    *   **Fields:** `userId` (Ascending), `status` (Ascending), `createdAt` (Descending)
    *   **Purpose:** To efficiently handle queries where a user is filtering their tasks by status (e.g., show only 'completed' tasks), while still maintaining a chronological sort order.

## 6. Security Rules

The following security rules (written for Firestore) are essential to ensure data privacy and are a primary component being tested by this application.

```javascript
// rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // The 'tasks' collection
    match /tasks/{taskId} {

      // READ: A user can only read tasks where their UID matches the task's userId.
      allow read: if request.auth != null && request.auth.uid == resource.data.userId;

      // CREATE: A user can only create a task if they are logged in and the new task's
      // userId is set to their own UID.
      allow create: if request.auth != null && request.auth.uid == request.resource.data.userId;

      // UPDATE: A user can only update a task they own. We also restrict which fields
      // can be changed (e.g., cannot change the userId or createdAt).
      allow update: if request.auth != null && request.auth.uid == resource.data.userId
                    && request.resource.data.userId == resource.data.userId
                    && request.resource.data.createdAt == resource.data.createdAt;

      // DELETE: A user can only delete tasks they own.
      allow delete: if request.auth != null && request.auth.uid == resource.data.userId;
    }
  }
}
```
**Rule Explanation:**
*   **Authentication Check:** `request.auth != null` ensures that no unauthenticated user can perform any action.
*   **Ownership Check:** `request.auth.uid == resource.data.userId` is the core logic. It compares the UID of the user making the request with the `userId` stored on the document they are trying to access. This effectively isolates data between users.