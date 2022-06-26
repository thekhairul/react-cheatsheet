<template>
  <div class="accordion max-w-96 rounded overflow-hidden shadow-md mb-2" :style="`max-height:${maxHeight}px`">
    <div
      ref="accordionTitleRef"
      class="
        flex
        justify-between
        items-center
        px-3
        py-2
        bg-white
        border-b
        cursor-pointer
      "
      @click="isOpen = !isOpen"
    >
      <span> {{ accordionTitle }} </span>
      <span class="text-2xl">{{ isOpen ? "-" : "+" }}</span>
    </div>

    <div ref="accordionBody" class="p-2 bg-white">
      <slot>
        Lorem ipsum, dolor sit amet consectetur adipisicing elit. Autem
        obcaecati cumque impedit? Labore velit pariatur eius quas aliquam
        corporis perspiciatis.
      </slot>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue'

export default defineComponent({
  props: {
    title: {
      type: String,
      default: 'Accordion Title',
    },
  },
  setup(props) {
    const isOpen = ref(false)
    const accordionTitleRef = ref(null)
    const accordionBody = ref(null)
    const maxHeight = ref(0)
    let titleHeight = 0
    watch(isOpen, () => {
      maxHeight.value = isOpen.value
        ? titleHeight + accordionBody.value.scrollHeight
        : titleHeight
    })
    onMounted(() => {
      titleHeight = accordionTitleRef.value.scrollHeight
      maxHeight.value = titleHeight
    })
    return {
      accordionTitle: props.title,
      isOpen,
      accordionTitleRef,
      accordionBody,
      maxHeight,
    }
  },
})
</script>

<style scoped>
.accordion {
  transition: max-height 0.3s ease-in-out;
  will-change: max-height;
}
</style>
