<script setup>
const { agentId } = useRoute().params

let abilities = {
    name: "",
    desc: "",
    icon: ""
}
const isSelected = ref(null)
function agentDesc(name, desc, icon) {
    isSelected.value = name
    abilities.name = name
    abilities.desc = desc
    abilities.icon = icon
    console.log(isSelected)
}

const { data: agent, pending } = useLazyFetch(
    `https://valorant-api.com/v1/agents/${agentId}`,
    {
        onResponse({ response }) {
            const data = response._data?.data
            const abilities = data?.abilities
            selectedAbility.value = abilities?.at?.(0)
        },
        server: false,
    }
)

const selectedAbility = ref(agent.data?.abilities?.at?.(0))
function selectAbility(ability) {
    selectedAbility.value = ability
}


</script>

<template>
    <v-theme-provider theme="dark" with-background>
        <v-img src="/bg.png" cover>
            
            <div>
                <v-btn variant="text" prepend-icon="mdi-arrow-left" @click="navigateTo('/agent')">Agent</v-btn>
            </div>
            <h1 class="text-center">Agent - {{ agent?.data.displayName }}</h1>

            <v-row v-if="agent?.data" class="pa-4">
                <v-col v-if="!$vuetify.display.sm" md="4">
                    <v-img :src="agent?.data.background" />
                </v-col>
                <v-col cols="12" md="4">
                    <div class="d-flex">
                        <v-img height="600" :src="agent?.data.fullPortraitV2" cover />
                    </div>
                </v-col>
                <v-col class="px-4" md="4">
                    <div class="d-flex flex-column">
                        <div> {{ agent?.data.role.displayName }}</div>
                        <div class="text-h2 font-weight-bold text-uppercase"> {{ agent?.data.displayName }}</div>
                    </div>
                    <v-row>
                        <v-col>
                            <v-card color="grey" height="94" class="pa-1">
                                Ingfo
                                <v-img :src="agent?.data.role.displayIcon" />
                            </v-card>
                        </v-col>
                        <v-col v-for="ability in agent?.data.abilities" :key="ability.slot">
                            <v-card @click="selectAbility(ability)" color="grey" height="94" class="pa-1">
                                {{ ability.slot }}
                                <v-img :src="ability.displayIcon" />
                            </v-card>
                        </v-col>
                    </v-row>
                    <v-row>
                        <v-col>
                            <v-card class="pa-4" style="color:rgb(91, 121, 255)"> {{ agent?.data.description }}</v-card>
                        </v-col>
                    </v-row>
                    <v-row>
                        <v-col>
                            <div class="text-h6 font-weight-bold text-uppercase">{{ agent?.data.role.displayName }}</div>
                            <div>{{ agent?.data.role.description }}</div>
                        </v-col>
                    </v-row>
                    <v-card variant="tonal" :title="selectedAbility?.displayName" :text="selectedAbility?.description"
                        class="mt-6"></v-card>
                </v-col>
            </v-row>
        </v-img>
    </v-theme-provider>
</template>