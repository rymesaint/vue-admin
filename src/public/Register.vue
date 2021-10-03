<template>
    <main class="form-signin">
        <form @submit.prevent="submit">
            <h1 class="h3 mb-3 fw-normal">Please Register</h1>

            <div class="form-floating">
                <input type="text" class="form-control" id="inputFirstName" placeholder="First Name" required v-model="firstName">
                <label for="inputFirstName">First Name</label>
            </div>

            <div class="form-floating">
                <input type="text" class="form-control" id="inputLastName" placeholder="Last Name" required v-model="lastName">
                <label for="inputLastName">Last Name</label>
            </div>

            <div class="form-floating">
                <input type="email" class="form-control" id="inputEmail" placeholder="E-mail address" required v-model="email">
                <label for="inputEmail">E-mail address</label>
            </div>

            <div class="form-floating">
                <input type="password" class="form-control" id="inputPassword" placeholder="Password" required v-model="password">
                <label for="inputPassword">Password</label>
            </div>

            <div class="form-floating">
                <input type="password" class="form-control" id="inputPasswordConfirm" placeholder="Password Confirmation" required v-model="passwordConfirmation">
                <label for="inputPasswordConfirm">Password Confirmation</label>
            </div>

            <button class="w-100 btn btn-lg btn-primary" type="submit">Register</button>
        </form>
    </main>
</template>

<script>
import {ref} from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

export default {
    name: "Register",
    setup() {
      const firstName = ref('');
      const lastName = ref('');
      const email = ref('');
      const password = ref('');
      const passwordConfirmation = ref('');
      const router = useRouter();

      const submit = async () => {
        console.log({
          first_name: firstName.value,
          last_name: lastName.value,
          email: email.value,
          password: password.value,
          passwordConfirmation: passwordConfirmation.value,
        })

        await axios.post('/register', {
          first_name: firstName.value,
          last_name: lastName.value,
          email: email.value,
          password: password.value,
          password_confirmation: passwordConfirmation.value,
          role_id: 1
        });

        await router.push('/login');
      }

      return {
        firstName,
        lastName,
        email,
        password,
        passwordConfirmation,
        submit
      }
    }
}
</script>

<style scoped>
html,
body {
  height: 100%;
}

body {
  display: flex;
  align-items: center;
  padding-top: 40px;
  padding-bottom: 40px;
  background-color: #f5f5f5;
}

.form-signin {
  width: 100%;
  max-width: 330px;
  padding: 15px;
  margin: auto;
}

.form-signin .checkbox {
  font-weight: 400;
}

.form-signin .form-floating:focus-within {
  z-index: 2;
}

.form-signin input[type="email"] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}

.form-signin input[type="password"] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
</style>