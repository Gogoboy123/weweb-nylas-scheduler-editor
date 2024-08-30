<template>
    <nylas-scheduling></nylas-scheduling>
</template>

<script setup>
import { onMounted, onBeforeMount } from 'vue'

const props = defineProps({
    configurationId: {
        type: String,
        required: true
    }
})

onBeforeMount(() => {
    let script = wwLib.getFrontDocument().createElement("script");
    script.type = "module";
    script.id = "nylas-scheduler-id"
    script.defer = true;
    script.src = "https://unpkg.com/@nylas/web-elements@latest";
    if (wwLib.getFrontDocument().getElementById('nylas-scheduler-id')) {
        wwLib.getFrontDocument().getElementById('nylas-scheduler-id').remove();
    }
    wwLib.getFrontDocument().head.appendChild(script);
})


onMounted(() => {
    const nylasScheduling = wwLib.getFrontDocument().querySelector('nylas-scheduling');
    nylasScheduling.configurationId = props.configurationId;
})
</script>