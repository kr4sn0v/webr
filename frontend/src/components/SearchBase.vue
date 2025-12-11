<template>
  <form @submit.prevent="handerSearch" class="relative mt-12">
    <input
      class="w-full rounded-md border border-gray-300 bg-white p-2 pr-20"
      type="text"
      name="search"
      placeholder="Поиск по блогу"
      v-model="searchQuery"
    />
    <button
      type="submit"
      class="absolute top-0 right-0 cursor-pointer rounded-md bg-blue-500 px-5 py-2 text-white hover:bg-blue-700"
    >
      <FontAwesomeIcon :icon="faMagnifyingGlass" />
    </button>
  </form>
</template>

<script setup>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faMagnifyingGlass } from '@fortawesome/free-solid-svg-icons'
import { ref, watch } from 'vue'

const searchQuery = ref('')

const props = defineProps({
  onSearch: {
    type: Function,
    required: true,
  },
})

const handerSearch = () => {
  props.onSearch(searchQuery.value)
}

let debouncedTimer

const debouncedSearch = (query) => {
  clearTimeout(debouncedTimer)

  debouncedTimer = setTimeout(() => {
    props.onSearch(query)
  }, 2000)
}

watch(searchQuery, (newQuery) => {
  debouncedSearch(newQuery)
})
</script>
