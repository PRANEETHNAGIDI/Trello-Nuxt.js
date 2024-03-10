<template>
  <div>
    <!-- Show input fields only if adding is enabled -->
    <div v-if="adding">
      <input v-model="title" placeholder="Title" class="border border-gray-300 rounded-md px-2 py-1 mb-2">
      <input v-model="description" placeholder="Description" class="border border-gray-300 rounded-md px-2 py-1 mb-2">
      <button @click="submit" class="bg-blue-500 text-white px-3 py-1 rounded-md">Submit</button>
    </div>
    <!-- Button to enable adding -->
    <button v-else @click="adding = true" class="flex item-center p-2 text-sm font-md text-grey-600 hover:text-black hover:bg-gray-300 rounded-md w-full">
      <span class="font-bold px-2 font-lg ">+ </span> Add card
    </button>
  </div>
</template>

<script setup>
import { ref, defineEmits } from "vue";

const emits = defineEmits(["cardAdded"]);

const adding = ref(false);
const title = ref('');
const description = ref('');

const submit = () => {
  const newCard = { title: title.value, description: description.value };
  emits("cardAdded", newCard);

  title.value = '';
  description.value = '';
  adding.value = false;
};
</script>
