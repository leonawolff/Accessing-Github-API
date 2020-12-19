Vue.prototype.$username = null

<template>
    <v-app>
        <v-main>
            <v-container>
                <v-card align="center">
                    <img alt="frog" src="../assets/Frog.gif">
                </v-card>
                <br/><br/>


                <v-card color="purple lighten-4" align="center">
                    <v-card-text> 
                        <br/><br/>
                        <h1>GitHub API Visualiser</h1>
                        <br/><br/>
                            <v-text-field
                                v-model="name"
                                label="GitHub Username"
                                outlined
                            >
                            </v-text-field>
                        </v-card-text> 
                    
                            <v-btn
                                color="purple lighten-2"
                                elevation="4"
                                @click="reveal = true; getData()">
                                Go!
                            </v-btn>
                        <br/><br/>
                     
                </v-card>

                <br/><br/>

                <v-card v-if="reveal">
                    <v-card-text> 
                        <br/><br/>
                        <h1>GitHub API Visualiser</h1>
                        <br/><br/>
                        {{ this.userData }}
                    </v-card-text> 
                 </v-card>


            </v-container>
        </v-main>
    </v-app>
</template>

<script>
    import axios from "axios"
    export default {
        name: 'Main',
        
        data () {
            return { 
                reveal: null,
                name: "leonawolff",
                userData: null
            }
        },
        methods: {
            getData () {
                let url = "https://api.github.com"
                let username = this.name
                let url2 = url + "/users/" + username
                axios.get(url2, {
                    headers: {
                        authorization: "token " + process.env.VUE_APP_API_KEY
                    },
                    timeout:1000
                })
                .then(response => {
                    this.userData = response
                    console.log(this.userData)
                })
            }
        }
    }
</script>
