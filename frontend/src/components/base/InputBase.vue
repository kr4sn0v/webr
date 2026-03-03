<template>
  <Field
    v-if="$route.path === '/users'"
    class="w-full rounded-md border border-gray-300 bg-white p-2"
    ><slot></slot
  ></Field>
  <Field v-else class="w-full rounded-md border border-gray-300 bg-white p-2"></Field>
</template>

<script setup>
import { Field } from 'vee-validate'
</script>
