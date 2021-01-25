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
          <button-add @click="addToList" />
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
        <i class="text-gray-400 text-sm text-right pt-4">
          Total items: {{ itemsCount }} - Done: {{ itemsDoneCount }} - Left:
          {{ itemsCount - itemsDoneCount }}
        </i>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, computed } from 'vue';
import ButtonAdd from './components/ButtonAdd.vue';

interface ListItem {
  title: string;
  isDone: boolean;
}

export default defineComponent({
  name: 'App',
  components: { ButtonAdd },
  setup: () => {
    const state = reactive({ newListItem: '', allItems: [] as ListItem[] });

    const itemsCount = computed(() => state.allItems.length);
    const itemsDoneCount = computed(() =>
      state.allItems.reduce((acc, cur) => acc + +cur.isDone, 0)
    );

    return { ...toRefs(state), itemsCount, itemsDoneCount };
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
