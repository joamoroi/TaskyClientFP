<template>
    <div class="tasky-layout-auth">
        <v-app class="tasky-main">
            <TaskyAppBar />
                <v-main>
                    <v-container>
                        <v-row>
                            <v-col>
                                <Nuxt />
                            </v-col>
                        </v-row>
                    </v-container>    
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