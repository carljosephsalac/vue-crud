<script setup>
import IconDots from '@/components/icons/IconDots.vue'
import IconSavedSolid from '@/components/icons/IconSavedSolid.vue'
import IconTrashSolid from '@/components/icons/IconTrashSolid.vue'
import IconEdit from '@/components/icons/IconEdit.vue'
import ModalEdit from './ModalEdit.vue'
import DropDownItem from './DropDownItem.vue'
import IconUnsaved from './icons/IconUnsaved.vue'
import { useNotesStore } from '@/stores/notes'

defineProps({
  note: {
    type: Object,
    required: true
  }
})

const noteStore = useNotesStore()
</script>

<template>
  <div
    :id="`dropdownMenuIconButton${note.id}`"
    :data-dropdown-toggle="`dropdownDots${note.id}`"
    class="flex items-center justify-center p-1 rounded-full hover:bg-gray-100 hover:cursor-pointer"
  >
    <IconDots />
    <!-- Dropdown menu -->
    <div
      :id="`dropdownDots${note.id}`"
      class="z-10 hidden w-48 px-2 bg-white divide-y divide-gray-300 rounded-lg shadow dark:bg-gray-700 dark:divide-gray-600"
    >
      <ul
        class="py-2 text-sm text-gray-700 dark:text-gray-200"
        :aria-labelledby="`dropdownMenuIconButton${note.id}`"
      >
        <DropDownItem v-if="note.is_saved" @click="noteStore.saveNotes(note.id)">
          <IconUnsaved />
          Unsave notes
        </DropDownItem>
        <DropDownItem v-else @click="noteStore.saveNotes(note.id)">
          <IconSavedSolid class="text-gray-800 size-5" />
          Save notes
        </DropDownItem>
        <DropDownItem @click="noteStore.trashNotes(note.id)">
          <IconTrashSolid class="text-gray-800 size-5" />
          Move to trash
        </DropDownItem>
      </ul>
      <div class="py-2 text-gray-700 dark:text-gray-200">
        <a
          :data-modal-target="`edit-modal${note.id}`"
          :data-modal-toggle="`edit-modal${note.id}`"
          class="flex items-center gap-2 px-2 py-2 text-base font-semibold rounded-md hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white"
        >
          <IconEdit class="text-gray-800" />
          Edit notes
        </a>
      </div>
    </div>
  </div>
  <ModalEdit :note="note" />
</template>
