<template>
    <main>
        <div class="form-group">
            <p>Username:</p>
            <input v-model="user.username" placeholder="" />
            <p>Password:</p>
            <input v-model="user.password" placeholder="" type="password"/>
            <p>Full Name:</p>
            <input v-model="user.name" placeholder="" />
            <p>Address:</p>
            <input v-model="user.address" placeholder="" />
            <p></p>
            <button class="btn-primary" type="submit"
                @click.prevent="register">Submit</button>

        </div>
        <br/>
        <div >
            <div class="green">
                <li v-for="(user, index) in users" v-bind:key="index">{{user.username}} | {{user.name}} | {{user.address}} | 
                    [
                        <a href="#" @click.prevent="remove(user.id)" style="color: red;">Delete</a> :: 
                        <a href="#" @click.prevent="edit(user)" style="color: blue;">Edit</a>
                    ]
                </li>            
            </div>
        </div>
        

    </main>
</template>

<style scoped>
input {
    margin-bottom: 8px;    
}

.btn-primary {
    margin: 10px;
    font-size: 14px;
    width: 75px;
    height: 35px;
    padding: 5px;
    background: hsla(160, 100%, 37%, 0.2);
    color: #fff;
    outline: none;
    border-radius: 4px;
    border: 1px solid transparent;
    transition: 0.5s;
}

.btn-primary:hover {
    background: green;
    color: #fff;
    transition: 0.5s;
}

</style>

<script lang="ts">
import axios from 'axios';
export default {
    data() {
        return {
            users: null,
            user: {
                username: '',
                password: '',
                name: '',
                address: ''
            },
            idUser: null            
        };
    },
    methods: {
        findUsers() {
            const config = {
                headers:{
                    Authorization: `Bearer ${localStorage.getItem('access_token')}`,                    
                }
            };

            axios.get('http://localhost:8080/api/users', config)
            .then((response) => {
                this.users = response.data.data;
                // console.log(this.users);
            })
            .catch((error) => {
                console.error(error);
            })
        },
        register() {
            console.log(this.user);
            if (this.idUser == null) {
                axios.post('http://localhost:8080/api/user', this.user)
                .then(response => {
                    this.user.username = '';
                    this.user.password = '';
                    this.user.name = '';
                    this.user.address = '';
                    console.log(response);
                    this.findUsers();
                    this.idUser = null;
                })
                .catch((error) => {
                    console.error(error);
                })
            } else {                
                axios.put('http://localhost:8080/api/user/' + this.idUser, this.user)
                .then(response => {   
                    this.user.username = '';                 
                    this.user.password = '';
                    this.user.name = '';
                    this.user.address = '';
                    console.log(response);
                    this.findUsers();
                    this.idUser = null;
                })
                .catch((error) => {
                    console.error(error);
                })
            }
            
        },
        remove(id: number) {
            console.log(id);
            axios.delete('http://localhost:8080/api/user/' + id)
            .then((response) => {
                console.log(response);
                this.findUsers();                
            })
            .catch((error) => {
                console.error(error);
            })
        },
        edit(data: any) {
            console.log(data);
            this.user.username = data.username;
            this.user.password = data.password;
            this.user.name = data.name;
            this.user.address = data.address;
            this.idUser = data.id;
        }        
    },
    mounted() {
        this.findUsers();
    },
};
</script>