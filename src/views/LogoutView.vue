<template>
    <main>
        <div class="logout">
            <h1>This is a logout page</h1>
        </div>
    </main>
</template>

<style>
@media (min-width: 1024px) {
  .logout {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>

<script lang="ts">
export default {
    methods: {
        logout() {
            localStorage.removeItem('user');
            localStorage.removeItem('access_token');
            console.log('clear local storage');
        }
    },
    mounted() {
        this.logout();
    },

}
</script>