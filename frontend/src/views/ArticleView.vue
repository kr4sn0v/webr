<template>
  <LayoutContainer class="mt-4">
    <AricleDetailsForm v-if="articleStore.isInEditMode" />
    <ArticleDetails v-else :date-options="formatDateOptions" />
    <div
      v-if="
        !articleStore.isInEditMode &&
        (articleStore.article?.comments?.length > 0 || userStore.isAuthorized)
      "
    >
      <h2 class="mb-4 text-2xl font-bold">Комментарии</h2>
      <CommentsList :date-options="formatDateOptions" />
      <CommentsForm v-if="userStore.isAuthorized" />
    </div>
  </LayoutContainer>
</template>

<script setup>
import LayoutContainer from '@/components/layout/LayoutContainer.vue'
import ArticleDetails from '@/components/ArticleDetails.vue'
import CommentsList from '@/components/CommentsList.vue'
import CommentsForm from '@/components/CommentsForm.vue'
import AricleDetailsForm from '@/components/AricleDetailsForm.vue'
import { onBeforeMount } from 'vue'
import { useArticleStore } from '@/stores/article'
import { useUserStore } from '@/stores/user'

const articleStore = useArticleStore()
const userStore = useUserStore()

const props = defineProps({
  id: {
    type: String,
    required: true,
  },
})

const formatDateOptions = {
  day: 'numeric',
  month: 'long',
  year: 'numeric',
  hour: 'numeric',
  minute: 'numeric',
}

onBeforeMount(async () => {
  try {
    await articleStore.fetchArticle(props.id)
  } catch (error) {
    console.error(error)
  }
})
</script>
