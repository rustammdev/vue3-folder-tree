<template>
  <div class="min-h-screen bg-gray-50 transition-colors duration-300">
    <!-- Main content -->
    <div class="max-w-7xl mx-auto p-4 md:p-8">
      <!-- Folder selection button -->
      <button
        @click="showModal = true"
        class="flex items-center gap-2 px-6 py-3 bg-blue-500 hover:bg-blue-600 text-white rounded-lg transform transition-all duration-300 hover:-translate-y-0.5 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2"
      >
        <i class="fas fa-folder-plus"></i>
        <span>Select folder</span>
      </button>

      <!-- Selected folder info -->
      <div v-if="selectedFolderId !== null" class="mt-8">
        <div class="bg-white rounded-xl shadow-lg p-6">
          <div class="flex items-center gap-3 text-blue-500 mb-4">
            <i class="fas fa-folder-open text-xl"></i>
            <h3 class="text-xl font-semibold">Selected folder</h3>
          </div>

          <div class="space-y-2">
            <div class="flex items-center gap-2">
              <span class="text-gray-500">ID:</span>
              <span class="font-medium">{{ selectedFolderId }}</span>
            </div>
            <div class="flex items-center gap-2">
              <span class="text-gray-500">Name:</span>
              <span class="font-medium">{{ getSelectedFolderName() }}</span>
            </div>
          </div>
        </div>
      </div>

      <!-- Dock -->
      <div
        class="fixed bottom-8 left-1/2 -translate-x-1/2 flex gap-4 bg-white p-4 rounded-2xl shadow-lg"
      >
        <button
          @click="showModal = true"
          class="flex flex-col items-center gap-2 p-3 hover:bg-blue-500 hover:text-white rounded-lg transition-colors duration-300"
        >
          <i class="fas fa-folder text-2xl"></i>
          <span class="text-xs">Folders</span>
        </button>

        <button
          @click="clearSelection"
          class="flex flex-col items-center gap-2 p-3 hover:bg-blue-500 hover:text-white rounded-lg transition-colors duration-300"
        >
          <i class="fas fa-trash-alt text-2xl"></i>
          <span class="text-xs">Clear</span>
        </button>
      </div>
    </div>

    <!-- Modal -->
    <Modal
      :visible="showModal"
      :folders="folders"
      @close="showModal = false"
      @select="handleSelect"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import Modal from "./components/Modal.vue";

const showModal = ref(false);
const selectedFolderId = ref<number | null>(null);

// Mock data
const folders = [
  {
    id: 1,
    name: "Main folder",
    children: [
      {
        id: 2,
        name: "pictures.jpg",
        children: [],
      },
      {
        id: 3,
        name: "Docs",
        children: [
          {
            id: 4,
            name: "template.doc",
            children: [],
          },
        ],
      },
    ],
  },
  {
    id: 5,
    name: "Arxiv",
    children: [],
  },
];

function handleSelect(id: number) {
  selectedFolderId.value = id;
  showModal.value = false;
}

function clearSelection() {
  selectedFolderId.value = null;
}

function getSelectedFolderName(): string {
  const findFolder = (folders: any[], id: number): string | null => {
    for (const folder of folders) {
      if (folder.id === id) return folder.name;
      if (folder.children.length) {
        const found = findFolder(folder.children, id);
        if (found) return found;
      }
    }
    return null;
  };

  return selectedFolderId.value
    ? findFolder(folders, selectedFolderId.value) || "Folder not found"
    : "";
}
</script>
