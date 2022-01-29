<template>
    <div class="tasky-home">
        <TaskyAppBar :back="false"></TaskyAppBar>
        <v-container style="align-content:center">
            <v-row>
                <v-col cols="1,5"></v-col>
                <v-col cols="5">
                    <img class="image-main-1 mt-2" src="https://to-do-cdn.microsoft.com/static-assets/c26cd0d92ec61ba2c661adefaa535ab3cc4fb124f347a850fded8034dad5d360/icons/welcome-left.png" alt="">
                    <img class="image-main-2 mt-2" src="https://to-do-cdn.microsoft.com/static-assets/f2f56b7d4c72910540effed9ccddae703d8d09b94075dddfeeab6cd79def0c60/icons/welcome-right.png" alt="">
                </v-col>
                <v-col cols="4">
                    <div class="tasky-description">
                        <p class="tasky-description-headline">Adéntrate en TASKY</p>
                        <p class="tasky-description-text">Ya esta aquí TASKY. Regístrate e incia sesión para extraer todo su jugo. Logra llegar a tdoas tus metas con esta nueva agenda virtual. Gestiona todos tus quehacers del dia, semana, mes o año con esa nueva aplicación tan sencilla e intuitiva. TASKY te proporciona todas las herramientas para que alcances el éxito. Aprovecha para hacer más facil tu día a día y olvídate del resto.</p>
                        <div class="tasky-description-buttons d-flex justify-center">
                            <v-btn to="/sign-up" class="mx-2" color="#1DE9B6" large>Registrarse</v-btn>
                            <v-btn to="/sign-in" class="mx-2" color="#1DE9B6" large>Iniciar Sesión</v-btn>
                        </div>
                    </div>
                </v-col>
                <v-col cols="1,5"></v-col>
            </v-row>
        </v-container>
    </div>
</template>

<style scoped>

.tasky-main {
background-color:#E0F2F1;
}

.image-main-1 {
align-content: center;
width: 369px;
height: 570px;
}

.image-main-2 {
width: 270px;
height: 650px;
}

.tasky-description {
color: black;
margin-top: 200px;
}

.tasky-description-headline {
text-align: left;
font-size: 45px;
margin-bottom: 8px;
}

.tasky-description-text {
text-align: justify;
}

</style>
