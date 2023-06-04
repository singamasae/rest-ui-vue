<template>
     <main>
        <div class="form-group">
            <p>Username:</p>
            <input v-model="user.username" placeholder="" />
            <p>Password:</p>
            <input v-model="user.password" placeholder="" type="password"/>
            <p></p>
            <button class="btn-primary" type="submit"
                @click.prevent="login">Login</button>
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
            user: {
                username: '',
                password: '',
            }
        }
    },    
    methods: {
        login() {
            axios.post('http://localhost:8080/auth/token', this.user)
            .then(response => {
                this.user.username = '';
                this.user.password = '';
                localStorage.setItem('user', JSON.stringify(response.data.data));    
                localStorage.setItem('access_token', response.data.access_token);
                console.log(response);
                alert('Login successfully');            
            })
            .catch((error) => {
                alert('Login failed');
            })
        }
    }
}
</script>