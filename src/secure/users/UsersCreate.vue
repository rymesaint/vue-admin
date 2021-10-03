<template>
    <form @submit.prevent="submit">
        <div>&nbsp;</div>
        <div class="form-group form-floating">
            <label for="first_name">First Name</label>
            <input type="text" class="form-control" id="first_name" v-model="firstName">
        </div>
        <div>&nbsp;</div>
        <div class="form-group form-floating">
            <label for="last_name">Last Name</label>
            <input type="text" class="form-control" id="last_name" v-model="lastName">
        </div>
        <div>&nbsp;</div>
        <div class="form-group form-floating">
            <label for="email">E-mail</label>
            <input type="email" class="form-control" id="email" v-model="email">
        </div>
        <div>&nbsp;</div>
        <div class="form-group form-floating">
            <label for="role_id">Role</label>
            <select type="text" class="form-control" id="role_id" v-model="roleId">
                <option>Select Role</option>
                <option v-for="role in roles" :key="role.id" :value="role.id">
                    {{ role.name }}
                </option>
            </select>
        </div>
        <div>&nbsp;</div>
        <button class="btn btn-outline-secondary">Save</button>
    </form>
</template>

<script>
import {ref, onMounted} from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

export default {
    name: 'UsersCreate',
    setup() {
        const firstName = ref('');
        const lastName = ref('');
        const email = ref('');
        const roleId = ref(0);
        const roles = ref([]);
        const router = useRouter();

        onMounted(async () => {
            const response = await axios.get('/roles');

            roles.value = response.data.data;
        });

        const submit = async () => {
            await axios.post('/users', {
                first_name: firstName.value,
                last_name: lastName.value,
                email: email.value,
                role_id: roleId.value
            });

            await router.push('/users');
        }

        return {
            firstName,
            lastName,
            email,
            roleId,
            roles,
            submit
        }
    }
}
</script>

<style scoped>

</style>