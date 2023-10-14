<template>
    <Form ref="form" class="newsletter__form" @submit="handleSubmit" v-slot="{ errors }">
        <div class="newsletter__inputs-container">
            <div>
                <label class="newsletter__input-label" for="name">NOMBRE</label>
                <Field :disabled="isDisabledInput" name="name" type="text" v-model="form.firstname"
                    class="newsletter__input" :class="{ 'error__input': errors.name }" :rules="validateName" />
                <span class="error">
                    <ErrorMessage name="name" />
                </span>
            </div>

            <div>
                <label class="newsletter__input-label" for="lastname">APELLIDO</label>
                <Field :disabled="isDisabledInput" name="lastname" type="text" v-model="form.lastname"
                    class="newsletter__input" :class="{ 'error__input': errors.lastname }" :rules="validateLastName" />
                <span class="error">
                    <ErrorMessage name="lastname" />
                </span>
            </div>

            <div>
                <label class="newsletter__input-label" for="mail">MAIL</label>
                <Field :disabled="isDisabledInput" name="email" type="email" v-model="form.email" class="newsletter__input"
                    :class="{ 'error__input': errors.email }" :rules="validateEmail" />
                <span class="error">
                    <ErrorMessage name="email" />
                </span>
            </div>

            <div>
                <label class="newsletter__input-label" for="phone">téléfono</label>
                <Field :disabled="isDisabledInput" name="phone" type="tel" v-model="form.phone" class="newsletter__input"
                    :class="{ 'error__input': errors.phone }" :rules="validatePhone" />
                <span class="error">
                    <ErrorMessage name="phone" />
                </span>
            </div>
        </div>

        <button :disabled="isDisabledButton || errors.name || errors.lastname || errors.email || errors.phone"
            class="newsletter__form-button">
            ENVIAR
        </button>
    </Form>
</template>

<script>
import axios from 'axios'
import Swal from 'sweetalert2'
import { Form, Field, ErrorMessage } from 'vee-validate';


export default {
    components: {
        Form,
        Field,
        ErrorMessage,
    },
    data() {
        return {
            form: {
                firstname: '',
                lastname: '',
                email: '',
                phone: ''
            },
            isDisabledButton: false,
            isDisabledInput: false
        }
    },
    methods: {
        handleSubmit() {
            this.isDisabledButton = true
            this.isDisabledInput = true
            axios.post('https://5eed24da4cbc340016330f0d.mockapi.io/api/newsletter', this.form)
                .then(response => {
                    Swal.fire({
                        icon: 'success',
                        title: '¡Gracias!',
                        text: 'Tu información ha sido enviada',
                    })
                }).catch(error => {
                    Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: 'Hubo un error, intenta de nuevo',
                    })
                }).finally(() => {
                    this.isDisabledButton = false
                    this.isDisabledInput = false
                    this.form = {
                        firstname: '',
                        lastname: '',
                        email: '',
                        phone: ''
                    }
                })
        },
        validateEmail(value) {
            // if the field is empty
            if (!value) {
                return 'Este campo es requerido';
            }
            // if the field is not a valid email
            const regex = /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i;
            if (!regex.test(value)) {
                return 'Debe ser un correo valido';
            }
            // All is good
            return true;
        },
        validatePhone(value) {
            if (value.length > 10) {
                return 'Debe ser un número entre 7 y 10 dígitos';
            }
            // if the field is empty
            if (!value) {
                return 'Este campo es requerido';
            }
            //validar si el numero de telefono esta entre 7 y 10 digitos
            const regex = /^[0-9]{7,10}$/;

            if (!regex.test(value)) {
                return 'Debe ser un número de teléfono valido';
            }
            // All is good
            return true;
        },
        validateName(value) {
            // if the field is empty
            if (!value) {
                return 'Este campo es requerido';
            }
            // if the field is not a valid name
            const regex = /^[a-zA-ZÀ-ÿ\s]{1,40}$/;
            if (!regex.test(value)) {
                return 'Debe ser un nombre valido';
            }
            // All is good
            return true;
        },
        validateLastName(value) {
            // if the field is empty
            if (!value) {
                return 'Este campo es requerido';
            }
            // if the field is not a valid lastname
            const regex = /^[a-zA-ZÀ-ÿ\s]{1,40}$/;
            if (!regex.test(value)) {
                return 'Debe ser un apellido valido';
            }
            // All is good
            return true;
        }

    }
}

</script>