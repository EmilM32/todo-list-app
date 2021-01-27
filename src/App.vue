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
        <div class="overflow-y-auto max-h-56">
          <div v-for="(item, i) in allItems" :key="i" class="px-4 pt-2">
            <div class="flex">
              <div class="flex-none pr-2">
                <input type="checkbox" v-model="item.isDone" />
              </div>
              <span
                @click="item.isDone = !item.isDone"
                class="flex-none cursor-pointer"
                :class="{ 'line-through': item.isDone }"
                >{{ item.title }}</span
              >
              <div class="flex-grow"></div>
              <div class="flex">
                <button-delete @click="deleteItem(item.id)" />
              </div>
            </div>
          </div>
        </div>
        <div class="text-gray-400 text-sm flex pt-4">
          <span
            class="cursor-pointer hover:underline hover:text-gray-500 active:text-green-700"
            @click="deleteDone"
            >Clear done</span
          >
          <div class="flex-grow"></div>
          <i>
            Total items: {{ itemsCount }} - Done: {{ itemsDoneCount }} - Left:
            {{ itemsCount - itemsDoneCount }}
          </i>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs, computed } from 'vue';
import ButtonAdd from './components/ButtonAdd.vue';
import ButtonDelete from './components/ButtonDelete.vue';

interface TodoItem {
  id: number;
  title: string;
  isDone: boolean;
}

export default defineComponent({
  name: 'App',
  components: { ButtonAdd, ButtonDelete },
  setup: () => {
    const state = reactive({
      newListItem: '',
      allItems: [] as TodoItem[],
      itemsCount: computed(() => getAllItemsCount()),
      itemsDoneCount: computed(() => getItemsDoneCount()),
    });

    function getItemsDoneCount(): number {
      return state.allItems.reduce(
        (acc: number, cur: TodoItem) => acc + +cur.isDone,
        0
      );
    }

    function getAllItemsCount(): number {
      return state.allItems.length;
    }

    function addToList(): void {
      if (state.newListItem) {
        const newItem: TodoItem = {
          id: generateUniqueId(),
          title: state.newListItem,
          isDone: false,
        };

        state.allItems.push(newItem);
        state.newListItem = '';
      }
    }

    function deleteItem(id: number): void {
      state.allItems = state.allItems.filter(
        (item: TodoItem) => item.id !== id
      );
    }

    function deleteDone(): void {
      state.allItems = state.allItems.filter((item: TodoItem) => !item.isDone);
    }

    function generateUniqueId(): number {
      function getRandom(maxValue: number) {
        return Math.floor(Math.random() * maxValue) + 1;
      }

      const maxValue = 99999;
      const allIdx: number[] = [];
      state.allItems.forEach((item) => {
        allIdx.push(item.id);
      });

      let uniqueId = getRandom(maxValue);
      while (allIdx.includes(uniqueId)) {
        uniqueId = getRandom(maxValue);
      }

      return uniqueId;
    }

    return { ...toRefs(state), addToList, deleteItem, deleteDone };
  },
});
</script>

<style lang="postcss">
body {
  @apply bg-gray-50 text-black;
}
</style>
