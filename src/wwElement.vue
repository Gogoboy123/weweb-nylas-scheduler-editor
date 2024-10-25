<template>
  <SchedulerEditor :clientId="content.clientId" :redirectUri="content.redirectUri" :requiresSlug="content.requiresSlug"
    :schedulerPreviewLink="content.schedulerPreviewLink" :key="updateComponent" />
</template>

<script setup>
import SchedulerEditor from './nylas/SchedulerEditor.vue'
import { ref, watch } from 'vue'
const props = defineProps({
  content: { type: Object, required: true },
})

const updateComponent = ref(0);
watch(() => props.content, () => (updateComponent.value += 1), { deep: true });
</script>