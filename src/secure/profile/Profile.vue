<template>
    <h2>Account Information</h2>
    <hr>
    <form @submit.prevent="submitInfo">
        <div class="form-group">
            <label>First Name</label>
            <input type="text" class="form-control" name="first_name" v-model="firstName">
        </div>
        <div>&nbsp;</div>
         <div class="form-group">
            <label>Last Name</label>
            <input type="text" class="form-control" name="last_name" v-model="lastName">
        </div>
        <div>&nbsp;</div>
         <div class="form-group">
            <label>E-mail</label>
            <input type="text" class="form-control" name="email" v-model="email">
        </div>
        <div>&nbsp;</div>
        <button class="btn btn-outline-secondary">Save</button>
    </form>

    <h2>Change Password</h2>
    <hr>
    <form @submit.prevent="submitPassword">
        <div class="form-group">
            <label>Password</label>
            <input type="password" class="form-control" name="password" v-model="password">
        </div>
        <div class="form-group">
            <label>Password Confirmation</label>
            <input type="password" class="form-control" name="password_confirmation" v-model="passwordConfirmation">
        </div>
        <div>&nbsp;</div>
        <button class="btn btn-outline-secondary">Save</button>
    </form>
</template>

<script lang="ts">
import {ref, onMounted, computed} from 'vue';
import axios from 'axios';
import { User } from '@/classes/user';
import { useStore } from 'vuex';

export default {
    name: "Profile",
    setup() {
        const firstName = ref('');
        const lastName = ref('');
        const email = ref('');
        const password = ref('');
        const passwordConfirmation = ref('');
        const store = useStore();

        onMounted( async () => {
            const user = computed(() => store.state.User.user);

            firstName.value = user.value.first_name;
            lastName.value = user.value.last_name;
            email.value = user.value.email;
        });

        const submitInfo = async () => {
            const response = await axios.put('/users/info', {
                first_name: firstName.value,
                last_name: lastName.value,
                email: email.value,
            });

            const user: User = response.data;

            await store.dispatch('setUser', new User(
                user.id,
                user.first_name,
                user.last_name,
                user.email,
                user.role,
                user.permissions
            ));
        }

        const submitPassword = async () => {
            await axios.put('/users/password', {
                password: password.value,
                password_confirmation: passwordConfirmation.value,
            });

            password.value = '';
            passwordConfirmation.value = '';
        }

        return {
            firstName,
            lastName,
            email,
            password,
            passwordConfirmation,
            submitInfo,
            submitPassword
        }
    }
}
</script>