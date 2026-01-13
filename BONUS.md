# Bonus: The Story of a Modern Web Application

This is a high-level walkthrough of how a modern web app typically works end-to-end. It is not a strict blueprint, but a helpful mental model for junior and mid-level engineers.

## 1) Initial load and bundles

- The browser requests `index.html` and downloads CSS and JavaScript bundles.
- Build tools (Vite/Webpack) split and optimize code (tree-shaking, minification).
- Only critical code loads first; the rest is lazy-loaded when needed.

## 2) Rendering strategy

- For faster first paint and SEO, apps often use SSR or SSG (Next.js, Nuxt, Angular Universal).
- Hydration attaches interactivity after the initial HTML is delivered.

## 3) Auth check and routing

- The app checks if the user is authenticated on startup.
- Protected routes redirect to login when required.

## 4) Login and identity flow

- Credentials are verified by an auth server (OAuth 2.0 with PKCE is common).
- The main app never directly handles raw passwords.

## 5) Tokens, cookies, and security

- Tokens are safest in HttpOnly, Secure, SameSite cookies.
- CORS and CSP are configured to limit which origins and scripts are trusted.

## 6) Data fetching and async models

- API requests include auth headers or cookies via an interceptor.
- React/Vue often use Promises; Angular often uses Observables.
- The UI shows loading and error states while requests are in flight.

## 7) Real-time and error handling

- WebSockets or SSE enable live updates (notifications, chats, dashboards).
- Errors are caught with error boundaries or global error handlers and logged (e.g., Sentry).

## 8) State and UI updates

- Data is stored in state (Redux/Context, NgRx/Services, Pinia/Composables).
- When state changes, the UI re-renders automatically.

## 9) Layout, accessibility, and i18n

- Layout uses CSS Grid and Flexbox for responsive structure.
- Semantic HTML and ARIA improve accessibility.
- Internationalization handles locales, formatting, and RTL languages.

## 10) Performance and UX

- Caching (CDN + client) speeds up repeated visits.
- Virtualization renders only visible list items.
- Core Web Vitals guide performance budgets (LCP, INP, CLS).

## 11) Delivery and maintenance

- CI/CD runs tests and deploys after review.
- Monitoring and logs help track issues in production.
