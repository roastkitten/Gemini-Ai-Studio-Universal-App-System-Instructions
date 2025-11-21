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

## Usage

Copy the contents of `system_prompt.md` into the "System Instructions" field in Google AI Studio to start generating standalone, portable web apps.
