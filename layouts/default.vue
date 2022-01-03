<template>
    <div class="tasky-layout-default">
        <v-app class="tasky-main">
            <TaskyAppBar />
                <v-main>
                    <Nuxt />
                </v-main>
            <TaskyFooter />
        </v-app>
    </div>
</template>

<style scoped>

.tasky-main {
background-color:#E0F2F1;
}

</style>