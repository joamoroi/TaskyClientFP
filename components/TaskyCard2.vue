<template>
    <div class="tasky-card my-5">
        <v-card class="tasky-card-card">
            <v-card-text class="tasky-card-text pa-1">
                <div class="tasky-card-text-container pa-2">
                    <slot />
                </div>
            </v-card-text>
        </v-card>
    </div>
</template>

<style scoped>

.tasky-card-card {
background-color:#1DE9B6; 
}

.tasky-card-text-container {
background-color:#B2DFDB;
}

.tasky-card-text{
background-color:#00BFA5;
}
</style>