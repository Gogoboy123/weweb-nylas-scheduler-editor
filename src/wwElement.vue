<template>
  <Scheduler :configurationId="content.configurationId" :key="updateComponent" />
</template>

<script setup>
import Scheduler from './nylas/Scheduler.vue'
import { ref, watch } from 'vue'
const props = defineProps({
  content: { type: Object, required: true },
})

const updateComponent = ref(0);
watch(() => props.content, () => (updateComponent.value += 1), { deep: true });
</script>