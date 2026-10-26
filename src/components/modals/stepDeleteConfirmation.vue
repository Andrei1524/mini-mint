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
      <catnip-button slot="footer" appearance="ghost" size="m" @click="closeModal">
        Cancel
      </catnip-button>
      <catnip-button slot="footer" size="m">Submit</catnip-button>
    </catnip-dialog>
  </div>
</template>

<script setup>
import { useUiStore } from "../../store/ui";

const ui = useUiStore();

function closeModal() {
  ui.stepToDeleteId = null;
}
</script>

<style lang="scss" scoped></style>
