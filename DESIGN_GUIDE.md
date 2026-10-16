# Philosophy

> A living document. It will change while the project is in major version zero (0.y.z).

## Color Palette

Components derive their styles from these foundational tokens rather than having dozens of independent variables per component.

miniml-ui uses the [Nord](https://www.nordtheme.com/) palette: sixteen dimmed pastel colors that are easy on the eye but still colorful. Nord was made for clear, uncluttered and elegant designs in a minimal, flat style.

The colors are numbered `nord0` to `nord15` and grouped into four palettes. Each palette has a different role in dark and light themes.

The guidance below is a set of soft recommendations that describe Nord's original intent. Colors can be used in other ways or with different shading where a component needs it.

| Palette     | Colors            | Role                                                            |
| ----------- | ----------------- | --------------------------------------------------------------- |
| Polar Night | `nord0`–`nord3`   | Dark base colors: backgrounds (dark theme), text (light theme)  |
| Snow Storm  | `nord4`–`nord6`   | Light base colors: text (dark theme), backgrounds (light theme) |
| Frost       | `nord7`–`nord10`  | Bluish accents: primary UI and highlights                       |
| Aurora      | `nord11`–`nord15` | Colorful states: error, warning, success and more               |

---

### Polar Night

Four dark colors, used for base elements such as backgrounds or text.

| Token   | Hex       | Dark theme                                                                                                                         | Light theme                                                                        |
| ------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| `nord0` | `#2e3440` | Background and area color                                                                                                          | Plain text, caret, structural characters; occasionally borders for higher contrast |
| `nord1` | `#3b4252` | Elevated elements: panels, modals, popups, status bars, form controls (buttons, inputs, checkboxes); also borders and drop shadows | Subtle, secondary text; hover/active/focus text states                             |
| `nord2` | `#434c5e` | Active line, selection and text highlight                                                                                          | A lighter alternative to `nord1` for the same elements                             |
| `nord3` | `#4c566a` | Guides and markers (indent, wrap); comments and invisible characters in code                                                       | The most subtle, secondary text                                                    |

### Snow Storm

Three light colors, used for text or base UI elements.

There are two ways to shade with it:

- **Bright to dark (recommended):** `nord6` as the base, with `nord5` and `nord4` for highlights. The table below follows this style.
- **Dark to bright (semi-light):** `nord4` as the base, with `nord5` and `nord6` for highlights. Read the table in reverse order.

| Token   | Hex       | Dark theme                                              | Light theme                                                                                          |
| ------- | --------- | ------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| `nord4` | `#d8dee9` | Caret; variables, constants and attributes in code      | Elevated elements: panels, modals, popups, status bars, form controls; also borders and drop shadows |
| `nord5` | `#e5e9f0` | Subtle, secondary text; hover/active/focus text states  | Active line, selection and text highlight                                                            |
| `nord6` | `#eceff4` | Prominent text that needs attention; plain text in code | Background and area color                                                                            |

### Frost

The heart of Nord: four bluish colors for primary UI components, text highlights and key code elements. They are used the same way in dark and light themes.

| Token    | Hex       | Character                               | Use                                                                                    |
| -------- | --------- | --------------------------------------- | -------------------------------------------------------------------------------------- |
| `nord7`  | `#8fbcbb` | Calm, high contrast, frozen polar water | Elements that should stand out alongside the primary accent; classes and types in code |
| `nord8`  | `#88c0d0` | Bright, clear ice: **primary accent**   | Primary UI elements that need the most attention; functions in code                    |
| `nord9`  | `#81a1c1` | Darker, less saturated arctic water     | Secondary UI elements; keywords, operators and tags in code                            |
| `nord10` | `#5e81ac` | Dark, intense deep arctic ocean         | Tertiary UI elements; pragmas and pre-processor statements in code                     |

### Aurora

Five colors inspired by the aurora borealis, mainly for states. They are used the same way in dark and light themes.

| Token    | Hex       | State             | Use                                                     |
| -------- | --------- | ----------------- | ------------------------------------------------------- |
| `nord11` | `#bf616a` | Error             | Error states, deletions                                 |
| `nord12` | `#d08770` | Danger / advanced | Rare; advanced or dangerous actions; decorators in code |
| `nord13` | `#ebcb8b` | Warning           | Warning states, modifications                           |
| `nord14` | `#a3be8c` | Success           | Success states, additions; strings in code              |
| `nord15` | `#b48ead` | Uncommon          | Rare; uncommon functionality; numbers in code           |

---

## Composition

Composition is a first-class feature, rather than every component being a closed, fully styled object. Accessible, unstyled primitives are paired with polished default components, so users get ready-made UI and the freedom to build their own.

```vue
<DialogRoot v-model:open="open">
  <DialogTrigger>Delete project</DialogTrigger>

  <DialogContent>
    <DialogTitle>Delete this project?</DialogTitle>
    <DialogDescription>This action cannot be undone.</DialogDescription>
    <DialogClose>Cancel</DialogClose>
  </DialogContent>
</DialogRoot>
```

## Motion

A shared motion token system: every component uses the same timing and easing curves.

| Principle     | Guideline                                                 |
| ------------- | --------------------------------------------------------- |
| Duration      | 100–160 ms for small interactions                         |
| Properties    | Subtle opacity and transform transitions                  |
| Accessibility | Reduced-motion support built in                           |
| Character     | No bouncing or spring effects unless they serve a purpose |

## Progressive Complexity

Components expose a simple API for simple use cases, and allow complexity only when it's needed.

**Basic usage**

```vue
<Button>Save changes</Button>
```

**More control**

```vue
<Button variant="primary" :loading="saving" loading-label="Saving changes">
  Save changes
</Button>
```

## The 100% Details Rule

Quality in the small things is part of the identity. Every component ships with:

| Area           | Requirement                                                       |
| -------------- | ----------------------------------------------------------------- |
| Keyboard       | Keyboard interaction and focus-visible states                     |
| Screen readers | Screen-reader labels and appropriate ARIA behavior                |
| States         | Loading, empty, error, disabled and success states where relevant |
| Layout         | Correct behavior at narrow widths                                 |
| Motion         | Reduced-motion support                                            |
