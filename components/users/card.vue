<template>
    <div v-if="user" class="users__card">
        <div class="users__card-color" :style="`box-shadow: inset 0 0 0 3000px ` + user.color + `;`"></div>
        <div class="users__card-name"> {{ user.name }}</div>
        <div class="users__card-balance"> Баланс: {{ user.balance }}</div>
        <div class="users__card-role"> Роль: {{ user.role }}</div>
        <div class="users__card-age"> Возраст: {{ user.age }}</div>
        <div class="users__card-blocked" >
            <NuxtIcon v-if="user.blocked" class="users__card-blocked-icon" name="blocked" filled />    
        </div>
    </div>
</template>
<script setup>
    import '~/components/users/__users.css';
    const { user,selectRole,selectColor,selectBlocked } = defineProps(['user','selectRole','selectColor','selectBlocked']);
</script>