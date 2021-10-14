<template>
  <div class="accordion max-w-72 rounded overflow-hidden shadow-md" :style="`max-height:${maxHeight}px`">
    <div ref="accordionTitle" class="p-2 bg-white border-b cursor-pointer" @click="isOpen = !isOpen">
      <slot name="title">
        Accordion Title
      </slot>
    </div>

    <div ref="accordionBody" class="p-2 bg-white">
      <slot name="body">
        Lorem ipsum, dolor sit amet consectetur adipisicing elit. Autem obcaecati cumque impedit? Labore velit pariatur eius quas aliquam corporis perspiciatis.
      </slot>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue'

export default defineComponent({
  setup() {
    const isOpen = ref(false)
    const accordionTitle = ref(null)
    const accordionBody = ref(null)
    const maxHeight = ref(0)
    let titleHeight = 0
    let bodyHeight = 0
    watch(isOpen, async() => {
      maxHeight.value = isOpen.value ? titleHeight + bodyHeight : titleHeight
    })
    onMounted(() => {
      titleHeight = accordionTitle.value.scrollHeight
      bodyHeight = accordionBody.value.scrollHeight
      maxHeight.value = titleHeight
    })
    return { isOpen, accordionTitle, accordionBody, maxHeight }
  },
})
</script>

<style scoped>
.accordion {
  transition: max-height 0.3s ease-in-out;
  will-change: max-height;
}
</style>
