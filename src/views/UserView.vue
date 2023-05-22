<template>
    <main>
        <div class="green">
            <li v-for="(user, index) in users" v-bind:key="index">{{user.username}}
            </li>            
        </div>

    </main>
</template>

<script lang="ts">
import axios from 'axios';
export default {
    data() {
        return {
            users: null
        };
    },
    methods: {
        findUsers() {
            axios.get('http://localhost:8080/api/users')
            .then((response) => {
                this.users = response.data.data;
                // console.log(this.users);
            })
            .catch((error) => {
                console.error(error);
            })
        }
    },
    mounted() {
        this.findUsers();
    },
};
</script>