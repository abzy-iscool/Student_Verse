<script setup>
  import { ref } from 'vue'
  import { supabase } from '../supabase'

  const loading = ref(false)
  const email = ref('')

  const handleLogin = async () => {
    try {
      loading.value = true
      const { error } = await supabase.auth.signInWithOtp({
        email: email.value,
      })
      if (error) throw error
      alert('Check your email for the login link!')
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      loading.value = false
    }
  }
</script>

<template>
  <form class="row flex-center flex" id="email-validation-form" @submit.prevent="handleLogin">
    <div class="col-6 form-widget" id="email-validation-items">
      <h1 class="header">Welcome to the Student-Verse</h1>
      <p class="description">Sign in via magic link with your email below</p>

      <div>
        <input
          type="email"
          class="inputField"
          id="email-field"
          placeholder="Your email"
          v-model="email"
        />
      </div>
      
      <div>
        <input
          type="submit"
          id="submit-button"
          class="button block"
          :value="loading ? 'Loading' : 'Send magic link'"
          :disabled="loading"
        />
      </div>
    </div>
  </form>

</template>

<style scoped>

.email-validation-form{
  display: grid;
  place-items: center;
}
.header{
  color:transparent;
  text-align: center;
  font-family:monospace;
  font-weight: bold;
  animation: header-animation ease-in-out 5s infinite alternate;
}
@keyframes header-animation {
  0%   {color: #FF8B01;}
  25%  {color: #FA6F01;}
  50%  {color: #F55301;}
  75%  {color: #F03801;}
  100% {color: orangered;}
}
.description{
  color:#00ABB3;
  text-align: center;
  font-family:monospace;
  font-weight: bold;
  font-size: larger;
}

#submit-button{
  display: grid;
  margin: 5px;
  height: 40px;
  width: 100%;
  border-radius: 10px;
  border-color: black;
  background-color: #3ddc84;
  color: black;
  font-family:monospace;
  font-weight: bold;
  font-size: large;
}
#submit-button:hover {
 background-color: green;
  -moz-transition: all 0.2s ease-in;
  -o-transition: all 0.2s ease-in;
  -webkit-transition: all 0.2s ease-in;
  transition: all 0.2s ease-in;
}

#email-field{
  display: grid;
  margin: 5px;
  height: 40px;
  width: 100%;
  border-radius: 10px;
  border-color: black;
  background-color: white;
  color: black;
  font-family:monospace;
  font-size: large;

}
</style>
