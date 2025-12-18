<template>
  <LayoutContainer class="py-8">
    <h1 class="mb-4 text-center text-2xl font-bold">Новая статья</h1>

    <Form
      class="mx-auto rounded-md bg-white p-6 shadow-md"
      :validation-schema="schema"
      @submit="handleAddArticle"
    >
      <div class="mb-4">
        <LabelBase for="imageUrl">URL изображения</LabelBase>
        <InputBase type="url" name="imageUrl" id="imageUrl" />
        <InputErrorBase name="imageUrl" />
      </div>
      <div class="mb-4">
        <LabelBase for="title">Заголовок</LabelBase>
        <InputBase type="text" name="title" id="title" />
        <InputErrorBase name="title" />
      </div>
      <div class="mb-4">
        <LabelBase for="content">Содержимое</LabelBase>
        <InputBase as="textarea" name="content" id="content" rows="15" />
        <InputErrorBase name="content" />
      </div>

      <div class="flex items-center justify-between">
        <MessageBoxBase v-if="errorMessage" type="error">{{ errorMessage }}</MessageBoxBase>
        <ButtonBase class="ml-auto" type="submit">Создать</ButtonBase>
      </div>
    </Form>
  </LayoutContainer>
</template>

<script setup>
import LayoutContainer from '@/components/layout/LayoutContainer.vue'
import LabelBase from '@/components/base/LabelBase.vue'
import InputBase from '@/components/base/InputBase.vue'
import InputErrorBase from '@/components/base/InputErrorBase.vue'
import ButtonBase from '@/components/base/ButtonBase.vue'
import MessageBoxBase from '@/components/base/MessageBoxBase.vue'
import { Form } from 'vee-validate'
import { ref } from 'vue'
import * as yup from 'yup'
import { useArticleStore } from '@/stores/article'
import { useRouter } from 'vue-router'

const articleStore = useArticleStore()
const router = useRouter()

const schema = yup.object({
  imageUrl: yup.string().required('Поле обязательно').url('Введите корректный URL'),
  title: yup.string().required('Поле обязательно'),
  content: yup.string().required('Поле обязательно'),
})

const errorMessage = ref('')

const handleAddArticle = async (formData) => {
  errorMessage.value = ''
  const response = await articleStore.addArticle(formData)

  if (response.error) {
    errorMessage.value = response.error
  } else {
    router.push(`/post/${response.data.id}`)
  }
}
</script>
