# Universal Web App System Instructions for Google Gemini

This repository contains a specialized system prompt (`system_prompt.md`) designed for use with Google Gemini AI Studio. 

## Purpose

The goal of these instructions is to enable Google Gemini to generate "Universal" or "Hybrid" vanilla web applications. These applications are unique because they function correctly in two distinct environments without requiring code modification or build steps:

1.  **Google AI Studio / Vite:** The generated code satisfies the bundler requirements of the AI Studio preview environment (using a bridge `index.tsx`).
2.  **Local File System:** The same code can be downloaded, and `index.html` can be opened directly in a browser via the `file://` protocol. 

## How It Works

Standard bundlers (like Vite) and local file execution often conflict. Bundlers want ES Modules and entry points; local execution via `file://` blocks ES Modules due to CORS policies.

This system prompt instructs the AI to follow a **"Hybrid Injection Strategy"**:
*   It generates a `main.js` that avoids ES modules and uses self-executing functions (IIFE).
*   It includes a "self-healing" mechanism that injects the HTML structure if the bundler ignores the HTML body.
*   It uses a dummy `index.tsx` solely to import the CSS and JS for the bundler environment.

## Limitations vs. Standard Vite Apps

By prioritizing strict compatibility with the local file system (`file://`), these applications face specific constraints compared to standard Vite/TypeScript environments found in AI Studio:

*   **No ES Modules:** You cannot use `import` and `export` keywords to organize code across multiple files. All logic must reside within the single `main.js` file or share state via global variables.
*   **No NPM Imports:** You cannot import libraries from `node_modules`. 
*   **No TypeScript:** The logic must be written in plain JavaScript. Browsers cannot execute TypeScript directly without a build step.
*   **No Environment Variables:** You cannot rely on `.env` files or `process.env`. API keys must be input by the user via the UI.
*   **Strict CORS Rules:** The `file://` protocol is treated as a `null` origin. This prevents `fetch()` requests to local files and may block some external API calls.

## External Libraries & Frameworks

**Can I use frameworks?**
*   **Build-step Frameworks (NO):** You **cannot** use React (JSX), Vue (.vue files), Svelte, or Angular. These require a compilation step that does not exist when opening a file locally.
*   **Runtime Libraries (YES):** You **can** use "regular" JavaScript libraries and lightweight frameworks that function via a CDN script tag (UMD/Global build).
    *   *Examples:* Alpine.js, jQuery, p5.js, D3.js, Three.js, TailwindCSS (via CDN script).

## Usage

Copy the contents of `system_prompt.md` into the "System Instructions" field in Google AI Studio to start generating standalone, portable web apps.
