<template>
    <div class="tasky-card">
        <v-card class="tasky-card-card">
            <v-card-title>
                <div class="tasky-card-title d-flex">
                    <v-icon color="white" size="30" class="">{{ icon }}</v-icon>
                    <p class="headline mx-2 white--text">{{ title }}</p>
                </div>
            </v-card-title>
            <v-card-text class="tasky-card-text pa-1">
                <div class="tasky-card-text-container pa-2">
                    <slot />
                </div>
            </v-card-text>
        </v-card>
    </div>
</template>

<script>
export default {
    props: {
        title: {
            type: String,
        },
        icon: {
            type: String,
            default: undefined,
        },
    },
}
</script>


<style scoped>

.tasky-card-card {
background-color:#00897B; 
}

.tasky-card-title {
height: 30px; 
}

.tasky-card-text-container {
background-color:#80CBC4;
}

.tasky-card-text{
background-color:#00897B;
}
</style>