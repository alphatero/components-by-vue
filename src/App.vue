<script setup>
import IpsetInput from "./components/IpsetInput.vue";
import { ref } from "vue";

function isValidateIP(val) {
  return /(\b25[0-5]|\b2[0-4][0-9]|\b[01]?[0-9][0-9]?)(\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3}/.test(
    val
  );
}

const isError = ref(false);

function toFormData(form) {
  return Object.fromEntries(new FormData(form).entries());
}

function onSubmit(e) {
  const ip = Array.from({ ...toFormData(e.target), length: 4 }).join(".");

  isError.value = !isValidateIP(ip);
}

function onChange(e) {
  const { value, maxLength, name } = e.target;

  if (value.length !== maxLength && value.length !== 0) return;

  const form = e.target.closest("form");
  const current = Number(name);

  if (value.length === 0) {
    if (current - 1 >= 0)
      form.querySelector(`input[name="${current - 1}"]`)?.focus();
    return;
  }
  form.querySelector(`input[name="${current + 1}"]`)?.focus();
}
</script>

<template>
  <main class="flex h-screen w-screen items-center justify-center bg-slate-500">
    <div
      class="min-w-1/6 mx-8 flex h-[152px] flex-col space-y-5 rounded-md bg-white p-4 shadow-md"
    >
      <h2>IP input</h2>
      <form class="flex space-x-3" @submit.prevent="onSubmit" @input="onChange">
        <div class="flex space-x-3" v-for="index in 4" :key="index">
          <IpsetInput :name="index - 1" />

          <label v-if="index !== 4" class="self-end">.</label>
        </div>

        <div>
          <button
            :class="[
              'rounded-md  px-3 py-1',
              'bg-green-500 text-white',
              'hover:bg-green-700',
              'transition-colors duration-300',
            ]"
          >
            Submit
          </button>
        </div>
      </form>
      <strong class="text-red-500" v-if="isError"
        >Format is not correct!!</strong
      >
    </div>
  </main>
</template>
