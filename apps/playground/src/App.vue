<script setup lang="ts">
import { ref, watchEffect } from "vue";
import { Button, type ButtonProps } from "miniml-ui";

type Theme = "system" | "light" | "dark";

const themes: Theme[] = ["system", "light", "dark"];
const theme = ref<Theme>("system");

watchEffect(() => {
  const root = document.documentElement;
  if (theme.value === "system") delete root.dataset.theme;
  else root.dataset.theme = theme.value;
});

const variants: NonNullable<ButtonProps["variant"]>[] = [
  "primary",
  "secondary",
  "ghost",
  "danger",
];
const sizes: NonNullable<ButtonProps["size"]>[] = ["sm", "md", "lg"];

const saving = ref(false);

function save() {
  saving.value = true;
  setTimeout(() => (saving.value = false), 2000);
}

const clicks = ref(0);
</script>

<template>
  <main class="playground">
    <header class="header">
      <h1>miniml-ui playground</h1>

      <div class="theme" role="group" aria-label="Theme">
        <Button
          v-for="t in themes"
          :key="t"
          size="sm"
          :variant="theme === t ? 'secondary' : 'ghost'"
          :aria-pressed="theme === t"
          @click="theme = t"
        >
          {{ t }}
        </Button>
      </div>
    </header>

    <section>
      <h2>Variants</h2>
      <div class="row">
        <Button v-for="v in variants" :key="v" :variant="v" @click="clicks++">
          {{ v }}
        </Button>
      </div>
      <p class="note">Clicks: {{ clicks }}</p>
    </section>

    <section>
      <h2>Sizes</h2>
      <div v-for="v in variants" :key="v" class="row">
        <Button v-for="s in sizes" :key="s" :variant="v" :size="s">
          {{ v }} {{ s }}
        </Button>
      </div>
    </section>

    <section>
      <h2>Loading</h2>
      <div class="row">
        <Button
          variant="primary"
          :loading="saving"
          loading-label="Saving changes"
          @click="save"
        >
          Save changes
        </Button>
        <Button loading>Always loading</Button>
        <Button variant="danger" loading>Deleting</Button>
      </div>
      <p class="note">
        Click "Save changes": it keeps focus and ignores clicks for 2 seconds.
      </p>
    </section>

    <section>
      <h2>Disabled</h2>
      <div class="row">
        <Button v-for="v in variants" :key="v" :variant="v" disabled>
          {{ v }}
        </Button>
      </div>
    </section>

    <section>
      <h2>As a link</h2>
      <div class="row">
        <Button as="a" href="https://www.nordtheme.com/" target="_blank">
          Nord docs
        </Button>
        <Button as="a" href="https://www.nordtheme.com/" disabled>
          Disabled link
        </Button>
      </div>
    </section>

    <section>
      <h2>Narrow width</h2>
      <div class="narrow">
        <Button variant="primary">A long label that has to wrap onto lines</Button>
      </div>
    </section>
  </main>
</template>

<style scoped>
.playground {
  max-width: 56rem;
  margin: 0 auto;
  padding: var(--mm-space-5) var(--mm-space-4);
}

.header {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
  gap: var(--mm-space-3);
}

h1 {
  font-size: 1.25rem;
  margin: 0;
}

h2 {
  font-size: var(--mm-font-size-md);
  color: var(--mm-text-muted);
  font-weight: var(--mm-font-weight-medium);
  margin: var(--mm-space-5) 0 var(--mm-space-3);
}

section {
  border-top: var(--mm-border-width) solid var(--mm-border);
  margin-top: var(--mm-space-5);
}

.theme {
  display: flex;
  gap: var(--mm-space-1);
}

.row {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: var(--mm-space-3);
  margin-bottom: var(--mm-space-3);
}

.note {
  font-size: var(--mm-font-size-sm);
  color: var(--mm-text-muted);
}

.narrow {
  width: 10rem;
  padding: var(--mm-space-3);
  border: var(--mm-border-width) dashed var(--mm-border);
  border-radius: var(--mm-radius-md);
}
</style>
