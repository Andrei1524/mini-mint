<template>
  <div>
    <catnip-dialog
      @close-dialog="toggleModal(false)"
      :open="ui.stepDeleteConfirmationOpen"
      title="Confirm deletion of a step"
    >
      <span
        >This step outputs are being used by others steps, see below the outputs and confirm
        deletion.</span
      >
      <catnip-button slot="footer" appearance="ghost" size="m" @click="toggleModal(false)">
        Cancel
      </catnip-button>
      <catnip-button slot="footer" size="m">Submit</catnip-button>
    </catnip-dialog>
  </div>
</template>

<script setup>
import { useUiStore } from "../../store/ui";

const ui = useUiStore();

function toggleModal(value) {
  ui.stepDeleteConfirmationOpen = value;
}
</script>

<style lang="scss" scoped></style>
