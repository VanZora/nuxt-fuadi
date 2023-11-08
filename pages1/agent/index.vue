<script setup>
import { onMounted } from 'vue';

const { data: agents, pending, error } = useFetch(
    "https://valorant-api.com/v1/agents",
    {
        query: {
            isPlayableCharacter: true,
        },
    })

</script>

<template>
    <h1>Agents</h1>
    <agent-list :agents="agents.data" />
</template>