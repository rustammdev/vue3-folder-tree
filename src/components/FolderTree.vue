<script setup lang="ts">
import { ref } from "vue";

// structure
interface Folder {
  id: number;
  name: string;
  children: Folder[];
}

const props = defineProps({
  folders: {
    type: Array as () => Folder[],
    required: true,
  },
  onSelect: {
    type: Function as unknown as () => (id: number) => void,
    required: true,
  },
});

const openFolders = ref<Record<number, boolean>>({});

const toggleFolder = (id: number) => {
  openFolders.value[id] = !openFolders.value[id];
};
</script>

<template>
  <div class="font-inter text-gray-700 dark:text-gray-200">
    <ul class="pl-2 m-0 list-none">
      <li
        v-for="folder in props.folders"
        :key="folder.id"
        class="relative py-1"
      >
        <!-- Vertikal -->
        <div
          class="absolute left-[-12px] top-0 h-full border-l-2 border-dashed border-gray-200 dark:border-gray-700 last:h-1/2"
        ></div>

        <div
          :class="{ 'bg-gray-100 dark:bg-gray-800': openFolders[folder.id] }"
        >
          <!-- headerer  -->
          <div
            class="relative flex items-center p-2 cursor-pointer rounded-md transition-all duration-200 hover:bg-gray-100 dark:hover:bg-gray-800 group"
            @click="toggleFolder(folder.id)"
          >
            <!-- Gorizontal -->
            <div
              class="absolute left-[-12px] top-1/2 w-3 border-t-2 border-dashed border-gray-200 dark:border-gray-700"
            ></div>

            <!--  icon -->
            <div
              class="flex items-center mr-3 text-gray-500 dark:text-gray-400 transition-colors duration-200 group-hover:text-blue-500 dark:group-hover:text-blue-400"
            >
              <template v-if="folder.children.length > 0">
                <i
                  :class="
                    openFolders[folder.id]
                      ? 'fas fa-folder-open'
                      : 'fas fa-folder'
                  "
                  class="text-lg"
                ></i>
              </template>
              <i v-else class="fas fa-file text-lg"></i>
            </div>

            <span
              class="text-sm font-medium transition-colors duration-200 hover:text-blue-500 dark:hover:text-blue-400"
              @click.stop="props.onSelect(folder.id)"
            >
              {{ folder.name }}
            </span>

            <!-- Arrow icon, children -->
            <div
              v-if="folder.children.length > 0"
              class="ml-auto transform transition-transform duration-200"
              :class="openFolders[folder.id] ? 'rotate-90' : ''"
            >
              <i
                class="fas fa-chevron-right text-xs text-gray-400 dark:text-gray-500"
              ></i>
            </div>
          </div>

          <!-- subfolders -->
          <transition
            enter-active-class="transition-all duration-300 ease-out"
            enter-from-class="opacity-0 -translate-y-1"
            enter-to-class="opacity-100 translate-y-0"
            leave-active-class="transition-all duration-300 ease-in"
            leave-from-class="opacity-100 translate-y-0"
            leave-to-class="opacity-0 -translate-y-1"
          >
            <div v-if="openFolders[folder.id]" class="pl-6">
              <folder-tree
                v-if="folder.children.length"
                :folders="folder.children"
                :onSelect="props.onSelect"
                class="relative ml-2"
              />
            </div>
          </transition>
        </div>
      </li>
    </ul>
  </div>
</template>
