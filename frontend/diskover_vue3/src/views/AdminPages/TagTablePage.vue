<template>
    <v-container>
        <v-btn class="primary text-white" @click="onClickNewTag">
            ADD NEW TAG
        </v-btn>
        <v-card>
            <v-card-title class="primary text-white">
                <h1>Tags</h1>
            </v-card-title>
            <v-layout>
                <v-flex>
                    <TagTable v-on:delete-item = "onDeleteItem" v-on:edit-item="onEditItem" :tags="this.tags"/>
                </v-flex>
            </v-layout>
        </v-card>
    </v-container>
</template>

<script>
import AdminVerifierMixin from "@/mixins/AdminVerifierMixin"

export default{
    mixins: [AdminVerifierMixin],
    data(){
        return{
            tags: null
        }
    },
    mounted(){
        this.getTags()
    },
    methods:{
        getTags() {
            this.$http.get('/tags/')
            .then(response =>{
                this.tags = response.data
                console.log("Tags successfully retreived")
            })
            .else(error =>{
                console.log("Failed to GET tags")
                console.log(error)
            })
        },
        onClickNewTag(){
            this.$router.push(`/tagform/create`)
        },
        onEditItem(id){
            this.$router.push(`/tagform/update/${id}`)
        },
        onDeleteItem(id){
            this.$http.delete(`tags/${id}`)
            .then(response =>{
                console.log("Successfully deleted item", response)
                this.getTags()
            })
            .else(error =>{
                console.log("Failed to delete item", error)
            })
        }
    }
}
</script>