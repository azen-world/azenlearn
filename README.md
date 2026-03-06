# AZEN Learn

AZEN Learn is a lightweight static web prototype for a Functional Skills Maths platform.

It currently includes:
- A marketing homepage (`index.html`)
- A simple Supabase-backed login page (`login.html`)
- A basic dashboard (`dashboard.html`)
- A step-by-step lesson page (`lesson.html`)
- Shared marketing styles (`styles.css`)

## Run locally

Because this project uses plain HTML/CSS/JS, you can run it with any static file server.

### Option 1: Python

```bash
python3 -m http.server 8080
```

Then open <http://localhost:8080>.

### Option 2: VS Code Live Server

Open the folder in VS Code and launch with the Live Server extension.

## Current project structure

```text
.
├── index.html
├── login.html
├── dashboard.html
├── lesson.html
├── styles.css
└── README.md
```

## Optimisation opportunities

Below are practical optimisations to improve performance, maintainability, accessibility, and reliability.

### 1) Performance and loading

1. **Move inline CSS/JS into versioned external files**
   - `login.html` and `lesson.html` contain inline styles/scripts.
   - Extracting these into `assets/css/*.css` and `assets/js/*.js` improves browser caching and keeps HTML lean.

2. **Defer non-critical scripts**
   - Add `defer` to external scripts where possible so parsing/rendering is not blocked.
   - Keep only critical bootstrap logic inline (if needed).

3. **Preconnect to third-party origins**
   - `login.html` loads Supabase from jsDelivr; adding `preconnect` hints can reduce connection setup latency.

4. **Add cache headers in deployment**
   - Serve HTML with short cache lifetimes and hashed static assets with long cache lifetimes.

### 2) Front-end architecture

1. **Unify page shell components**
   - Header/footer patterns are only present on the homepage.
   - Reuse a shared layout across dashboard/login/lesson for consistency.

2. **Create a small JS module structure**
   - Suggested split:
     - `assets/js/auth.js` (login/signup)
     - `assets/js/lesson.js` (lesson step flow)
     - `assets/js/ui.js` (shared helpers)

3. **Use shared utility CSS classes**
   - Dashboard and lesson currently use minimal/no shared styling.
   - Apply `styles.css` globally to reduce duplicated style definitions.

### 3) Accessibility improvements

1. **Use semantic form markup in `login.html`**
   - Wrap inputs in a `<form>` and submit via Enter key.
   - Add explicit `<label for="...">` associations.

2. **Improve heading and landmark structure**
   - Ensure each page has consistent `header`, `main`, and `footer` landmarks.

3. **Add focus styles and keyboard checks**
   - Confirm visible focus indicators on buttons/links and keyboard-only navigation.

### 4) Security and auth hardening

1. **Keep secrets server-side only**
   - Supabase publishable key is okay for client use, but never expose service role keys.

2. **Guard authenticated routes**
   - `dashboard.html` is directly accessible.
   - Add a session check and redirect unauthenticated users to `login.html`.

3. **Validate input and display safe messages**
   - Keep using `textContent` for user-visible messages (avoid `innerHTML` unless sanitized).

### 5) Quality and developer workflow

1. **Add formatting/linting**
   - Add Prettier + HTML/CSS linting for consistency.

2. **Add basic CI checks**
   - Run lint + link check + optional Lighthouse budget checks on pull requests.

3. **Add lightweight tests**
   - For static pages, Playwright can verify key flows:
     - Homepage loads
     - Login form validation message appears
     - Lesson “next step” flow works

## Recommended implementation order

1. Extract inline assets and defer scripts.
2. Add route/session guard for dashboard.
3. Improve login semantics/accessibility.
4. Add lint/format checks and CI.
5. Add Playwright smoke tests.

## Notes

This repo is a strong prototype foundation; most gains now come from **structure + consistency** rather than heavy framework migration.
