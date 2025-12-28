<template>
  <form class="ronded-ms mb-8 bg-white p-6 shadow-md" @submit.prevent="handleCommentSubmit">
    <LabelBase for="newComment">Ваш комментарий</LabelBase>
    <InputBase name="newComment" as="textarea" v-model="newComment" id="newComment"></InputBase>
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
import LabelBase from './base/LabelBase.vue'
import InputBase from './base/InputBase.vue'

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
