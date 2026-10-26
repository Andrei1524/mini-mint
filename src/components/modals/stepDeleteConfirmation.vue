<template>
  <div>
    <catnip-dialog
      @close-dialog="closeModal"
      :open="ui.stepDeleteConfirmationOpen"
      title="Confirm deletion of a step"
    >
      <span
        >This step outputs are being used by others steps, see below the outputs and confirm
        deletion.</span
      >

      <catnip-list class="dependents" size="m" role="list">
        <catnip-list-item v-for="d in dependents" :key="d.step.id">
          {{ d.step.name || d.step.type }}
          <span class="dependents__outputs">uses {{ d.outputKeys.join(", ") }}</span>
        </catnip-list-item>
      </catnip-list>
      <catnip-button slot="footer" appearance="ghost" size="m" @click="closeModal">
        Cancel
      </catnip-button>
      <catnip-button slot="footer" size="m" @click="confirm">Delete anyway</catnip-button>
    </catnip-dialog>
  </div>
</template>

<script setup>
import { computed } from "vue";
import { getStepDependents } from "../../logic/graph";
import { useEditorStore } from "../../store/editor";
import { useUiStore } from "../../store/ui";

const ui = useUiStore();
const editor = useEditorStore();

const dependents = computed(() =>
  ui.stepToDeleteId ? getStepDependents(editor.flow, ui.stepToDeleteId) : [],
);

function confirm() {
  editor.confirmDeleteStep(ui.stepToDeleteId);
  ui.stepToDeleteId = null;
}

function closeModal() {
  ui.stepToDeleteId = null;
}
</script>

<style lang="scss" scoped></style>
