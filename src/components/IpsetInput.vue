<script setup>
import { defineProps } from "vue";

defineProps({
  name: Number,
});
</script>

<template>
  <input
    type="text"
    class="h-full w-full rounded-md border border-gray-300 p-1 text-center md:w-20"
    maxlength="3"
    :name="name"
  />
</template>
