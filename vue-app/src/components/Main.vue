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
                                :rules="nameRules"
                                label="GitHub Username"
                                outlined
                            >
                            </v-text-field>
                        </v-card-text> 
                    
                        <v-card-actions>
                            <v-btn
                                color="purple lighten-2"
                                elevation="4"
                                @click="reveal = true" >
                                Go!
                            </v-btn>
                        </v-card-actions>

                        <v-expand-transition>
                            <v-card
                                v-if="reveal"
                                class="transition-fast-in-fast-out v-card--reveal"
                                style="height: 100%;"
                            >
                                <v-card-text> 
                                    <br/><br/>
                                    <h1>GitHub API Visualiser</h1>
                                    <br/><br/>
                                    <div>Sum Shit</div>
                                </v-card-text> 
                    
                                <v-card-actions>
                                    <v-btn
                                        color="purple lighten-2"
                                        elevation="4"
                                        @click="reveal = false" >
                                        Close
                                    </v-btn>
                                </v-card-actions>
                            </v-card>
                        </v-expand-transition>
                </v-card>
            </v-container>
        </v-main>
    </v-app>
</template>

<script>
    export default {
        name: 'Main'
    }
</script>
