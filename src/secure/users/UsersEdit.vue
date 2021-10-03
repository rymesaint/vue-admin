<template>
    <form @submit.prevent="submit">
        <div>&nbsp;</div>
        <div class="form-group">
            <label for="first_name">First Name</label>
            <input type="text" class="form-control" id="first_name" v-model="firstName">
        </div>
        <div>&nbsp;</div>
        <div class="form-group">
            <label for="last_name">Last Name</label>
            <input type="text" class="form-control" id="last_name" v-model="lastName">
        </div>
        <div>&nbsp;</div>
        <div class="form-group">
            <label for="email">E-mail</label>
            <input type="email" class="form-control" id="email" v-model="email">
        </div>
        <div>&nbsp;</div>
        <div class="form-group">
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

<script lang="ts">
import {ref, onMounted} from 'vue';
import axios from 'axios';
import { useRoute, useRouter } from 'vue-router';
import {User} from '@/classes/user';

export default {
    name: 'UsersEdit',
    setup() {
        const firstName = ref('');
        const lastName = ref('');
        const email = ref('');
        const roleId = ref(0);
        const roles = ref([]);
        const router = useRouter();
        const {params} = useRoute();

        onMounted(async () => {
            const response = await axios.get('/roles');

            roles.value = response.data.data;

            const userCall = await axios.get(`/users/${params.id}`);

            const user: User = userCall.data.data;

            firstName.value = user.first_name;
            lastName.value = user.last_name;
            email.value = user.email;
            roleId.value = user.role.id;
        });

        const submit = async () => {
            await axios.put(`/users/${params.id}`, {
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