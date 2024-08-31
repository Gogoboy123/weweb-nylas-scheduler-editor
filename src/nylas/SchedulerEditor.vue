<template>
    <nylas-scheduler-editor></nylas-scheduler-editor>
</template>

<script setup>
import { onMounted, onBeforeMount } from 'vue'
const props = defineProps({
    configurationId: {
        type: String,
        required: true
    },
    clientId: {
        type: String,
        required: true
    },
})

onBeforeMount(() => {
    let script = wwLib.getFrontDocument().createElement("script");
    script.type = "module";
    script.id = "nylas-scheduler-id-editor"
    script.defer = true;
    script.src = "https://unpkg.com/@nylas/web-elements@1.1.1/dist/nylas-web-elements/nylas-web-elements.esm.js";
    if (wwLib.getFrontDocument().getElementById('nylas-scheduler-id-editor')) {
        wwLib.getFrontDocument().getElementById('nylas-scheduler-id-editor').remove();
    }
    wwLib.getFrontDocument().head.appendChild(script);
})

onMounted(() => {
    const nylasSchedulerEditor = wwLib.getFrontDocument().querySelector('nylas-scheduler-editor')
    nylasSchedulerEditor.schedulerPreviewLink = `${window.location.origin}/?config_id=${props.configurationId}`
    nylasSchedulerEditor.nylasSessionsConfig = {
        clientId: props.clientId,
        redirectUri: `${window.location.href}/scheduler-editor`,
        domain: 'https://api.us.nylas.com/v3',
        hosted: true,
        accessType: 'offline'
    }
    nylasSchedulerEditor.defaultSchedulerConfigState = {
        selectedConfiguration: {
            requires_session_auth: false
        }
    }
})
</script>