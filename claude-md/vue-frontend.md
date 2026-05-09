# CLAUDE.md — Vue 3 Frontend Project

## Project

- **Name**: {project-name}
- **Stack**: Vue 3 + Vite + TypeScript + Pinia + Ant Design Vue
- **Node**: >= 18

## Code Style

- Use `<script setup lang="ts">` for all components
- Use Composition API exclusively — no Options API
- Use `defineProps` / `defineEmits` with TypeScript generics
- Prefer `ref()` over `reactive()` for primitive values
- Use Pinia stores with `defineStore` (setup syntax preferred)

## Naming Conventions

- Components: PascalCase (`UserProfile.vue`)
- Composables: camelCase with `use` prefix (`useFormSubmit.ts`)
- Stores: camelCase with `use` prefix and `Store` suffix (`useUserStore.ts`)
- API files: camelCase (`userApi.ts`)
- Types: PascalCase with descriptive suffix (`UserListResponse`, `CreateOrderParams`)

## File Structure

```
src/
├── api/          # API request functions (one file per domain)
├── assets/       # Static assets
├── components/   # Reusable components
├── composables/  # Composable hooks
├── layouts/      # Layout components
├── pages/        # Route pages (or views/)
├── router/       # Vue Router config
├── stores/       # Pinia stores
├── types/        # TypeScript type definitions
└── utils/        # Utility functions
```

## API Patterns

- All HTTP requests go through the centralized `request` wrapper
- API functions return typed responses
- Error handling is done in the request interceptor, not per-call

## Rules

- Do NOT use `any` type — use `unknown` or proper typing
- Do NOT use `console.log` in production code — use `console.warn` or `console.error` only for real issues
- Do NOT install new dependencies without asking first
- Always use the project's existing utilities before creating new ones
- Commit messages must be in Chinese
