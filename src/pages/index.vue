<template>
  <div>
    <!-- TODO: implement listing into categories -->
    <Accordion v-for="sheet in cheatsheets" :key="sheet" :title="sheet">
      <keep-alive>
        <component :is="sheet" />
      </keep-alive>
    </Accordion>
  </div>
</template>

<script lang="ts">
import { defineComponent, defineAsyncComponent, ref } from "vue";

const sheets = [];
const components = {};

const modules = import.meta.glob("../sheets/*.md");

for (const path in modules)
  sheets.push(path.replace("../sheets/", "").split(".")[0]);

sheets.forEach((sheet) => {
  components[sheet] = defineAsyncComponent(
    () => import(`../sheets/${sheet}.md`)
  );
});

export default defineComponent({
  components,
  setup() {
    const cheatsheets = ref(sheets);
    return { cheatsheets };
  },
});
</script>
