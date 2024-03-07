<template>
    <div class="users__list">
        <div class="users__list-sort">
            <div class="users__list-sort-color">
                <select v-model="selectColor">
                    <option value="all" selected >Все цвета</option>
                    <option  v-for="color in colors" :value="color" >{{ color }}</option>
                </select>
            </div>
            <div class="users__list-sort-blocked">
                <select v-model="selectBlocked">
                    <option value="all" selected >Блокировка</option>
                    <option v-for="block in blocked" :value="block">{{ block }}</option>
                </select>
            </div>
            <div class="users__list-sort-role">
                <select v-model="selectRole">
                    <option value="all" selected >Роль</option>
                    <option v-for="role in roles" :value="role">{{ role }}</option>
                </select>
            </div>
            <div class="users__list-sort-age">
                <label for="volume">Возраст</label>
                <div class="users__list-sort-age-inputs">
                    <div class="users__list-sort-age-input">
                        <span>
                            От
                        </span>
                        {{ minAge }}
                        <input v-model="minAge" type="number" /> 
                    </div>
                    <div class="users__list-sort-age-input">
                        <span>
                            До
                        </span>
                        {{ maxAge }}
                        <input v-model="maxAge" type="number" /> 
                    </div>
                </div>
            </div>
        </div>
        <UsersCard v-for="(user, index) in users" 
            :key="index" 
            :user="user"
            v-show="userShow(user) " />
    </div>
</template>
<script setup>
    const route = useRoute();
    const router = useRouter();
    const { users } = defineProps(['users']);
    const selectColor = ref(route.query.color ? route.query.color : 'all');
    const selectBlocked = ref(route.query.blocked ? route.query.blocked : 'all');
    const selectRole = ref(route.query.role ? route.query.role : 'all');
    const minAge = ref(route.query.minAge ? route.query.minAge : '');
    const maxAge = ref(route.query.maxAge ? route.query.maxAge : '');
    let colors = new Set();
    let blocked = new Set();
    let roles = new Set();
    onBeforeMount(
        function getSelectOption(){
            users.forEach(user =>{
                colors.add(user.color);
                blocked.add(user.blocked);
                roles.add(user.role);
            })
            colors = Array.from(colors)
            blocked = Array.from(blocked)
            roles = Array.from(roles)
        }

    )
    function userShow(user){
        let color,role,blocked,age
        if(route.query.color == user.color || selectColor.value == 'all'){
            color = true;
        }
        if(route.query.role == user.role || selectRole.value == 'all'){
            role = true
        }
        if(route.query.blocked == String(user.blocked) || selectBlocked.value == 'all'){
            blocked = true
        }
        if((route.query.minAge <= user.age || minAge.value == '') && (user.age <= route.query.maxAge || maxAge.value == '')){
            age = true
        }
        if(color && role && blocked && age){
            return true
        }
    }
    watch([selectColor, selectBlocked, selectRole, maxAge, minAge], ([selectColor, selectBlocked, selectRole, maxAge, minAge], previous) => {
      router.push({
        path: '',
        query: { color: selectColor,
                 blocked: selectBlocked,
                 role: selectRole,
                 minAge: minAge,
                 maxAge: maxAge },
      });
    })
</script>