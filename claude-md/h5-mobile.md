# CLAUDE.md — H5 Mobile App (WebView)

## Project

- **Name**: {project-name}
- **Stack**: Vue 3 + Vite + TypeScript
- **Runtime**: Android WebView (API 21+)
- **Design**: Mobile-first, single-column layout

## Mobile Constraints

- Screen width: 375px baseline, use `vw`/`vh` units or rem
- No hover states — touch only
- Consider virtual keyboard pushing up content
- Fixed-position elements need `safe-area-inset-*` handling
- Test with both portrait and landscape orientations

## Native Bridge

- Android native methods are accessed via `window.android.{methodName}()`
- Always wrap native calls in try-catch — the app may run in a browser without the bridge
- Check `typeof window.android !== 'undefined'` before calling native methods

```typescript
// Pattern for safe native calls
function callNative(method: string, ...args: any[]) {
  try {
    if (typeof window.android !== 'undefined' && window.android[method]) {
      return window.android[method](...args);
    }
  } catch (e) {
    console.warn(`Native method ${method} not available`);
  }
  return null;
}
```

## Data Storage

- Use IndexedDB for large data (logs, images, signatures)
- Use localStorage only for small config values (< 1KB per key)
- All IndexedDB operations must be async with proper error handling
- Implement offline-first: store locally, sync when online

## Rules

- Do NOT assume network connectivity — handle offline gracefully
- Do NOT use `alert()` / `confirm()` — use custom modal components
- Do NOT use `window.open()` — it may not work in WebView
- Keep bundle size minimal — users may have slow connections
- All images should be lazy-loaded
- Commit messages must be in Chinese
