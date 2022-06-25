<template>
  <section>
    <header
      class="
        bg-purple-600
        flex
        gap-5
        items-center
        justify-between
        p-4
        shadow-xl
      "
    >
      <h1 class="text-white font-semibold">
        React Cheatsheet
      </h1>
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Search by topic"
        class="
          bg-purple-800 bg-white
          focus:outline-none
          px-4
          py-2
          rounded-full
          sm:w-auto
          text-white
          w-full
        "
      />
    </header>
    <main class="p-4 flex gap-5">
      <div v-for="category in Object.keys(sheetCategories)" :key="category">
        <h3 class="text-xl mb-4">
          {{ category.toUpperCase() }}
        </h3>
        <Accordion
          v-for="title in sheetCategories[category]"
          :key="title"
          :title="title.split('-')[0]"
          :class="{
            'outline-solid-purple-700': matchQuery(`${category}-${title}`),
          }"
        >
          <keep-alive>
            <component :is="`${category}_${title}`" />
          </keep-alive>
        </Accordion>
      </div>
    </main>
  </section>
</template>

<script lang="ts">
import { defineComponent, defineAsyncComponent } from 'vue'

const sheets = []
const sheetCategories: {[key: string]: string[]} = {}
const components: {[key: string]: typeof defineAsyncComponent} = {}

const modules = import.meta.glob('../sheets/*.md')

for (const path in modules)
  sheets.push(path.replace('../sheets/', '').split('.')[0])

sheets.forEach((sheet) => {
  // Extract cheatsheet categories from filename
  const [category, title] = sheet.split('_')
  if (sheetCategories[category]) sheetCategories[category].push(title)
  else sheetCategories[category] = [title]

  // register components dynamically
  components[sheet] = defineAsyncComponent(
    () => import(`../sheets/${sheet}.md`),
  )
})

export default defineComponent({
  components,
  setup() {
    const searchQuery = ref('')
    const matchQuery = (title: string) => {
      if (!searchQuery.value.trim()) return false
      const titleStr = title.split('-').join(' ').toLowerCase()
      return searchQuery.value && titleStr.includes(searchQuery.value.toLowerCase())
    }
    return { sheetCategories, searchQuery, matchQuery }
  },
})
</script>
