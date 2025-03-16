<script setup>
import { ref } from 'vue';
import AppHeader from '~/components/AppHeader.vue';

const form = ref({
    name: '',
    surname: '',
    username: '',
    email: '',
    password: '',
    password_confirmation: '',
})
const error = ref(null);
const success = ref(false);

const register = async () => {
    try {
        if (form.value.password !== form.value.password_confirmation) {
            error.value = 'Las contraseñas no coinciden';
            return;
        }
        const response = await $fetch('http://localhost:8000/api/register', {
            method: 'post',
            body: JSON.stringify(form.value),
            headers: {
                'Content-type': 'application/json',
            }
        })

        if (response.success) {
            success.value = true;
            error.value = null;
        } else {
            error.value = response.message || 'Error al registrar al usuario';
        }
    } catch (err) {
        error.value = 'Error de conexión con el servidor';
        console.error(err);
    }
}
</script>

<template>
    <AppHeader />
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-6">
                <form @submit.prevent='register'>
                    <div class="mb-3">
                        <label for="name" class="form-label">Nombre</label>
                        <input v-model="form.name" type="text" class="form-control" id="name" placeholder="Nombre">
                    </div>
                    <div class="mb-3">
                        <label for="surname" class="form-label">Apellidos</label>
                        <input v-model="form.surname" type="text" class="form-control" id="surname"
                            placeholder="Apellido1 Apellido2">
                    </div>
                    <div class="mb-3">
                        <label for="username" class="form-label">Nombre de usuario</label>
                        <input v-model="form.username" type="text" class="form-control" id="username"
                            placeholder="Username">
                    </div>
                    <div class="mb-3">
                        <label for="email" class="form-label">Correo electrónico</label>
                        <input v-model="form.email" type="email" class="form-control" id="email"
                            placeholder="email@example.com">
                    </div>
                    <div class="mb-3">
                        <label for="password" class="form-label">Contraseña</label>
                        <input v-model="form.password" type="text" class="form-control" id="password"
                            placeholder="Contraseña">
                    </div>
                    <div class="mb-3">
                        <label for="password_confirmation" class="form-label">Confirmación de la contraseña</label>
                        <input v-model="form.password_confirmation" type="text" class="form-control"
                            id="password_confirmation" placeholder="Contraseña">
                    </div>
                    <button type="submit" class="btn btn-primary">Registrarse</button>
                </form>

                <div v-if="error" class="alert alert-danger mt-3">
                    {{ error }}
                </div>
                <div v-if="success" class="alert alert-success mt-3">
                    ¡Registrado con éxito!
                </div>
            </div>
        </div>
    </div>
</template>