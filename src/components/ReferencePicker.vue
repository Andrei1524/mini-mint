<template>
  <div class="reference-picker">
    <catnip-list role="menu" size="m" v-for="group in references" :key="group.stepId">
      <catnip-list-item
        v-for="output in group.outputs"
        :key="output.key"
        @click="emit('select', output.ref)"
      >
        <catnip-icon slot="left" .name="group.icon" size="20"></catnip-icon>
        {{ group.label }} → {{ output.label }}
      </catnip-list-item>
    </catnip-list>
  </div>
</template>

<script setup>
import { computed } from "vue";
import { makeReference } from "../logic/references";
import { STEP_TYPES } from "../logic/stepTypes";
import { useEditorStore } from "../store/editor";

defineProps({
  /** The step that will receive the reference. */
  stepId: { type: String, required: true },
});

/** Emit `select` with a reference string, e.g. makeReference(stepId, outputKey) from logic/references.js */
const emit = defineEmits(["select"]);

const editor = useEditorStore();

const references = computed(() => {
  const flow = editor.flow;
  const targetId = editor.selectedStepId;
  if (!flow || !targetId) return [];

  // collect every step upstream of the selected one
  const upstream = new Set();
  const queue = [targetId];
  while (queue.length) {
    const id = queue.shift();
    for (const c of flow.connections) {
      if (c.targetStepId === id && !upstream.has(c.sourceStepId)) {
        upstream.add(c.sourceStepId);
        queue.push(c.sourceStepId);
      }
    }
  }
  upstream.delete(targetId); // guards against loops pointing back at itself

  return flow.steps
    .filter((step) => upstream.has(step.id))
    .map((step) => {
      const type = STEP_TYPES[step.type];
      return {
        stepId: step.id,
        label: step.name || type?.label || step.type,
        icon: type?.icon,
        outputs: (type?.outputs?.(step) ?? []).map((o) => ({
          ...o,
          ref: makeReference(step.id, o.key),
        })),
      };
    })
    .filter((group) => group.outputs.length);
});
</script>

<style scoped lang="scss">
.reference-picker {
  margin-top: var(--catnip-space-2);
}
</style>
