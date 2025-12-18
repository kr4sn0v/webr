<template>
  <div v-show="modalStore.isVisible" class="fixed top-0 right-0 bottom-0 left-0 z-50">
    <div
      class="fixed top-0 right-0 bottom-0 left-0 bg-black opacity-50"
      @click="modalStore.close"
    ></div>
    <div class="relative top-50 mx-auto w-md rounded-md bg-white p-8">
      <p class="my-8 mt-4 text-center">{{ modalStore.message }}</p>
      <div class="flex justify-end gap-4">
        <button
          @click="modalStore.close"
          type="button"
          class="text-gray cursor-pointer rounded-md bg-gray-100 px-4 py-2 hover:bg-gray-200"
        >
          Отмена
        </button>
        <button
          @click="modalStore.confirm"
          type="button"
          class="cursor-pointer rounded-md bg-blue-500 px-4 py-2 text-white hover:bg-blue-700"
        >
          Да
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { useModalStore } from '@/stores/modal'

const modalStore = useModalStore()
</script>
