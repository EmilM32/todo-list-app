<template>
  <div class="flex items-center justify-center h-screen">
    <div class="bg-white rounded-3xl border shadow-2xl p-3 w-1/3">
      <div class="grid grid-cols-1">
        <span class="font-bold text-2xl pb-3">TODO List</span>
        <div class="flex">
          <input
            v-model="newListItem"
            type="text"
            class="flex-1 border rounded-3xl p-3 focus:outline-none focus:ring-1 focus:ring-gray-300 focus:border-transparent"
            placeholder="Add to list"
            @keyup.enter="addToList"
          />
          <svg
            class="h-8 w-8 text-gray-500 hover:text-gray-600 active:text-green-700 place-self-center cursor-pointer"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            @click="addToList"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M12 9v3m0 0v3m0-3h3m-3 0H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z"
            />
          </svg>
        </div>
        <div v-for="(item, i) in allItems" :key="i" class="px-4 pt-2">
          <div class="flex">
            <span class="flex-none" :class="{ 'line-through': item.isDone }">{{
              item.title
            }}</span>
            <div class="flex-grow"></div>
            <div class="flex-none">
              <input type="checkbox" v-model="item.isDone" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs } from 'vue';

interface ListItem {
  title: string;
  isDone: boolean;
}

export default defineComponent({
  name: 'App',
  setup: () => {
    const state = reactive({ newListItem: '', allItems: [] as ListItem[] });
    return { ...toRefs(state) };
  },

  methods: {
    addToList() {
      if (this.newListItem) {
        const newItem: ListItem = {
          title: this.newListItem,
          isDone: false,
        };

        this.allItems.push(newItem);
        this.newListItem = '';
      }
    },
  },
});
</script>

<style lang="postcss">
body {
  @apply bg-gray-50 text-black;
}
</style>
