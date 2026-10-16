# miniml-ui

A minimal, zero-dependency Vue 3 component library built on the [Nord](https://www.nordtheme.com/) palette.

> **Status:** major version zero (0.y.z). The API can change at any time. `Button` is shipped; more components will follow.

## Philosophy

- **Tokens first.** Components derive their styles from a small set of foundational Nord tokens rather than dozens of independent variables per component.
- **Composition.** Accessible, unstyled primitives sit alongside polished default components, so you get ready-made UI and the freedom to build your own.
- **Shared motion.** Every component uses the same timing and easing: short, subtle opacity and transform transitions, with reduced-motion support built in.
- **Progressive complexity.** A simple API for simple cases; more control only when you need it.
- **The 100% Details Rule.** Every component ships with keyboard interaction, focus-visible states, screen-reader labels, relevant states (loading, disabled, error…), narrow-width behavior and reduced-motion support.
- **Zero dependencies.** No component or motion libraries. Focus management and ARIA are owned by the library, and native platform features (`<dialog>`, CSS transitions, `@starting-style`) come first.

The full palette, theme roles and requirements are in [DESIGN_GUIDE.md](./DESIGN_GUIDE.md).

## Example

```vue
<script setup lang="ts">
import { ref } from "vue";
import { Button } from "miniml-ui";

const saving = ref(false);
</script>

<template>
  <!-- Basic usage -->
  <Button>Save changes</Button>

  <!-- More control -->
  <Button variant="primary" :loading="saving" loading-label="Saving changes">
    Save changes
  </Button>
</template>
```

`Button` supports the `primary`, `secondary`, `ghost` and `danger` variants, the `sm`, `md` and `lg` sizes, and an `as` prop to render another element or component (for example `a` or `RouterLink`). While loading, it keeps focus and blocks clicks.

## Getting started

Requires Node 18+ and pnpm.

```sh
pnpm install
pnpm dev
```

The playground runs at [http://localhost:3000](http://localhost:3000).

## Repo structure

| Path                         | Package                     | Purpose                                      |
| ---------------------------- | --------------------------- | -------------------------------------------- |
| `packages/ui`                | `miniml-ui`                 | The component library: components and tokens |
| `apps/playground`            | `playground`                | Vite + Vue app for trying components locally |
| `packages/eslint-config`     | `@miniml/eslint-config`     | Shared ESLint config (Vue + Prettier)        |
| `packages/typescript-config` | `@miniml/typescript-config` | Shared `tsconfig.json` bases                 |

The monorepo is managed with [Turborepo](https://turborepo.dev/) and pnpm workspaces.

## Scripts

Run from the repo root:

| Command            | What it does                                  |
| ------------------ | --------------------------------------------- |
| `pnpm dev`         | Starts the playground                         |
| `pnpm lint`        | Lints all packages                            |
| `pnpm check-types` | Type-checks all packages with `vue-tsc`       |
| `pnpm format`      | Formats `.ts`, `.tsx`, `.vue` and `.md` files |

To run a task for one package, use a filter, for example `pnpm exec turbo dev --filter=playground`.

## Roadmap

- More components, built to the same standard as `Button`
- A shared, self-written roving-focus composable for menus and lists
- [Floating UI](https://floating-ui.com/) for popup positioning in Select, Menu and Tooltip, the one planned dependency
