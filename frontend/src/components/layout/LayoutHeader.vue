<template>
  <LayoutContainer>
    <div class="flex items-center justify-between py-4">
      <div>
        <RouterLink to="/" class="hover:text-blue-500">
          <FontAwesomeIcon :icon="faCode" class="mr-2" />
          Блог веб-разработчика
        </RouterLink>
      </div>
      <div>
        <p>Веб-технологии</p>
        <p>Написание кода</p>
        <p>Разбор ошибок</p>
      </div>
      <div>
        <div class="mb-3">
          <RouterLink
            v-if="!userStore.isAuthorized"
            to="/login"
            class="rounded-md bg-blue-500 px-4 py-2 text-white hover:bg-blue-700"
            >Войти</RouterLink
          >
          <div v-else class="text-right">
            <span>{{ userStore.user.login }}</span>
            &nbsp;|&nbsp;
            <button class="cursor-pointer hover:text-blue-500" @click="hanleLogout">
              <FontAwesomeIcon :icon="faArrowRightFromBracket" />
            </button>
          </div>
        </div>
        <p>
          <a href="#" @click="$router.go(-1)" aria-label="Назад" class="hover:text-blue-500">
            <FontAwesomeIcon :icon="faBackward" />
          </a>
          &nbsp; &nbsp;
          <RouterLink
            v-if="userStore.isAuthorized"
            to="/post"
            aria-label="Новая статья"
            class="hover:text-blue-500"
          >
            <FontAwesomeIcon :icon="faFile" />
          </RouterLink>
          &nbsp; &nbsp;
          <RouterLink
            v-if="userStore.isAuthorized"
            to="/users"
            aria-label="Пользователи"
            class="hover:text-blue-500"
          >
            <FontAwesomeIcon :icon="faUsers" />
          </RouterLink>
        </p>
      </div>
    </div>
  </LayoutContainer>
</template>

<script setup>
import LayoutContainer from './LayoutContainer.vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import {
  faUsers,
  faFile,
  faBackward,
  faCode,
  faArrowRightFromBracket,
} from '@fortawesome/free-solid-svg-icons'
import { RouterLink } from 'vue-router'
import { useUserStore } from '@/stores/user'
import { useRoute, useRouter } from 'vue-router'

const userStore = useUserStore()
const route = useRoute()
const router = useRouter()

const hanleLogout = async () => {
  const response = await userStore.logout()

  if (!response.error && route.meta.requiredAuth) {
    router.push('/')
  }
}
</script>
