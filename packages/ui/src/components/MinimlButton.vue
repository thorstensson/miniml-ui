<script setup lang="ts">
import { computed, type Component } from "vue";

export interface ButtonProps {
  /** Visual style. */
  variant?: "primary" | "secondary" | "ghost" | "danger";
  size?: "sm" | "md" | "lg";
  /** Native button type. Ignored when rendering another element. */
  type?: "button" | "submit" | "reset";
  disabled?: boolean;
  /** Shows a spinner, keeps focus and blocks clicks. */
  loading?: boolean;
  /** Accessible name while loading. */
  loadingLabel?: string;
  /** Element or component to render, e.g. "a" or RouterLink. */
  as?: string | Component;
}

defineOptions({ name: "MinimlButton" });

const props = withDefaults(defineProps<ButtonProps>(), {
  variant: "secondary",
  size: "md",
  type: "button",
  disabled: false,
  loading: false,
  loadingLabel: "Loading",
  as: "button",
});

const emit = defineEmits<{ click: [event: MouseEvent] }>();

const isNativeButton = computed(() => props.as === "button");
const isInert = computed(() => props.disabled || props.loading);

const classes = computed(() => [
  "mm-button",
  `mm-button--${props.variant}`,
  `mm-button--${props.size}`,
  { "mm-button--loading": props.loading },
]);

// A native disabled button leaves the tab order. While loading we use
// aria-disabled instead, so focus stays where the user left it.
const stateAttrs = computed(() =>
  isNativeButton.value
    ? {
        type: props.type,
        disabled: props.disabled || undefined,
        "aria-disabled": props.loading || undefined,
      }
    : {
        "aria-disabled": isInert.value || undefined,
        tabindex: props.disabled ? -1 : undefined,
      },
);

function onClick(event: MouseEvent) {
  if (isInert.value) {
    event.preventDefault();
    event.stopImmediatePropagation();
    return;
  }
  emit("click", event);
}
</script>

<template>
  <component
    :is="as"
    :class="classes"
    :aria-busy="loading || undefined"
    v-bind="stateAttrs"
    @click="onClick"
  >
    <span v-if="loading" class="mm-button__spinner" aria-hidden="true" />
    <span class="mm-button__label"><slot /></span>
    <span v-if="loading" class="mm-visually-hidden">{{ loadingLabel }}</span>
  </component>
</template>

<style>
/* :where() keeps specificity at zero, so a single app class overrides it */
:where(.mm-button) {
  /* <a> defaults to content-box, which would add padding on top of min-height */
  box-sizing: border-box;
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--mm-space-2);
  max-width: 100%;
  min-height: 2.25rem;
  padding: var(--mm-space-2) var(--mm-space-4);
  border: var(--mm-border-width) solid transparent;
  border-radius: var(--mm-radius-md);
  font-family: var(--mm-font-sans);
  font-size: var(--mm-font-size-md);
  font-weight: var(--mm-font-weight-medium);
  line-height: var(--mm-line-height-tight);
  text-align: center;
  text-decoration: none;
  cursor: pointer;
  user-select: none;
  -webkit-tap-highlight-color: transparent;
  transition:
    background-color var(--mm-duration-fast) var(--mm-ease-out),
    border-color var(--mm-duration-fast) var(--mm-ease-out),
    color var(--mm-duration-fast) var(--mm-ease-out),
    transform var(--mm-duration-fast) var(--mm-ease-out);
}

:where(.mm-button:focus-visible) {
  outline: var(--mm-focus-ring-width) solid var(--mm-focus-ring);
  outline-offset: var(--mm-focus-ring-offset);
}

/* Sizes. Padding rather than fixed height, so long labels can wrap */
:where(.mm-button--sm) {
  min-height: 2rem;
  padding: var(--mm-space-1) var(--mm-space-3);
  font-size: var(--mm-font-size-sm);
  border-radius: var(--mm-radius-sm);
}

:where(.mm-button--lg) {
  min-height: 2.75rem;
  padding: var(--mm-space-3) var(--mm-space-5);
  font-size: var(--mm-font-size-lg);
}

/* Variants */
:where(.mm-button--primary) {
  background-color: var(--mm-accent);
  color: var(--mm-on-accent);
}

:where(.mm-button--secondary) {
  background-color: var(--mm-surface);
  border-color: var(--mm-border);
  color: var(--mm-text);
}

:where(.mm-button--ghost) {
  background-color: transparent;
  color: var(--mm-text);
}

:where(.mm-button--danger) {
  background-color: var(--mm-danger-solid);
  color: var(--mm-on-danger);
}

/* Hover only on devices that really hover, so touch doesn't stick */
@media (hover: hover) {
  :where(.mm-button--primary:hover:not(:disabled, [aria-disabled="true"])) {
    background-color: color-mix(in oklab, var(--mm-accent) 85%, var(--mm-text));
  }

  :where(.mm-button--secondary:hover:not(:disabled, [aria-disabled="true"])) {
    background-color: var(--mm-surface-raised);
  }

  :where(.mm-button--ghost:hover:not(:disabled, [aria-disabled="true"])) {
    background-color: var(--mm-surface);
  }

  :where(.mm-button--danger:hover:not(:disabled, [aria-disabled="true"])) {
    background-color: var(--mm-danger-solid-hover);
  }
}

:where(.mm-button:active:not(:disabled, [aria-disabled="true"])) {
  transform: scale(var(--mm-press-scale));
}

/* States */
:where(.mm-button:disabled, .mm-button[aria-disabled="true"]) {
  cursor: not-allowed;
}

:where(.mm-button:disabled, .mm-button[aria-disabled="true"]:not(.mm-button--loading)) {
  opacity: 0.5;
}

:where(.mm-button--loading) {
  cursor: progress;
}

/* Hide the label but keep its width, so the button doesn't jump */
:where(.mm-button--loading .mm-button__label) {
  visibility: hidden;
}

:where(.mm-button__spinner) {
  position: absolute;
  inset: 0;
  width: 1em;
  height: 1em;
  margin: auto;
  border: 2px solid currentColor;
  border-right-color: transparent;
  border-radius: 50%;
  animation: mm-spin 700ms linear infinite;
}

/* The spinner still has to show progress, so slow it rather than stop it */
@media (prefers-reduced-motion: reduce) {
  :where(.mm-button__spinner) {
    animation-duration: 1.6s;
  }
}

@keyframes mm-spin {
  to {
    transform: rotate(360deg);
  }
}
</style>
