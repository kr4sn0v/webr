<template>
  <LayoutContainer>
    <h1 class="my-10 text-center text-3xl font-bold">Пользователи</h1>

    <div class="mb-10 rounded-md bg-white p-8 shadow-md">
      <MessageBoxBase v-if="errorMessage" type="error">{{ errorMessage }}</MessageBoxBase>

      <table v-if="users.length > 0" class="min-w-full table-fixed">
        <thead>
          <tr>
            <th class="p-2">Логин</th>
            <th class="p-2">Дата регистрации</th>
            <th class="p-2">Роль</th>
            <th class="p-2"></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in users" :key="user.id">
            <td class="p-2 text-center">{{ user.login }}</td>
            <td class="p-2 text-center">
              {{
                formatDate(user.registeredAt, {
                  day: 'numeric',
                  month: 'numeric',
                  year: 'numeric',
                })
              }}
            </td>
            <td class="p-2">
              <form class="relative flex gap-2" @submit.prevent="handleUserRole(user)">
                <select
                  :name="`user-${user.id}-role`"
                  v-model="user.roleId"
                  class="w-full rounded-md border border-gray-300 p-2"
                >
                  <option
                    v-for="role in rolesStore.roles"
                    :key="`${user.id}-${role.id}`"
                    :value="role.id"
                  >
                    {{ role.name }}
                  </option>
                </select>
                <button
                  type="submit"
                  class="cursor-pointer rounded-md bg-blue-500 px-3 py-1 text-white hover:bg-blue-700"
                >
                  <FontAwesomeIcon :icon="faFloppyDisk" />
                </button>
                <MessageBoxBase
                  class="absolute left-full ml-2"
                  v-if="userMessage[user.id]"
                  :type="userMessageType[user.id]"
                  >{{ userMessage[user.id] }}</MessageBoxBase
                >
              </form>
            </td>
            <td class="p-2 text-right">
              <button
                class="cursor-pointer text-red-500 hover:text-red-700"
                @click="handleUserDelete(user.id)"
              >
                <FontAwesomeIcon :icon="faTrash" />
                &nbsp; Удалить
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </LayoutContainer>
</template>

<script setup>
import LayoutContainer from '@/components/layout/LayoutContainer.vue'
import MessageBoxBase from '@/components/base/MessageBoxBase.vue'

import { onBeforeMount, ref } from 'vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faFloppyDisk, faTrash } from '@fortawesome/free-solid-svg-icons'

import { useUsersStore } from '@/stores/users'
import { useRolesStore } from '@/stores/roles'

import { formatDate } from '@/utils/dateFormatters'

const users = ref([])
const userMessage = ref({})
const userMessageType = ref({})

const usersStore = useUsersStore()
const rolesStore = useRolesStore()

const errorMessage = ref('')

onBeforeMount(async () => {
  Promise.all([rolesStore.fetchRoles(), usersStore.fetchUsers()]).then(
    ([rolesResponse, usersResponse]) => {
      if (rolesResponse.error || usersResponse.error) {
        console.error(rolesResponse.error || usersResponse.error)
        errorMessage.value = 'Ошибка загрузки данных'
      } else {
        rolesStore.roles = rolesResponse.data
        usersStore.users = JSON.parse(JSON.stringify(usersResponse.data))
        users.value = JSON.parse(JSON.stringify(usersResponse.data))
      }
    },
  )
})

const handleUserDelete = async (userId) => {
  const response = await usersStore.deleteUser(userId)

  if (response.error) {
    console.error(response.error)
    errorMessage.value = response.error
  } else {
    usersStore.users = usersStore.users.filter((user) => user.id !== userId)
    users.value = users.value.filter((user) => user.id !== userId)
  }
}

const handleUserRole = async (user) => {
  const storedUser = usersStore.users.find((u) => u.id === user.id)

  if (user.roleId === storedUser.roleId) {
    return
  }

  const response = usersStore.changeUserRole(user.id, user.roleId)

  if (response.error) {
    console.error(response.error)
    errorMessage.value = response.error
    user.roleId = storedUser.roleId
  } else {
    userMessage.value[user.id] = 'Обновлено'
    userMessageType.value[user.id] = 'success'
    storedUser.roleId = user.roleId

    setTimeout(() => {
      userMessage.value[user.id] = ''
      userMessageType.value[user.id] = ''
    }, 3000)
  }
}
</script>
