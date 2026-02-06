---
id: design
title: Design System & Brand
description: The visual and brand guidelines for Test Pipeline v2, including colors, typography, components, and accessibility.
type: doc
subtype: core
status: draft
sequence: 6
tags:
  - design
  - ui
  - accessibility
createdAt: 2026-02-06T23:19:34.328Z
updatedAt: 2026-02-06T23:19:34.328Z
---

## 1. Overview

This document establishes the design system for "Test Pipeline v2." While the application is utilitarian, a consistent and clear design is essential for usability and for creating a professional-grade testbed. The aesthetic is minimal, clean, and functional, prioritizing clarity over ornamentation.

## 2. Brand Voice & Tone

*   **Voice:** Utilitarian and direct. The application communicates information clearly and without embellishment. It is a tool for developers, and its language should reflect that.
*   **Tone:** Neutral and informative. UI copy is concise and action-oriented. Examples: "Add Task," "Login," "Task completed."

## 3. Color System

The color palette is based on a primary brand color and a functional set of neutral and feedback colors.

*   **Primary:**
    *   `primary-500`: `#10b981` (Emerald 500) - Used for primary interactive elements like buttons, links, and focus rings.

*   **Neutral (Grayscale):**
    *   `gray-900`: `#111827` - For primary body text and headings.
    *   `gray-600`: `#4b5563` - For secondary text, labels, and placeholders.
    *   `gray-300`: `#d1d5db` - For borders and dividers.
    *   `gray-100`: `#f3f4f6` - For page backgrounds.
    *   `white`: `#ffffff` - For element backgrounds like cards and inputs.

*   **Feedback:**
    *   `red-500`: `#ef4444` - For destructive actions (e.g., delete button) and error messages.
    *   `blue-500`: `#3b82f6` - For informational alerts or messages.

## 4. Typography

We will use a single, highly-legible font family to ensure consistency and readability.

*   **Font Family:** `Inter` (sans-serif). Served via a font provider like Google Fonts.
*   **Typographic Scale:**
    *   **Heading 1:** 30px, Bold (700 weight) - Page titles (e.g., "Dashboard").
    *   **Heading 2:** 20px, Semi-Bold (600 weight) - Section titles.
    *   **Body (Default):** 16px, Regular (400 weight) - All primary text, task titles.
    *   **Label/Small:** 14px, Regular (400 weight) - Helper text, labels.

## 5. Core UI Components

This is a small library of reusable components that will form the building blocks of the UI.

*   **Button:**
    *   **Primary Action:** Solid `#10b981` background with white text. Used for "Add Task," "Login."
    *   **Secondary Action:** White background with a `#10b981` border and text. Used for "Sign Up."
    *   **Destructive Action:** Icon-only button with a transparent background. The icon color is `gray-600` by default and changes to `red-500` on hover. Used for deleting a task.

*   **Form Input:**
    *   A text input field with a `white` background and a `gray-300` border.
    *   On focus, the border color changes to `primary-500` (`#10b981`).
    *   Placeholder text uses the `gray-600` color.

*   **Task Item:**
    *   A container (card) with a `white` background, a subtle box-shadow, and rounded corners.
    *   Horizontally arranges a checkbox, the task title, and a delete button.
    *   When a task's status is `completed`, the title text will have a `text-decoration: line-through` and its color will change to `gray-600`.

## 6. Accessibility (A11y)

Accessibility is a primary consideration to ensure the application is usable by everyone and to test our base components for compliance.

*   **Color Contrast:** All text and background color combinations must meet WCAG AA standards. The primary color `#10b981` on `white` has a contrast ratio of 4.26:1. For normal-sized text (16px), this requires a bold font weight. We will ensure all interactive elements meet this requirement.
*   **Keyboard Navigation:** All interactive elements—including links, buttons, form fields, and checkboxes—must be reachable and operable using the Tab key. A visible focus ring (`outline: 2px solid #10b981`) will be present on all focused elements.
*   **Semantic HTML:** Use appropriate HTML tags (`<nav>`, `<main>`, `<button>`, `<label>`) to provide inherent meaning and structure for screen readers.
*   **ARIA Attributes:** For icon-only buttons like the delete action, an `aria-label` will be included to describe its function (e.g., `aria-label="Delete task"`).