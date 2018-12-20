/* eslint-disable */
<template>  
    <div>    
        <h2>Dashboard</h2>    
        <p>Welcome back to your dashboard {{ user.name }}!</p> 
        <h2>Companies</h2> 
        <p>Company 1 name: {{ companies[1].name }}</p>  
        <ul id="companies">
            <li v-for="company in companies">
                {{ company.name }}, {{company.status}}
            </li>
        </ul>
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
    list-style: none;
}
</style>
