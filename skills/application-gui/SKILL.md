---
name: application-gui
description: Use for application GUI architecture across desktop or web UIs, especially when screens, layout shell, shared widgets, and styling are starting to blend together. Keep shells thin and split by responsibility.
---

# Application GUI

Use this skill for GUI structure, not framework trivia.

## Default Shape

- Keep the app shell thin: boot, top-level routing, shared state, and global composition.
- Put page-level composition in page modules.
- Put reusable widgets in shared component modules.
- Keep styling in dedicated style modules or files when it stops being local to one component.
- Split by responsibility, not by arbitrary line count.

## Refactor Triggers

- One file owns navigation, state, layout, and style.
- A page needs helpers from distant parts of the same file.
- Multiple screens keep changing together because the module boundary is too broad.
- Styling edits are the main reason a UI file keeps growing.

## Working Rule

When a GUI file is already large and the change touches it again, extract one coherent slice during the same change instead of adding more surface area.

## Stop Conditions

- Don’t split if the module is still conceptually single-purpose.
- Don’t create `utils` or `misc` buckets for UI code.
- Don’t add indirection unless it makes local reasoning easier.
