Vue.prototype.$username = null

<template>
    <v-app>
        <v-main>
            <v-container>
                <v-card align="center">
                    <img alt="frog" v-if='this.userData === null' src="../assets/Frog.gif">
                    <img alt="profile picture" v-if='this.userData !== null' :src=this.userData.data.avatar_url>
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

                <v-card v-if="reveal" align="center">
                    <v-card-text>
                        <br/>
                        <h1>{{ this.userData["data"]["login"]}}</h1>
                        <div v-if= 'this.userData["data"]["name"] !== null'>
                            <br/>
                            <h3>{{this.userData["data"]["name"]}}</h3>
                        </div>
                        
                        <br/>
                        Number of public repositories: {{ this.userData["data"]["public_repos"]}}
                        <br/>
                        Followers: {{ this.userData["data"]["followers"]}}
                        <br/>
                        Following: {{ this.userData["data"]["following"]}}
                        <br/>
                        Account created: {{ format_date(this.userData["data"]["created_at"])}}
                        <div v-if= 'this.userData["data"]["bio"] !== null'>
                            Bio: {{ this.userData["data"]["bio"]}}
                        </div>
                        <div v-if= 'this.userData["data"]["email"] !== null'>
                            Email: {{ this.userData["data"]["email"]}}
                        </div>
                        <br/>
                    </v-card-text> 
                </v-card>
                
                <br/><br/>

                <v-card v-if="reveal" align="center">
                    <v-card-text>
                        <h2>
                            Languages Used
                        </h2>


                        <pie-chart :data='this.languageData'></pie-chart>
                    </v-card-text>
                </v-card>

            </v-container>
        </v-main>
    </v-app>
</template>

<script>
    import moment from 'moment'
    import axios from "axios"
    export default {
        name: 'Main',
        data () {
            return { 
                reveal: null,
                name: "leonawolff",
                userData: null,
                repoData: [],
                languages: [],
                languageData: []
            }
        },
        methods: {
            getData () {
                console.log(this.name)
                let url = "https://api.github.com"
                let url2 = url + "/users/" + this.name
                axios.get(url2, {
                    headers: {
                        authorization: "token " + process.env.VUE_APP_API_KEY
                    },
                    timeout:1000000
                })
                .then(response => {
                    this.userData = response
                    console.log(this.userData)
                    this.getRepoData(1)
                })
            },
            getRepoData (page) {
                console.log(this.name)
                let url3 = "https://api.github.com/users/" + this.name + "/repos?per_page=100&page=" + page
                axios.get(url3, {
                    headers: {
                        authorization: "token " + process.env.VUE_APP_API_KEY
                    },
                    timeout:1000000
                })
                .then(response => {
                    this.repoData = this.repoData.concat(response.data)
                    console.log(this.repoData)
                    
                    if(response.data.length === 100){
                        this.getRepoData(page+1)
                    }
                    this.getLanguageInfo()
                })
            },
            format_date(value){
                if(value){
                    return moment(String(value)).format('MMMM Do YYYY, h:mm a')
                }
            },
            getLanguageInfo(){

                let i;
                if (this.repoData !== null){
                    for (i in this.repoData){
                        console.log("all the shit " + this.repoData[i].language)
                        if(this.repoData[i].language !== null){
                            this.languages.push(this.repoData[i].language)
                        }
                        
                    }
                    console.log("bhhhv" + this.languages.length)
                }
                this.languageData = this.languages.reduce((acum,cur) => Object.assign(acum,{[cur]: (acum[cur] || 0)+1}),{});
                console.log(this.languageData)
    //            console.log("first boy! " + this.languageData[0])
            }
        }
    }
</script>
