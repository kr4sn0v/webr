<template>
  <form class="ronded-ms mb-8 bg-white p-6 shadow-md" @submit.prevent="handleCommentSubmit">
    <label for="newComment" class="text-gay-700 block">Ваш комментарий</label>
    <textarea
      v-model="newComment"
      id="newComment"
      class="mb-4 w-full rounded-md border border-gray-300 p-2"
    ></textarea>
    <div class="flex justify-between">
      <p class="text-red-500" v-if="errorMessage">{{ errorMessage }}</p>
      <ButtonBase type="submit" class="ml-auto w-xs">Отправить</ButtonBase>
    </div>
  </form>
</template>

<script setup>
import ButtonBase from '@/components/base/ButtonBase.vue'
import { useArticleStore } from '@/stores/article'
import { ref } from 'vue'

const aricleStore = useArticleStore()

const newComment = ref('')
const errorMessage = ref('')

const handleCommentSubmit = async () => {
  errorMessage.value = ''

  if (!newComment.value.trim()) {
    errorMessage.value = 'Комментарий не может быть пустым'
    return
  }

  const response = await aricleStore.addComment(newComment.value)
  if (response.error) {
    errorMessage.value = response.error
  } else {
    newComment.value = ''
  }
}
</script>
