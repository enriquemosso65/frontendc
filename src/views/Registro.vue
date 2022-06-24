<template>
<v-container>
    <v-form  @submit.prevent = "agregarUser()">
        <v-text-field v-model="atributos.UserID" :counter="5" label="UserID" required></v-text-field>

        <v-text-field v-model="atributos.UserName" :counter="50" label="UserName" required></v-text-field>

        <v-text-field v-model="atributos.Date" :counter="30" label="Date" required></v-text-field>

        <v-text-field v-model="atributos.PunchIn" :counter="30" label="PunchIn" required></v-text-field>

        <v-text-field v-model="atributos.PunchOut" :counter="30" label="PunchOut" required></v-text-field>     

    
        <v-btn  color="success" class="mr-4" type="submit">
            Enviar registro
        </v-btn>

        <v-btn color="error" class="mr-4" >
            Limpiar campos
        </v-btn>
    </v-form>
</v-container>
</template>

<script>
export default{
    data(){
        return {
            Atributos:[],
            atributos: {
                UserID:"",
                UserName:"",
                Date:"",
                PunchIn:"",
                PunchOut:"",
            }
        }
    },
    methods:{
        agregarUser(){
            console.log(this.atributos);

            if(!this.atributos.UserID){
                this.$swal('Error!',
                    'Escribe un ID',
                    'error');
                
            }
            else if(!this.atributos.UserName){
                    this.$swal('Error!',
                        'Escribe un nombre!',
                        'error');

            }
            else if(!this.atributos.Date){
                    this.$swal('Error!',
                        'Escribe una Fecha!',
                        'error');

            }
            else if(!this.atributos.PunchIn){
                    this.$swal('Error!',
                        'Escribe una Hora!',
                        'error');

            }
            else if(!this.atributos.PunchOut){
                    this.$swal('Error!',
                        'Escribe una Hora !',
                        'error');

            }
            else{
                this.axios
                .post("/nuevo-registro", this.atributos)
                .then((res)=>{
                    this.Atributos.push(res.data);
                    this.atributos.UserID ="";
                    this.atributos.UserName = "";
                    this.atributos.Date ="";
                    this.atributos.PunchIn = "";
                    this.atributos.PunchOut ="";
                    alert("Registro agregado");

                    this.$swal('success!',
                        'Registro agregado!',
                        'success');
                })
                .catch((e)=>{
                    console.log(e.response);
                    alert("Error al agregar registro")
                })
            }
            
        }
    }

}

</script>
