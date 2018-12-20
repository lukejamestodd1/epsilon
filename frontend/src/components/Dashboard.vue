/* eslint-disable */
<template> 
    <div class="mt1">    
        <router-link :to="{ name: 'Dashboard'}">Dashboard</router-link>    
        <router-link :to="{ name: 'Login'}">Login</router-link> 
        <a href="#" v-on:click="logout">Logout</a>  
        <h2>Dashboard</h2>    
        <p>Welcome back to your dashboard, {{ user.name }}!</p>  
        <div id="companies" class="pb1">
            <h2 class="mt1">Your Clients</h2> 
            <div v-for="company in companies">
                <span class="companyName">{{ company.name }} ({{company.status}})</span>
                <label class="switch">
                    <input type="checkbox">
                    <span class="slider"></span>
                </label>
            </div>
        </div>
    </div>
</template>  
<script>  
    import axios from "axios"    
    import router from "../router"    
    export default {    
        name: "Login", 
          
        data() {    
            return {    
                user: {    
                    name: 'Jesse'    
                },
                companies: []
            }    
        },    
        methods: {    
            getUserData: function() {    
                let self = this    
                axios.get("/api/user")    
                    .then((response) => {    
                        console.log(response)    
                        self.$set(this, "user", response.data.user)    
                    })    
                    .catch((errors) => {    
                        console.log(errors)    
                        router.push("/")    
                    })    
            },
            getCompanies: function() {    
                let self = this    
                axios.get("/api/companies")    
                    .then((response) => {    
                        console.log(response.data.companies);
                        // this.companies = response.data.companies;
                        self.$set(this, "companies", response.data.companies)    
                    })    
                    .catch((errors) => {    
                        console.log(errors)    
                        router.push("/")    
                    })    
            },
            logout: function (e) {  
                axios
                .get("/api/logout")
                .then(() => {
                    router.push("/")
                })
            }   
        },    
        mounted() {    
            this.getUserData();
            this.getCompanies();
            console.log('FE has received companies' + this.companies);
        }    
    }
</script>

<style scoped>
#companies{
    border: solid black 7px;
    width: 30em;
    margin: 0 auto;
}
.companyName{
    vertical-align: text-bottom;
}
.switch {
  position: relative;
  display: inline-block;
  width: 30px;
  height: 17px;
}
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 13px;
  width: 13px;
  left: 2px;
  bottom: 2px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider {
  background-color: #2196F3;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196F3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(13px);
  -ms-transform: translateX(13px);
  transform: translateX(13px);
}

</style>
