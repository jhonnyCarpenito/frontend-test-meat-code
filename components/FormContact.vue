<template>
    <Form ref="form" class="newsletter_form" @submit="handleSubmit" v-slot="{ errors }">
        <div class="newsletter_inputs_container">
            <div>
                <label class="newsletter-input-text" for="name">NOMBRE</label>
                <Field name="name" type="text" v-model="form.firstname" class="newsletter_input"
                    :class="{ 'error_input': errors.name }" :rules="validateName" />
                <span class="error">
                    <ErrorMessage name="name" />
                </span>
            </div>
            <div>
                <label class="newsletter-input-text" for="lastname">APELLIDO</label>
                <Field name="lastname" type="text" v-model="form.lastname" class="newsletter_input"
                    :class="{ 'error_input': errors.lastname }" :rules="validateLastName" />
                <span class="error">
                    <ErrorMessage name="lastname" />
                </span>
            </div>

            <div>
                <label class="newsletter-input-text" for="mail">MAIL</label>
                <Field name="email" type="email" v-model="form.email" class="newsletter_input"
                    :class="{ 'error_input': errors.email }" :rules="validateEmail" />
                <span class="error">
                    <ErrorMessage name="email" />
                </span>
            </div>

            <div>
                <label class="newsletter-input-phone" for="phone">téléfono</label>
                <Field name="phone" type="tel" v-model="form.phone" class="newsletter_input"
                    :class="{ 'error_input': errors.phone }" :rules="validatePhone" />
                <span class="error">
                    <ErrorMessage name="phone" />
                </span>
            </div>
        </div>

        <button :disabled="isDisabledButton || errors.name || errors.lastname || errors.email || errors.phone"
            class="newsletter_form_button">
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
            isDisabledButton: false
        }
    },
    methods: {
        handleSubmit() {
            this.isDisabledButton = true
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
            // if the field is empty
            if (!value) {
                return 'Este campo es requerido';
            }
            // if the field is not a valid phone
            const regex = /^[0-9]{10}$/;
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