<script setup>
import IpsetInput from "./components/IpsetInput.vue";
import { reactive, ref, watch } from "vue";

const refs = ref([]);

function Validate(val) {
  if (isNaN(Number(val)) || Number(val) > 255 || val === "") return true;
}

const isError = ref(false);

function Submit() {
  isError.value = false;

  refs.value.map((item) => {
    if (Validate(item.dom.value)) {
      isError.value = true;
    }
  });
}

const ip = reactive({
  ip1: "",
  ip2: "",
  ip3: "",
  ip4: "",
});

watch(
  () => ip.ip1,
  () => {
    if (ip.ip1.length >= 3) {
      ip.ip2 = ip.ip1.substring(3);
      ip.ip1 = ip.ip1.substring(0, 3);
      refs.value[2].dom.focus();
    }
  }
);

watch(
  () => ip.ip2,
  () => {
    if (ip.ip2.length >= 3) {
      ip.ip3 = ip.ip2.substring(3);
      ip.ip2 = ip.ip2.substring(0, 3);
      refs.value[3].dom.focus();
    }

    if (ip.ip2.length === 0 && ip.ip1.length > 0) {
      refs.value[1].dom.focus();
    }
  }
);

watch(
  () => ip.ip3,
  () => {
    if (ip.ip3.length >= 3) {
      ip.ip4 = ip.ip3.substring(3);
      ip.ip3 = ip.ip3.substring(0, 3);
      refs.value[4].dom.focus();
    }

    if (ip.ip3.length === 0 && ip.ip2.length > 0) {
      refs.value[2].dom.focus();
    }
  }
);

watch(
  () => ip.ip4,
  () => {
    if (ip.ip4.length === 0 && ip.ip3.length > 0) {
      refs.value[3].dom.focus();
    }
  }
);
</script>

<template>
  <main class="flex h-screen w-screen items-center justify-center bg-slate-500">
    <div
      class="min-w-1/6 mx-8 flex h-[152px] flex-col space-y-5 rounded-md bg-white p-4 shadow-md"
    >
      <h2>IP input</h2>
      <form class="flex space-x-3" @submit.prevent="Submit">
        <IpsetInput
          v-model="ip['ip' + index]"
          v-for="index in 4"
          :index="index"
          :key="index"
          :ref="(el) => (refs[index] = el)"
        />

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
