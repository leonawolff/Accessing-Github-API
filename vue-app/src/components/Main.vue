<template>
    <v-app>
        <v-main>
            <v-container>
                <v-card align="center">
                    <img alt="frog" src="../assets/Frog.gif">
                </v-card>
                <br/><br/>
                <v-card color="purple lighten-4" align="center">
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
                    <v-btn
                        color="purple lighten-2"
                        elevation="4" >
                        Go!
                    </v-btn>
                    <br/><br/>
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

