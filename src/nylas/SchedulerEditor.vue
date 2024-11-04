<template>
    <nylas-scheduler-editor></nylas-scheduler-editor>
</template>

<script setup>
import { onMounted, onBeforeMount } from 'vue'
const props = defineProps({
    clientId: {
        type: String,
        required: true
    },
    redirectUri: {
        type: String,
        required: true
    },
    schedulerPreviewLink: {
        type: String,
        required: true
    },
    requiresSlug: {
        type: String,
        required: true
    },
})

onBeforeMount(() => {
    let script = wwLib.getFrontDocument().createElement("script");
    script.type = "module";
    script.id = "nylas-scheduler-id-editor"
    script.defer = true;
    script.src = "https://cdn.jsdelivr.net/npm/@nylas/web-elements@1.1.1/dist/nylas-web-elements/nylas-web-elements.esm.js";
    if (wwLib.getFrontDocument().getElementById('nylas-scheduler-id-editor')) {
        wwLib.getFrontDocument().getElementById('nylas-scheduler-id-editor').remove();
    }
    wwLib.getFrontDocument().head.appendChild(script);
})

onMounted(() => {
    const nylasSchedulerEditor = wwLib.getFrontDocument().querySelector('nylas-scheduler-editor')
    nylasSchedulerEditor.nylasSessionsConfig = {
        clientId: props.clientId,
        redirectUri: props.redirectUri || window.location.href,
        domain: 'https://api.us.nylas.com/v3',
        hosted: true,
        accessType: 'offline'
    }
    nylasSchedulerEditor.schedulerPreviewLink = props.schedulerPreviewLink
    nylasSchedulerEditor.requiresSlug = props.requiresSlug

    nylasSchedulerEditor.defaultSchedulerConfigState = {
        selectedConfiguration: {
            scheduler: { // The callback URLs to be set in email notifications
                rescheduling_url: `${window.location.origin}/reschedule/:booking_ref`, // The URL of the email notification includes the booking reference
                cancellation_url: `${window.location.origin}/cancel/:booking_ref`
            },
        },
    };
})
</script>