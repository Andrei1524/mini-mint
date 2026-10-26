<template>
  <div class="reference-picker">
    <!-- <catnip-list role="menu" size="m">
        <catnip-list-item v-for="type in ADDABLE_STEP_TYPES" :key="type" @click="add(type)">
          <catnip-icon slot="left" .name="getStepIcon(type)" size="20"></catnip-icon>
          {{ getStepTypeLabel(type) }}
        </catnip-list-item>
      </catnip-list> -->
  </div>
</template>

<script setup>
import { useEditorStore } from "../store/editor";

defineProps({
  /** The step that will receive the reference. */
  stepId: { type: String, required: true },
});

/** Emit `select` with a reference string, e.g. makeReference(stepId, outputKey) from logic/references.js */
defineEmits(["select"]);

const editor = useEditorStore();

console.log(editor.flow.steps);
</script>

<style scoped lang="scss">
.reference-picker {
  margin-top: var(--catnip-space-2);
}
</style>
