<script setup>
const { agentId } = useRoute().params
const { data: agent, pending } = useFetch(`https://valorant-api.com/v1/agents/${agentId}`)
</script>

<template>
    <h1>Valorant Detail {{ agentId }}</h1>

    <v-row>
        <v-col>Kiri</v-col>
        <v-col>
            <div class="d-flex">
                <v-img height="600" :src="agent.data.fullPortraitV2" cover />
            </div>
        </v-col>
        <v-col>
            <div class="d-flex flex-column">
                <div> {{ agent.data.role.displayName }}</div>
                <div class="text-h2 font-weight-bold text-uppercase"> {{ agent.data.displayName }}</div>
            </div>
            <v-row>
                <v-col>
                    <v-card color="grey" height="64">
                        <v-img :src="agent.data.role.displayIcon" />
                    </v-card>
                </v-col>
                <v-col v-for="ability in agent.data.abilities" :key="ability.slot">
                    <v-card color="grey" height="64">
                        <v-img :src="ability.displayIcon" />
                    </v-card>
                </v-col>
            </v-row>
        </v-col>
    </v-row>
    <pre>
        {{ agent.data }}
    </pre>
</template>