<template>
<v-container>
    <v-data-table :headers="headers" :items="desserts" sort-by="calories" class="elevation-1">
        <template v-slot:top>
            <v-toolbar flat>
                <v-toolbar-title>Users</v-toolbar-title>
                <v-divider class="mx-4" inset vertical></v-divider>
                <v-space></v-space>
                <v-dialog v-model="dialog" max-width="500px">
                    <template v-slot:activator="{ on, attrs }">
                        <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on">
                            New Item
                        </v-btn>
                    </template>
                    <v-card>
                        <v-card-title>
                            <span class="text-h5">{{ formTitle }}</span>
                        </v-card-title>

                        <v-card-text>
                            <v-container>
                                <v-row>
                                    <v-col cols="12" sm="6" md="4">
                                        <v-text-field v-model="editedItem.UserID" label="UserID"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" sm="6" md="4">
                                        <v-text-field v-model="editedItem.UserName" label="UserName"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" sm="6" md="4">
                                        <v-text-field v-model="editedItem.Date" label="Date"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" sm="6" md="4">
                                        <v-text-field v-model="editedItem.PunchIn" label="PunchIn "></v-text-field>
                                    </v-col>
                                    <v-col cols="12" sm="6" md="4">
                                        <v-text-field v-model="editedItem.PunchOut" label="PunchOut "></v-text-field>
                                    </v-col>
                                </v-row>
                            </v-container>
                        </v-card-text>

                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="blue darken-1" text @click="close">
                                Cancelar
                            </v-btn>
                            <v-btn color="blue darken-1" text @click="editarUser(editedItem)">
                                Modificar
                            </v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
                <v-dialog v-model="dialogDelete" max-width="500px">
                    <v-card>
                        <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
                            <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
                            <v-spacer></v-spacer>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
            </v-toolbar>
        </template>
        <template v-slot:item.actions="{ item }">
            <v-icon small class="mr-2" @click="editItem(item._id)">
                mdi-pencil
            </v-icon>
            <v-icon small @click="eliminarUser(item._id)">
                mdi-delete
            </v-icon>
        </template>
        <template v-slot:no-data>
            <v-btn color="primary" @click="initialize">
                Reset
            </v-btn>
        </template>
    </v-data-table>
</v-container>
</template>

<script>
export default {
    data: () => ({
        dialog: false,
        dialogDelete: false,
        headers: [
            {
                text: 'UserID',
                value: '_id'
            },
            {
                text: 'UserName',
                value: 'UserName'
            },
            {
                text: 'Date',
                value: 'Date'
            },
            {
                text: 'PunchIn',
                value: 'PunchIn'
            },
            {
                text: 'PunchOut',
                value: 'PunchOut',
            },
            {
                text:'Acciones',
                value: 'actions',
                sortable: false
            },
        ],
        desserts: [],
        editedIndex: -1,
        editedItem: [],
        defaultItem: {
            
        },
    }),

    computed: {
        formTitle() {
            return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
        },
    },

    watch: {
        dialog(val) {
            val || this.close()
        },
        dialogDelete(val) {
            val || this.closeDelete()
        },
    },

    created() {
        this.initialize()
        this.listarUsers();
    },

    methods: {
        initialize() {
        
        },

        editItem(item) {
            
            this.dialog = true
            console.log(item);
            this.axios.get(`buscarParametro/${item}`)
                .then(res=>{
                    this.editedItem = res.data
                })
        },

        deleteItem(item) {
            this.editedIndex = this.desserts.indexOf(item)
            this.editedItem = Object.assign({}, item)
            this.dialogDelete = true
        },

        deleteItemConfirm() {
            this.desserts.splice(this.editedIndex, 1)
            this.closeDelete()
        },

        close() {
            this.dialog = false
            this.$nextTick(() => {
                this.editedItem = Object.assign({}, this.defaultItem)
                this.editedIndex = -1
            })
        },

        closeDelete() {
            this.dialogDelete = false
            this.$nextTick(() => {
                this.editedItem = Object.assign({}, this.defaultItem)
                this.editedIndex = -1
            })
        },

        save() {
            if (this.editedIndex > -1) {
                Object.assign(this.desserts[this.editedIndex], this.editedItem)
            } else {
                this.desserts.push(this.editedItem)
            }
            this.close()
        },

        //
        listarUsers(){
            this.axios.get('/buscarTodo')
            .then((response)=>{
                this.desserts=response.data;
            })
            .catch((e)=>{
                console.log('error' + e)
            })
        },
        editarUser(item){

            this.axios.put(`/actualizar/${item._id}`, item)
            .then(res =>{
                this.$swal('success!',
                        'Usuario editado!',
                        'success');
                this.close();
                const index = this.desserts.findindex(n=> n._id === res.data._id)
                this.desserts[index].UserID = res.data.UserID;
                this.desserts[index].UserName = res.data.UserName;
                this.desserts[index].Date = res.data.Date;
                this.desserts[index].PunchIn = res.data.PunchIn;
                this.desserts[index].PunchOut = res.data.PunchOut;
                

            })
            .catch(e=>{
            console.log(e.response)
            })
        },

        eliminarUser(id){
            console.log(id);
            this.axios.delete(`/eliminarParametro/${id}`)
            .then(res =>{
                this.$swal('success!',
                        'Usuario eliminado!',
                        'success');
                
                const index = this.desserts.findIndex(item => item._id === res.data._id)
                this.desserts.splice(index, 1);
                this.editedIndex = this.desserts.indexOf(item);
                this.editedItem = Object.assign({}, item)

            }) .catch(e=>{
                console-log(e.response)
            })
        }
        
    },
}
</script>
