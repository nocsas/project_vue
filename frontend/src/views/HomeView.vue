<template>
    <div class="HomeView" style="margin-top:80px">
        <div class="container">
            <div class="row">
                <div class="col-md-4">
                    <div class="card">
                        <div class="card-body">
                            MAIN MENU
                            <hr>
                            <ul class="list-group">
                                <router-link :to="{name: 'HomeView'}" class="list-group-item text-dark text-decoration-none">DASHBOARD</router-link>
                                <li @click="logout" class="list-group-item text-dark text-decoration-none" style="cursor:pointer">LOGOUT</li>
                            </ul>
                        </div>
                    </div>
                </div>
                <div class="col-md-8">
                    <div class="card">
                        <div class="card-body">
                            DASHBOARD
                            <hr>
                            Selamat Datang <strong>{{ user.name }}</strong>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import axios from 'axios'

export default {
    name: 'HomeView',

    data() {
        return {
            //state loggedIn with localStorage
            loggedIn: localStorage.getItem('loggedIn'),
            //state token
            token: localStorage.getItem('token'),
            //state user logged In
            user: []
        }
    },

    created() {
        axios.get('http://localhost:8000/api/user', {headers: {'Authorization': 'Bearer '+this.token}})
        .then(response => {
            console.log(response)
            this.user = response.data // assign response to state user
        })
    },

    methods: {
        logout() {
            axios.get('http://localhost:8000/api/logout')
            .then(() => {
                //remove localStorage
                localStorage.removeItem("loggedIn")    


                //redirect
                return this.$router.push({ name: 'login' })
            })
        }
    },

    //check user logged in or not
    mounted() {
        if(!this.loggedIn) {
            return this.$router.push({ name: 'login' })
        }
    }
}
</script>