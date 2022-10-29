<script setup>
  import { supabase } from '../supabase'
  import { onMounted } from 'vue';

  const username = ref(data.username)
  const age = ref('')
  const avatar_url = ref('')
  const phone = ref('')
  const last_grade = ref('')

  onMounted(() => {
    getusername
  })

  async function getusername(){
    try {
        let { data, error, status } = await supabase
        .from('profiles')
        .select(`username, age, avatar_url, phone, last_grade`)
    if (error && status !== 406) throw error

    if(data){
        username.value = data.username
        age.value = data.age
        avatar_url.value = data.avatar_url
        phone.value = data.phone
        last_grade.value = data.last_grade
    }
    } catch (error) {
        alert(error.message)
    }
  }

</script>

<template>

</template>

<style scoped>

</style>