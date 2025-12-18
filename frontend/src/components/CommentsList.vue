<template>
  <ul>
    <li
      v-for="comment in articleStore.article.comments"
      :key="comment.id"
      class="mb-4 rounded-md bg-white p-4 shadow-md"
    >
      <div class="mb-4 flex justify-between">
        <p class="font-bold">
          <FontAwesomeIcon :icon="faUser" />
          &nbsp; {{ comment.author }}
        </p>
        <button
          v-if="userStore.isAdmin || userStore.isModerator"
          type="button"
          class="cursor-pointer text-red-500 hover:text-red-700"
          @click="handleDeleteComment(comment.id)"
        >
          <FontAwesomeIcon :icon="faTrash" />
          &nbsp; Удалить
        </button>
      </div>
      <p class="mb-2 text-sm text-gray-500">
        <time :datetime="comment.publishedAt">{{
          formatDate(comment.publishedAt, props.dateOptions)
        }}</time>
      </p>
      <div class="mb-2 whitespace-pre-wrap">{{ comment.content }}</div>
    </li>
  </ul>
  <MessageBoxBase v-if="errorMessage" type="error">{{ errorMessage }}</MessageBoxBase>
</template>

<script setup>
import { ref } from 'vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faTrash, faUser } from '@fortawesome/free-solid-svg-icons'
import { useArticleStore } from '@/stores/article'
import { useUserStore } from '@/stores/user'
import { useModalStore } from '@/stores/modal'
import { formatDate } from '@/utils/dateFormatters'
import MessageBoxBase from './base/MessageBoxBase.vue'

const articleStore = useArticleStore()
const userStore = useUserStore()
const modalStore = useModalStore()
const errorMessage = ref('')

const props = defineProps({
  dateOptions: {
    type: Object,
    required: false,
  },
})

const handleDeleteComment = (commentId) => {
  errorMessage.value = ''
  modalStore.open('Удалить комментарий?', async () => {
    const response = await articleStore.deleteComment(commentId)
    if (response.error) {
      errorMessage.value = response.error
    }
  })
}
</script>
