# Universal Web App System Instructions

This repository contains specialized system instructions designed for use with Gemini Ai Studio.

## Purpose
The goal of these instructions is to enable the generation of "Universal Vanilla Web Applications." These applications are architected to function correctly in two distinct environments without requiring code modification or build steps:

1. Bundled Environments: Google AI Studio, Vite, or Replit. The code utilizes a bridge entry point (index.tsx) to satisfy bundlers.
2. Local File System: The same code can be downloaded, and index.html can be opened directly in a browser via the file:// protocol.

## Usage
Copy the contents of the system_prompt.md file into Ai Studio System Instructions configuration (https://aistudio.google.com/apps) to generate modular, portable web apps.

## How It Works
Standard bundlers and local file execution have conflicting requirements. Bundlers typically rely on ES Modules; however, local execution via the file:// protocol blocks ES Modules due to Cross-Origin Resource Sharing (CORS) restrictions.

This system prompt instructs the AI to follow a "JS-Driven Namespace Strategy" to bypass these local limitations:

*   Namespace Architecture: All JavaScript code attaches to a global window.App object rather than using module exports.
*   Self-Executing Functions: Files use IIFEs to encapsulate logic and avoid global scope pollution, ensuring compatibility with strict file:// restrictions.
*   Bridge File: A dummy index.tsx file imports all JavaScript files in dependency order to trigger the bundler.
*   Template Injection: HTML structure is stored within JavaScript strings (e.g., in a UI namespace) rather than the HTML body, ensuring consistent rendering across both environments.

## Limitations vs. Standard Vite Apps
By prioritizing strict compatibility with the local file system, these applications face specific constraints compared to standard TypeScript environments:

*   No ES Modules: You cannot use import or export keywords in JavaScript files. Browsers block ES module loading on the file:// protocol due to CORS policies.
*   Strict CORS Rules: The file:// protocol is treated as a null origin. This prevents fetch() requests to local files and blocks module loading.
*   No Node Imports: You cannot import libraries from node_modules.
*   No TypeScript Logic: Logic must be written in plain JavaScript. Type safety is handled via JSDoc comments.
*   Manual Dependency Management: Because there are no imports, files must be loaded in a strict order (e.g., Utils before Logic, Logic before UI).

## External Libraries & Frameworks
Can I use frameworks?

*   Build-step Frameworks (NO): You cannot use React, Vue, Svelte, or Angular. These require compilation steps incompatible with local file execution.
*   Runtime Libraries (YES): You can use JavaScript libraries that function via a CDN script tag (e.g., Three.js, PixiJS, p5.js, GSAP, Anime.js, Howler.js, Tone.js, Alpine.js).
