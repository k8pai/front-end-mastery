# SDE-2 Frontend Interview Preparation Checklist

> Derived from full mastery roadmap → compressed into high-signal interview domains

---

# Progress Legend

- [ ] Not Started
- [/] In Progress
- [x] Completed

---

# 1. Advanced JavaScript (Phase 2)

- [ ] Event loop (microtask vs macrotask)
- [ ] Execution context, call stack
- [ ] Closures (memory leaks)
- [ ] Prototype chain
- [ ] Async internals (Promises, async/await)
- [ ] Immutability vs mutation
- [ ] Garbage collection basics
- [ ] Implement:
  - [ ] Debounce / throttle
  - [ ] Promise.all polyfill
  - [ ] Deep clone
  - [ ] Event emitter

---

# 2. React + TypeScript (Phase 5, 6, 7)

- [ ] Reconciliation & rendering lifecycle
- [ ] Hooks deep dive (useEffect, useMemo, useCallback)
- [ ] Custom hooks architecture
- [ ] Controlled vs uncontrolled components
- [ ] TypeScript:
  - [ ] Generics in components
  - [ ] Utility types
  - [ ] Discriminated unions
  - [ ] Typing context + reducers

---

# 3. Frontend Design Patterns (Phase 6, 7, 12)

- [ ] Container vs Presentational
- [ ] Compound components
- [ ] Provider pattern
- [ ] Render props vs hooks
- [ ] Higher Order Components
- [ ] Headless UI pattern

---

# 4. State Management (Phase 7)

- [ ] Redux Toolkit
- [ ] Middleware (thunk basics)
- [ ] Normalized state
- [ ] Zustand (selectors, optimization)
- [ ] react-hook-form (uncontrolled model)
- [ ] Custom hooks for shared state

---

# 5. Browser Internals & Networking (Phase 0, 3, 9)

- [ ] Rendering pipeline (DOM → CSSOM → layout → paint)
- [ ] Reflow vs repaint
- [ ] HTTP1.1 vs HTTP2 vs HTTP3
- [ ] CORS
- [ ] Browser storage:
  - [ ] localStorage
  - [ ] sessionStorage
  - [ ] cookies
  - [ ] IndexedDB

---

# 6. CDN & Caching (Phase 3, 13)

- [ ] CDN architecture (edge vs origin)
- [ ] Cache-Control, ETag
- [ ] Asset versioning (hashing)
- [ ] Cache invalidation strategies
- [ ] Edge computing basics

---

# 7. Performance Engineering (Phase 3, 7, 13)

- [ ] Core Web Vitals (LCP, CLS, FID)
- [ ] Code splitting
- [ ] Lazy loading
- [ ] Virtualization
- [ ] Memoization (correct usage)
- [ ] Bundle optimization
- [ ] Preload / Prefetch

---

# 8. Security (Phase 14)

- [ ] XSS
- [ ] CSRF
- [ ] CSP
- [ ] Sanitization strategies
- [ ] Secure authentication handling

---

# 9. Testing (Phase 11)

- [ ] Unit testing (Jest)
- [ ] Component testing (RTL)
- [ ] Async testing
- [ ] Mocking APIs
- [ ] E2E basics (Cypress / Playwright)

---

# 10. Frontend System Design (Phase 12)

- [ ] Component architecture
- [ ] State architecture
- [ ] Design systems
- [ ] Folder structures
- [ ] Caching strategies
- [ ] Microfrontends

### Must Design:
- [ ] Form builder
- [ ] Dashboard
- [ ] Infinite scroll feed
- [ ] Workflow system

---

# 11. Error Handling & Resilience (Phase 6, 19)

- [ ] Error boundaries
- [ ] Retry strategies
- [ ] Graceful degradation
- [ ] Logging strategies

---

# 12. SSR / Hydration / Framework Concepts (Phase 3, 8)

- [ ] SSR vs CSR vs SSG
- [ ] Hydration issues
- [ ] Streaming SSR basics
- [ ] Server vs client components

---

# 13. Accessibility (Phase 1, 15)

- [ ] Semantic HTML
- [ ] ARIA roles
- [ ] Keyboard navigation
- [ ] Focus management
- [ ] WCAG basics

---

# 14. Tooling & Ecosystem (Phase 4, 8, 17)

- [ ] Bundlers (Vite, Webpack)
- [ ] Tree shaking
- [ ] ESLint + Prettier
- [ ] CI/CD basics
- [ ] Environment configs

---

# 15. Real-World Engineering & Behavioral (Phase 19, 20)

- [ ] Debugging strategy
- [ ] Refactoring patterns
- [ ] Code reviews
- [ ] Communication
- [ ] Ownership mindset
- [ ] Production issue handling

---

# How to Use This

For EACH topic:

1. Explain concept clearly
2. Write code from scratch
3. Discuss trade-offs
4. Solve real-world problems
5. Handle follow-ups

---

# Interview Readiness Signal

You are SDE-2 ready if you can:

- Connect topics across domains
- Justify decisions with trade-offs
- Think in systems, not components
- Optimize without over-engineering
