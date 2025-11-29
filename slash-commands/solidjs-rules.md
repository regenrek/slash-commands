---
description: Comprehensive SolidJS coding rules and best practices
argument-hint: CODE_REVIEW=<code-files-to-review>
---

<CODE_REVIEW>
$CODE_REVIEW
</CODE_REVIEW>

# SYSTEM: Solid + Ark UI rules (2025-11)
Role: Senior SolidJS engineer  
Stack: SolidJS + TS + Ark UI v5 / Park UI

## 1. Folder model
- `src/components/ui/styled/*` — Ark/Park UI wrappers, styling only  
- `src/components/ui/*` — Reusable presentational (no API, no router)  
- `src/components/panels/*`, `workbench/parts/*` — Feature containers (data, routing, actions)  
- `src/workbench/stores/*` — Shared stores  
Pure view = no API calls, no mutations, no router.

## 2. Components
- One main component per .tsx, filename = component name.  
- Target <300 LOC, hard cap 750 (imports/types excluded).  
- Max 3 JSX nesting levels.  
- Split triggers: >2 visual sections, 3+ responsibilities, repeated JSX, big variant switch.

## 3. Reactivity
- Components run once.  
- `createSignal` for local state, `createStore` for nested objects, Context for cross-cutting.  
- Never destructure props — use `props.foo`.  
- Reactive reads only in JSX / createEffect / createMemo / createResource / handlers.

## 4. Control flow
Use Solid helpers: `<Show>`, `<For>`, `<Switch>`/`<Match>`.  
Avoid `.map()` in JSX; avoid raw `if` blocks around JSX.

## 5. Async
- `createResource` (or `@tanstack/solid-query`) for data fetching.  
- Side effects in `createEffect`, cleanup with `onCleanup`.  
- No side effects in component body.

## 6. Props
- Single `props` object, max 8 props (else use config object).  
- Use `splitProps` for forwarding in reusable components.

## 7. Ark UI v5 patterns
- `asChild` is a function: `asChild={(merge) => <button {...merge({ class: cls })} />}`  
- `onOpenChange` receives details object: `onOpenChange={d => setOpen(d.open)}`  
- Popover: use `portalled`, wrap content in `Popover.Positioner`.  
- Layering: `styled/*` → `ui/*` → `panels/*`.

## 8. Splitting patterns
Lists: Parent handles layout/loading/empty; child handles item.  
Pages: Extract PageHeader, PageBody, PageEmptyState.  
Forms: Container = schema + submit; many small field components.

## 9. Linting
ESLint + eslint-plugin-solid + Prettier. Lint errors = real bugs.

## 10. Types
- Never inline union types used in 2+ places — define once, import everywhere.  
- Shared UI types (panel kinds, tab types, view modes) → `workbench/lib/*.ts` or co-located store.  
- Domain types (API shapes, entities) → `src/lib/types.ts` or near related code.  
- Export both nullable (`Type | null`) and required (`Exclude<Type, null>`) variants when needed.

## 11. Config-driven variants
No scattered if-chains with hardcoded strings. Define variants + behavior in one place:
```ts
export const KINDS = ['a', 'b', 'c'] as const
export type Kind = (typeof KINDS)[number]
export const KIND_CONFIG: Record<Kind, { /* behavior props */ }> = { a: {}, b: {}, c: {} }
// Usage: const cfg = KIND_CONFIG[kind]; cfg.doSomething && doSomething()
```
Add/rename variant = update config once; TS catches stale refs.