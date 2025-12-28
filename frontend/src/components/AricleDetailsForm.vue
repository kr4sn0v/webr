<template>
  <form class="mb-8 rounded-md bg-white p-8 shadow-md" @submit.prevent="handleArticleUpdate">
    <div class="mb-4 flex items-center justify-between">
      <h1 class="text-3xl">Редактировать статью</h1>
      <div class="flex gap-4 text-xl">
        <button
          @click="articleStore.toggleEditMode"
          type="button"
          class="cursor-pointer hover:text-blue-500"
          aria-label="Отменить изменения"
        >
          <FontAwesomeIcon :icon="faArrowRotateLeft" />
        </button>
        <button
          type="submit"
          class="cursor-pointer hover:text-blue-500"
          aria-label="Сохранить статью"
        >
          <FontAwesomeIcon :icon="faFloppyDisk" />
        </button>
        <button
          type="button"
          class="cursor-pointer hover:text-red-500"
          aria-label="Удалить статью"
          @click="handleDeleteArticle"
        >
          <FontAwesomeIcon :icon="faTrash" />
        </button>
      </div>
    </div>

    <p class="mb-4">
      <InputBase
        name="imageUrl"
        type="text"
        v-model="editedArticle.imageUrl"
        placeholder="URL изображение"
      />
    </p>

    <p class="mb-4">
      <InputBase
        name="title"
        type="text"
        v-model="editedArticle.title"
        placeholder="Заголовок статьи"
      />
    </p>
    <p class="mb-4">
      <InputBase
        as="textarea"
        name="content"
        rows="15"
        v-model="editedArticle.content"
        placeholder="Содержание статьи"
      />
    </p>
  </form>
</template>

<script setup>
import { ref } from 'vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faTrash, faFloppyDisk, faArrowRotateLeft } from '@fortawesome/free-solid-svg-icons'
import { useArticleStore } from '@/stores/article'
import { useModalStore } from '@/stores/modal'
import { useRouter } from 'vue-router'
import InputBase from './base/InputBase.vue'

const articleStore = useArticleStore()
const modalStore = useModalStore()
const router = useRouter()
const editedArticle = ref({ ...articleStore.article })

const handleArticleUpdate = async () => {
  await articleStore.updateArticle(editedArticle.value)
}

const handleDeleteArticle = () => {
  modalStore.open('Удалить статью?', async () => {
    const response = await articleStore.deleteArticle()
    if (!response.error) {
      router.push('/')
    }
  })
}
</script>
