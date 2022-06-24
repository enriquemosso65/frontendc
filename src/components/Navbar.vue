<template>
<v-card class="overflow-hidden">
    <v-app-bar  color="#6A76AB" dark shrink-on-scroll prominent src="" fade-img-on-scroll scroll-target="#scrolling-techniques-3">
        <template v-slot:img="{ props }">
            <v-img v-bind="props" gradient="to top right, rgba(220,0,47,47), rgba(255,255,255,.7)"></v-img>
        </template>

        <v-app-bar-nav-icon></v-app-bar-nav-icon>

        <v-app-bar-title>Keyence Web APP CRUD</v-app-bar-title>

        <v-spacer></v-spacer>

        <v-btn icon>
            <v-icon>mdi-magnify</v-icon>
        </v-btn>

        <v-btn icon>
            <v-icon>mdi-heart</v-icon>
        </v-btn>

        <v-btn icon>
            <v-icon>mdi-dots-vertical</v-icon>
        </v-btn>

        <template v-slot:extension>
            <v-tabs align-with-title>
                <v-tab @click="llamarRegistro()">Nuevo registro</v-tab>
                <v-tab @click="llamarDatabase()">Base de Datos</v-tab>
                <v-tab @click="llamarRegistrocsv()">Subir csv</v-tab>
                <!-- <v-tab>Tab 3</v-tab> -->

                
            </v-tabs>
        </template>
    </v-app-bar>
    <v-sheet id="scrolling-techniques-3" class="overflow-y-auto" max-height="600">
        
    </v-sheet>
</v-card>
</template>
<script>
    export default {
        data(){
            
        },
        methods:{
            llamarRegistro(){
                this.$router.push('/registro')
            },
            llamarDatabase(){
                this.$router.push('/database')
            },
            llamarRegistrocsv(){
                this.$router.push('/registrocsv')
            }
        }
    }
</script>
