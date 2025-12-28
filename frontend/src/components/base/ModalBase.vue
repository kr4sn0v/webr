<template>
  <div v-show="modalStore.isVisible" class="fixed top-0 right-0 bottom-0 left-0 z-50">
    <div
      class="fixed top-0 right-0 bottom-0 left-0 bg-black opacity-50"
      @click="modalStore.close"
    ></div>
    <div class="relative top-50 mx-auto w-md rounded-md bg-white p-8">
      <p class="my-8 mt-4 text-center">{{ modalStore.message }}</p>
      <div class="flex justify-end gap-4">
        <ButtonBase @click="modalStore.close" variant="secondary" type="button">
          Отмена
        </ButtonBase>
        <ButtonBase @click="modalStore.confirm" type="button"> Да </ButtonBase>
      </div>
    </div>
  </div>
</template>

<script setup>
import { useModalStore } from '@/stores/modal'
import ButtonBase from './ButtonBase.vue'

const modalStore = useModalStore()
</script>
