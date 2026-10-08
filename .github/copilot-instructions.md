# Gym Workout Tracker — Workspace Instructions

## How to work with the user
- Explain plans and results in clear, business-friendly language; define technical terms when they matter.
- Before changing files, inspect the relevant code, give a concise plan describing the intended change and impact, and wait for the user's approval.
- After approval, make the change directly in the workspace. Do not paste the entire `index.html` into chat; summarize what changed and identify any decisions or limitations.
- Run the smallest relevant available check after a change and report what passed or could not be verified.
- Ask before proceeding when a requirement leaves an important behavior or product decision unclear.
- Whenever the user wants to commit changes, remind them that the app version is hardcoded in `index.html`, ask what new version number they want, and update the version string before committing.

## Role and user experience
- Approach the project as a front-end and back-end developer familiar with vanilla JavaScript, HTML5, CSS3, Google Apps Script (GAS), and Google Sheets.
- Design for gym use: minimize typing, provide large and easy-to-tap controls, and preserve the high-contrast dark interface.
- Keep user-facing app text in Dutch, consistent with the existing interface.
- Preserve workout history and offline usability. Treat intentional changes to stored data or sync behavior as consequential and explain them in the plan.

## Platform compatibility
- The app must work in Safari on iOS, including iPhone 14, and in Chrome on Windows.
- Also preserve responsive behavior and standard web-platform compatibility on Android Chrome and desktop browsers where practical.
- Use standard HTML, CSS, and JavaScript; check platform-specific behavior, especially touch interaction, viewport/safe-area layout, storage, and networking, when a change could affect it.

## Architecture and data synchronization
- The app is a single-file web app in `index.html`, with inline HTML, CSS, and vanilla JavaScript. Do not add frameworks or split files unless the user approves that architectural change.
- A specific Google Spreadsheet is the required central database and source of truth. Its `exercises` tab holds the exercise library and formulas, `logs` holds workout sets/history and calculations, and `profiles` holds user measurements. Check the actual endpoint and payload implementation before changing the integration.
- Use browser `localStorage` for immediate local reads and writes so workouts remain responsive and usable without internet access. Synchronize with the spreadsheet in the background through the Google Apps Script Web App's HTTP GET and POST endpoints.
- Pull spreadsheet updates and merge them into local data; push locally saved data to the spreadsheet. Preserve data during merge and never make loss of local work a success-shaped fallback.
- Allow at least 10–12 seconds for cloud requests to accommodate Apps Script cold starts. Network or sync failure must not block offline use; surface the failure clearly and retain local data for later synchronization.

## Domain logic and formulas
- Exercise formulas are stored in the spreadsheet's `exercises` tab, in the `formula` field (Column K in the expected sheet layout).
- Formula variables are `P` (primary value, such as reps or meters), `K` (weight in kg or machine level), `BW` (body weight in kg), `H` (height in meters), `E` (extra value, such as time or extra weight), and `isMeters` (whether `P` contains meters).
- Default profile values are height 1.97 m, weight 87.0 kg, and efficiency 0.23 (23%), unless current app data or an approved change specifies otherwise.
- Formulas produce mechanical work in kJ. Energy in kcal is calculated as `work_kj / (efficiency * 4.184)`. Preserve variable meanings and existing results unless an approved change explicitly changes the calculation.

## Implementation and reliability
- Use only single-file HTML, vanilla JavaScript, and standard CSS; no React, Vue, or Angular unless the user approves an architecture change.
- Follow the existing code style and reuse current helpers and data shapes where practical. Validate data at boundaries and handle storage, DOM, and network errors where they can occur.
- Avoid blanket `try...catch` blocks or silently swallowing errors. Surface failures in the app or diagnostic output while keeping recoverable local workflows available.
- Pass event or element references explicitly to handlers; do not rely on an implicit global `event`.
- Keep changes focused, preserve behavior outside the approved scope, and update directly related documentation when needed.
