<script setup lang="ts">
import { defineProps, defineEmits } from "vue";
import FolderTree from "./FolderTree.vue";

defineProps({
  visible: { type: Boolean, required: true },
  folders: {
    type: Array as () => { id: number; name: string; children: any[] }[],
    required: true,
  },
});

const emit = defineEmits(["close", "select"]);

function handleSelect(id: number) {
  emit("select", id);
}
</script>

<template>
  <transition name="modal">
    <div
      v-if="visible"
      class="fixed inset-0 bg-black/50 flex items-center justify-center z-50"
    >
      <div
        class="bg-white p-6 rounded-lg shadow-lg w-full max-w-md animate-fade-in"
      >
        <h2 class="text-xl font-semibold mb-4 text-center text-gray-800">
          Folders
        </h2>
        <FolderTree :folders="folders" :onSelect="handleSelect" />
        <div class="mt-6 text-center">
          <button
            @click="$emit('close')"
            class="px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 transition"
          >
            Close
          </button>
        </div>
      </div>
    </div>
  </transition>
</template>
