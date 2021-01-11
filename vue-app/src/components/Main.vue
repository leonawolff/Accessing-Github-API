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
                        <v-row>
                            <v-col>
                                <br/>
                            </v-col>
                            <v-col>
                                <v-text-field
                                    v-model="name"
                                    label="GitHub Username"
                                    outlined
                                    clearable
                                >
                                </v-text-field>
                            </v-col>
                            <v-col>
                                <br/>
                            </v-col>
                        </v-row>
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
                        <h1>{{ this.userData.data.login}}</h1>
                        <div v-if= 'this.userData.data.name !== null'>
                            <br/>
                            <h3>{{this.userData.data.name}}</h3>
                        </div>
                        
                        <br/>
                        Number of public repositories: {{ this.userData.data.public_repos}}
                        <br/>
                        Followers: {{ this.userData.data.followers }}
                        <br/>
                        Following: {{ this.userData.data.following }}
                        <br/>
                        Account created: {{ format_date(this.userData.data.created_at)}}
                        <div v-if= 'this.userData.data.bio !== null'>
                            Bio: {{ this.userData.data.bio }}
                        </div>
                        <div v-if= 'this.userData.data.email !== null'>
                            Email: {{ this.userData.data.email }}
                        </div>
                        <br/>
                    </v-card-text> 
                </v-card>
                
                <br/><br/>

                <v-card v-if="reveal && !this.notFound" align="center">
                    <v-card-text>
                        <v-row>
                            <v-col>
                                <h2> User Stats </h2> 
                                <br/><br/><br/>
                                <img alt="user stats" :src="this.readMeStatsUrl"/>
                                <br/><br/>
                            </v-col>
                            <v-col>
                                <h2>
                                    Languages Used
                                </h2>
                                <br/>
                                <pie-chart :data='this.languageData'></pie-chart>
                                <br/><br/>
                            </v-col>
                        </v-row>
                        <h2>
                            Number of commits per repository
                        </h2>
                        <line-chart :data='this.commitData'></line-chart>
                        <br/> <br/> <br/> 
                        <h2>
                            Commit Times
                        </h2>
                        <scatter-chart :data="this.commitTimes" xtitle="Time" ytitle="Days" :xmin="0" :xmax="24"></scatter-chart>
                        <br/> <br/> <br/>
                        <h2 v-if="this.topRepos.length > 1"> Repository Insights </h2>
                        <v-row v-if="this.topRepos.length >= 2">
                            <v-col>
                                <br/>
                                <img alt="repo stats" :src="this.repoStatsUrl + this.name + '&repo=' +this.topRepos[0][0]"/>
                            </v-col>
                            <v-col>
                                <br/>
                                <img alt="repo stats" :src="this.repoStatsUrl + this.name + '&repo=' +this.topRepos[1][0]"/>
                            </v-col>
                        </v-row>
                        <v-row v-if="this.topRepos.length >= 4">
                            <v-col>
                                <br/>
                                <img alt="repo stats" :src="this.repoStatsUrl + this.name + '&repo=' +this.topRepos[2][0]"/>
                                <br/>
                            </v-col>
                            <v-col>
                                <br/>
                                <img alt="repo stats" :src="this.repoStatsUrl + this.name + '&repo=' +this.topRepos[3][0]"/>
                                <br/>
                            </v-col>
                        </v-row>
                        <v-row v-if="this.topRepos.length >= 6">
                            <v-col>
                                <br/>
                                <img alt="repo stats" :src="this.repoStatsUrl + this.name + '&repo=' +this.topRepos[4][0]"/>
                                <br/>
                            </v-col>
                            <v-col>
                                <br/>
                                <img alt="repo stats" :src="this.repoStatsUrl + this.name + '&repo=' +this.topRepos[5][0]"/>
                                <br/>
                            </v-col>
                        </v-row>
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
                notFound: false,
                repoData: [],
                languages: [],
                languageData: [],
                tempCommitData: [],
                commitData: [],
                moreCommitData: [],
                commitTimes: [],
                topRepos: [],
                repoStatsUrl: "https://github-readme-stats.vercel.app/api/pin/?username=",
                readMeStatsUrl: null
            }
        },
        methods: {
            getData () {
                this.clearData()
                let url = "https://api.github.com"
                let url2 = url + "/users/" + this.name
                axios.get(url2, {
                    headers: {
                        authorization: "token " + process.env.VUE_APP_API_KEY
                    },
                    timeout:1000000
                })
                .then(response => {
                    console.log(response)
                    this.notFound = false
                    this.userData = response
                    this.getRepoData(1)
                    this.readMeStatsUrl = "https://github-readme-stats.vercel.app/api?username="+ this.name
                })
                .catch(error => {
                    this.notFound = true
                    console.log(error)
                })
            },
            clearData(){
                this.userData = null
                this.repoData = []
                this.languages = []
                this.languageData = []
                this.tempCommitData = []
                this.commitData = []
                this.readMeStatsUrl = null
                this.moreCommitData = []
                this.commitTimes = []
                this.topRepos = []
                this.repoStatsUrl = "https://github-readme-stats.vercel.app/api/pin/?username="
            },
            getRepoData (page) {
                let url3 = "https://api.github.com/users/" + this.name + "/repos?per_page=100&page=" + page
                axios.get(url3, {
                    headers: {
                        authorization: "token " + process.env.VUE_APP_API_KEY
                    },
                    timeout:1000000
                })
                .then(response => {
                    this.repoData = this.repoData.concat(response.data)
                    
                    if(response.data.length === 100){
                        this.getRepoData(page+1)
                    }
                    this.getLanguageInfo()
                    this.getCommitsPerRepo()
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
                        if(this.repoData[i].language !== null){
                            this.languages.push(this.repoData[i].language)
                        }
                    }
                }
                this.languageData = this.languages.reduce((acum,cur) => Object.assign(acum,{[cur]: (acum[cur] || 0)+1}),{});
            },
            getCommitsPerRepo(){
                let url;
                let i;
                for(i in this.repoData){
                    url = "https://api.github.com/repos/" + this.name + "/" + this.repoData[i].name + "/commits?per_page=100&page="
                    this.getCommitData(url, 1, this.repoData[i].name, i)
                }
            },
            getCommitData(url, page, repoName, i){
                axios.get(url+page, {
                    headers: {
                        authorization: "token " + process.env.VUE_APP_API_KEY
                    },
                    timeout:1000000
                })
                .then(response => {

                    this.tempCommitData = this.tempCommitData.concat(response.data)

                    if(response.data.length === 100){
                        this.getCommitData(url, page+1, repoName)
                    }
                    else{
                        let numCommits = ((page - 1) * 100) + response.data.length
                        this.commitData.push([repoName, numCommits])
                        this.moreCommitData.push([i, this.tempCommitData])
                        this.tempCommitData = [];
                        if(i == this.repoData.length-1){
                            this.sortCommitData();
                        }
                    }
                })
            },
            sortCommitData(){
                for(let i = 0; i < this.moreCommitData.length; i++){
                    
                    let currentRepo = this.moreCommitData[i];
                    for(let j = 0; j < currentRepo[1].length; j++){
                        let date = currentRepo[1][j].commit.author.date;
                        let day = moment(String(date)).format('dddd');
                        let time = moment(String(date)).format('h a');
                        this.commitTimes.push([time, day])
                    }
                }
                this.chartifyCommitTimes();
                this.getTopRepos();
            },
            chartifyCommitTimes(){
                for(let i = 0; i < this.commitTimes.length; i++){
                    let time = this.commitTimes[i][0];
                    let times = time.split(" ");
                    if((times[1] === "pm" && parseInt(times[0]) !== 12)){
                        times[0] = parseInt(times[0]) + 12
                    }
                    else if(times[1] === "am" && parseInt(times[0] === 12)){
                        times[0] = 0
                    }
                    else{
                        times[0] = parseInt(times[0])
                    }
                    let day;
                    switch (this.commitTimes[i][1]) {
                        case 'Monday':
                            day = 0
                            break;
                        case 'Tuesday':
                            day = 1
                            break;
                        case 'Wednesday':
                            day = 2
                            break;
                        case 'Thursday':
                            day = 3
                            break;
                        case 'Friday':
                            day = 4
                            break;
                        case 'Saturday':
                            day = 5
                            break;
                        case 'Sunday':
                            day = 6
                            break;
                    }
                    this.commitTimes[i][0] = times[0]
                    this.commitTimes[i][1] = day
                }
            },
            getTopRepos(){
                this.topRepos = this.commitData
                this.topRepos.sort(function(a, b) { return b[1] - a[1]; });
                console.log(this.topRepos)
                console.log(this.repoStatsUrl+this.topRepos[0][0])
            }
        }
    }
</script>
