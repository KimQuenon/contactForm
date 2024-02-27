<script setup>
    import { ref } from 'vue'
    import VueHcaptcha from '@hcaptcha/vue3-hcaptcha';

    const successMessage = ref('')

    const fields = ref({
      name: { value: '', error: false, errorMessage: '' },
      surname: { value: '', error: false, errorMessage: '' },
      email: { value: '', error: false, errorMessage: '' },
      phone: { value: '', error: false, errorMessage: '' },
      message: { value: '', error: false, errorMessage: '' },
    })

    const validateField = (fieldName, errorMessage, validator) => {
      if (!validator(fields.value[fieldName].value)) {
        fields.value[fieldName].error = true
        fields.value[fieldName].errorMessage = errorMessage
      } else {
        fields.value[fieldName].error = false
        fields.value[fieldName].errorMessage = ''
      }
    }

    const handleInputName = () => {
      validateField('name', 'Le champs doit comporter plus de 2 caractères', (value) => value.length >= 3)
    }
  
    const handleInputSurname = () => {
      validateField('surname', 'Le champs doit comporter plus de 1 caractère', (value) => value.length >= 2)
    }

    const handleInputEmail = () => {
      validateField('email', 'L\'adresse email est invalide', (value) => validateEmail(value))
    }

    const handleInputPhone = () => {
      validateField('phone', 'Le numéro de téléphone est invalide', (value) => validatePhone(value))
    }

    const handleInputMessage = () => {
      validateField('message', 'Votre message doit comporter plus de 50 caractères', (value) => value.length >= 51)
    }

    const validateEmail = (email) => {
      const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
      return re.test(String(email).toLowerCase())
    }

    const validatePhone = (phone) => {
      const re = /^(\+\d{1,3}\s?)?((\(\d{1,3}\))|\d{1,3})[-.\s]?\d{2,4}[-.\s]?\d{2,4}[-.\s]?\d{2,4}$/;
      return re.test(String(phone)) && phone.length <= 15;
    }
  

    const handleSubmit = () => {
      handleInputName()
      handleInputSurname()
      handleInputEmail()
      handleInputPhone()
      handleInputMessage()

      console.log('Données du formulaire:', {
        name: fields.value.name.value,
        surname: fields.value.surname.value,
        email: fields.value.email.value,
        phone: fields.value.phone.value,
        message: fields.value.message.value
      })

      if (Object.values(fields.value).every(field => !field.error)) {

          successMessage.value = 'Votre message a bien été envoyé.'

          Object.keys(fields.value).forEach(field => {
            fields.value[field].value = ''
          })
      }

    }


</script>

<template>
  <div class="container">
    <form @submit.prevent="handleSubmit">
      <div v-if="successMessage" class="alert alert-success mt-3">
        {{ successMessage }}
      </div>
      <div class="mb-2">
        <label>Nom<span class="text-danger">*</span></label>  
        <input
            type="text"
            v-model="fields.name.value"
            @input="handleInputName"
            class="form-control"
            placeholder="Votre nom..."
            required="required"
            :class="{ 'is-invalid': fields.name.error }"
        >
        <div v-if="fields.name.error" class="invalid-feedback">{{ fields.name.errorMessage }}</div>
      </div>
      <div class="mb-2">
        <label>Prénom<span class="text-danger">*</span></label>  
        <input
            type="text"
            v-model="fields.surname.value"
            @input="handleInputSurname"
            class="form-control"
            placeholder="Votre prénom..."
            required="required"
            :class="{ 'is-invalid': fields.surname.error }"
        >
        <div v-if="fields.surname.error" class="invalid-feedback">{{ fields.surname.errorMessage }}</div>
      </div>
      <div class="mb-2">
        <label>Email<span class="text-danger">*</span></label>
        <input
          type="email"  
          v-model="fields.email.value"
          @input="handleInputEmail"
          class="form-control"
          placeholder="Votre adresse mail..."
          required="required"
          :class="{ 'is-invalid': fields.email.error }"
        >
        <div v-if="fields.email.error" class="invalid-feedback">{{ fields.email.errorMessage }}</div>
      </div>
      <div class="mb-2">
        <label>Téléphone<span class="text-danger">*</span></label>
        <input
          type="tel"  
          v-model="fields.phone.value"
          @input="handleInputPhone"
          class="form-control"
          placeholder="Votre numéro de téléphone..."
          required="required"
          :class = "{ 'is-invalid': fields.phone.error }"
        >
        <div v-if="fields.phone.error" class="invalid-feedback">{{ fields.phone.errorMessage }}</div>
      </div>
      <div class="mb-2">
        <label>Message<span class="text-danger">*</span></label>
        <textarea
          type="text"
          v-model="fields.message.value"
          @input="handleInputMessage"
          class="form-control"
          placeholder="Tapez votre message..."
          required="required"
          :class = "{ 'is-invalid': fields.message.error }"
        ></textarea>
        <div v-if="fields.message.error" class="invalid-feedback">{{ fields.message.errorMessage }}</div>
      </div>
      <div>
        <p>(<span class="text-danger">*</span>) Ce champ est obligatoire.</p>
      </div>
      <vue-hcaptcha sitekey="6Lc_lYEpAAAAAPMwCGNdtfqXD9rxQVXqQjEEYtzH"></vue-hcaptcha>
      <div>
        <button type="submit" class="btn btn-primary mt-2">Envoyer</button>
      </div>
    </form>
  </div>
  </template>