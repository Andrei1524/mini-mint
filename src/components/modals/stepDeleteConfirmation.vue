<template>
  <div>
    <catnip-dialog :open="ui.stepDeleteConfirmationOpen" title="Confirm changes">
      <span>Use the body for supporting content, form fields, or confirmation copy.</span>
      <catnip-tooltip placement="right">Extra hint without filling out a form</catnip-tooltip>
      <catnip-button slot="footer" appearance="ghost" size="m">Cancel</catnip-button>
      <catnip-button slot="footer" size="m">Submit</catnip-button>
    </catnip-dialog>
  </div>
</template>

<script setup>
import { useUiStore } from "../../store/ui";

const ui = useUiStore();
</script>

<style lang="scss" scoped></style>
