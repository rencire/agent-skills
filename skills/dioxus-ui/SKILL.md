---
name: dioxus-ui
description: Use for Dioxus component structure, rsx composition, signals and state patterns, document::Style usage, and Dioxus-specific UI styling decisions.
---

# Dioxus UI

Use this skill for Dioxus-specific implementation details.

## Component Shape

- Keep `main.rs` focused on startup and top-level app composition.
- Move screen-sized `rsx!` blocks into page or feature modules.
- Keep shared view pieces in reusable components.
- Prefer clear component boundaries over one giant component with many branches.

## Styling

- Use a dedicated stylesheet or styling module for broad app styling.
- Avoid growing inline `document::Style` blocks inside the app shell.
- Keep page-local styling near the page when it truly belongs there.
- If styling changes are the main reason a Dioxus file is growing, move the styles out.

## State and Composition

- Keep state close to the component that owns the behavior.
- Pass data down explicitly instead of making one root component know everything.
- If a component starts coordinating multiple unrelated UI areas, split it.

## Working Rule

If a Dioxus change touches both layout and styling in a large file, prefer extracting the layout or stylesheet at the same time.
