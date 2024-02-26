<script setup>
    import { ref } from 'vue'
    
    const name = ref('')
    const nameError = ref(false)
    const nameErrorMessage = ref('')

    const validateName = () => {
        if (name.value.length < 3) {
            nameError.value = true
            nameErrorMessage.value = 'Le nom doit contenir plus de 2 caractères'
        } else {
            nameError.value = false
            nameErrorMessage.value = ''
        }
    }
    
    // écouteur d'événements 
    const handleNameInput = () => {
        validateName()
    }

    const handleSubmit = () => {
        validateName()
    }
</script>

<template>
    <form @submit.prevent="handleSubmit">
      <div>
        <label>Nom</label>  
        <input
            type="text"
            v-model="name"
            @input="handleNameInput"
            class="form-control"
            placeholder="Votre nom...*"
            required="required"
            :class="{ 'is-invalid': nameError }"
        >
        <div v-if="nameError" class="invalid-feedback">{{ nameErrorMessage }}</div>

        </div>
      <div>
        <label>Prénom</label>  
        <input
          type="text"
          v-model="surname"
          class="form-control"
          placeholder="Votre prénom...*"
          required="required"
        >
      </div>
      <div>
        <label>Email</label>
        <input
          type="email"  
          v-model="email"
          class="form-control"
          placeholder="Votre adresse mail...*"
          required="required"
        >
      </div>
      <div>
        <label>Téléphone</label>
        <input
          type="tel"  
          v-model="email"
          class="form-control"
          placeholder="Votre numéro de téléphone..."
        >
      </div>
      <div>
        <label>Message</label>
        <textarea
          type="text"
          v-model="message"
          class="form-control"
          placeholder="Message"
          required="required"
        ></textarea>
      </div>
      <div>
        <button type="submit" class="btn">Envoyer</button>
      </div>
    </form>
  </template>