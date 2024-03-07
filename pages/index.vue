<template>
    <div class="users">
        <UsersList :users="users" />
    </div>
</template>
<script setup>
    import usersList from './assets/users.json';
    const users = ref(usersList);
</script>