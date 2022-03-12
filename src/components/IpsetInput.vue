<script setup>
import { ref, defineProps, defineEmits, defineExpose } from "vue";

const dom = ref(null);

defineProps({
  index: Number,
  modelValue: String,
});
defineEmits(["update:modelValue"]);
defineExpose({ dom });
</script>

<template>
  <div class="flex space-x-3">
    <input
      type="text"
      class="h-full w-full rounded-md border border-gray-300 p-1 text-center md:w-20"
      maxlength="3"
      :key="index"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      ref="dom"
    />
    <label v-if="index !== 4" class="self-end">.</label>
  </div>
</template>
