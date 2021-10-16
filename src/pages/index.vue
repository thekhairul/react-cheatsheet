<template>
  <div>
    <div v-for="category in Object.keys(sheetCategories)" :key="category">
      {{ category }}
      <Accordion v-for="title in sheetCategories[category]" :key="title" :title="title">
        <keep-alive>
          <component :is="`${category}_${title}`" />
        </keep-alive>
      </Accordion>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, defineAsyncComponent } from 'vue'

const sheets = []
const sheetCategories = {}
const components = {}

const modules = import.meta.glob('../sheets/*.md')

for (const path in modules)
  sheets.push(path.replace('../sheets/', '').split('.')[0])

sheets.forEach((sheet) => {
  // Extract cheatsheet categories from filename
  const [category, title] = sheet.split('_')
  if (sheetCategories[category])
    sheetCategories[category].push(title)

  else
    sheetCategories[category] = [title]

  // register components dynamically
  components[sheet] = defineAsyncComponent(
    () => import(`../sheets/${sheet}.md`),
  )
})

export default defineComponent({
  components,
  setup() {
    return { sheetCategories }
  },
})
</script>
