<script setup>
  import { supabase } from '../supabase'
  import { onMounted, ref, toRefs } from 'vue'
  import Avatar from './Avatar.vue'

  const props = defineProps(['session'])
  const { session } = toRefs(props)

  const loading = ref(true)
  const username = ref('')
  const age = ref('')
  const avatar_url = ref('')
  const phone = ref('')
  const last_grade = ref('')

  onMounted(() => {
    getProfile()
  })

  async function getProfile() {
    try {
      loading.value = true
      const { user } = session.value

      let { data, error, status } = await supabase
        .from('profiles')
        .select(`username, age, avatar_url, phone, last_grade`)
        .eq('id', user.id)
        .single()

      if (error && status !== 406) throw error

      if (data) {
        username.value = data.username
        age.value = data.age
        avatar_url.value = data.avatar_url
        phone.value = data.phone
        last_grade.value = data.last_grade
      }
    } catch (error) {
      alert(error.message)
    } finally {
      loading.value = false
    }
  }

  async function updateProfile() {
    try {
      loading.value = true
      const { user } = session.value

      const updates = {
        id: user.id,
        username: username.value,
        age: age.value,
        avatar_url: avatar_url.value,
        phone:phone.value,
        last_grade:last_grade.value,
        updated_at: new Date(),
      }

      let { error } = await supabase.from('profiles').upsert(updates)

      if (error) throw error
    } catch (error) {
      alert(error.message)
    } finally {
      loading.value = false
    }
  }

  async function signOut() {
    try {
      loading.value = true
      let { error } = await supabase.auth.signOut()
      if (error) throw error
    } catch (error) {
      alert(error.message)
    } finally {
      loading.value = false
    }
  }
</script>

<template>
  <form class="form-widget" @submit.prevent="updateProfile">
    <Avatar v-model:path="avatar_url" size="25"/>
    &nbsp;
    <div>
      <label for="email" id="email-text">Email</label>
      <input id="email" type="text" :value="session.user.email" disabled />
    </div>

    <div>
      <label for="username" id="name-text">Full Name</label>
      <input id="username" type="text" v-model="username" />
    </div>

    <div>
      <label for="phone" id="phone-text">Phone Number</label>
      <input id="phone" type="phone" v-model="phone" />
    </div>

    <div>
      <label for="age" id="age-text">Age</label>
      <input id="age" type="age" v-model="age" />
    </div>

    <div>
      <label for="last_grade" id="last-grade-text">Last Grade</label>
      <input id="last-grade" type="last_grade" v-model="last_grade" />
    </div>

    <div>
      <input
        type="submit"
        class="button primary block"
        id="update-button"
        :value="loading ? 'Loading ...' : 'Update'"
        :disabled="loading"
      />
    </div>

    <div>
      <button class="button block" id="signout-button" @click="signOut" :disabled="loading">
        Sign Out
      </button>
    </div>   
  </form>
</template>

<style scoped>
#email-text{
  margin: 5px;
  color: #00ABB3;
  text-align: center;
  font-family:monospace;
  font-size: large;
} 
#name-text{
  margin: 5px;
  color: #00ABB3;
  text-align: center;
  font-family:monospace;
  font-size: large;
} 
#age-text{
  margin: 5px;
  color: #00ABB3;
  text-align: center;
  font-family:monospace;
  font-size: large;
} 
#phone-text{
  margin: 5px;
  color: #00ABB3;
  text-align: center;
  font-family:monospace;
  font-size: large;
}
#last-grade-text{
  margin: 5px;
  color: #00ABB3;
  text-align: center;
  font-family:monospace;
  font-size: large;
}

#email{
  margin: 5px;
  height: 40px;
  width: 100%;
  color:tomato;
  border-color: black;
  border-radius: 10px;
  text-align: center;
  font-family:monospace;
  font-weight: bold;
  font-size: large;
}
#username{
  margin: 5px;
  height: 40px;
  width: 100%;
  border-radius: 10px;
  border-color: black;
  background-color: white;
  color:tomato;
  text-align: center;
  font-family:monospace;
  font-weight: bold;
  font-size: large;
}
#age{
  margin: 5px;
  height: 40px;
  width: 100%;
  border-radius: 10px;
  border-color: black;
  background-color: white;
  color:tomato;
  text-align: center;
  font-family:monospace;
  font-weight: bold;
  font-size: large;
}
#phone{
  margin: 5px;
  height: 40px;
  width: 100%;
  border-radius: 10px;
  border-color: black;
  background-color: white;
  color:tomato;
  text-align: center;
  font-family:monospace;
  font-weight: bold;
  font-size: large;
}
#last-grade{
  margin: 5px;
  height: 40px;
  width: 100%;
  border-radius: 10px;
  border-color: black;
  background-color: white;
  color:tomato;
  text-align: center;
  font-family:monospace;
  font-weight: bold;
  font-size: large;
}

#update-button{
  margin: 5px;
  height: 45px;
  width: 100%;
  border-radius: 10px;
  background-color: #3ddc84;
  color: black;
  font-family:monospace;
  font-weight: bold;
  font-size: large;
}
#update-button:hover {
 background-color: green;
  -moz-transition: all 0.2s ease-in;
  -o-transition: all 0.2s ease-in;
  -webkit-transition: all 0.2s ease-in;
  transition: all 0.2s ease-in;
}

#signout-button{
  margin: 5px;
  height: 45px;
  width: 100%;
  border-radius: 10px;
  background-color: tomato;
  color: black;
  font-family:monospace;
  font-weight: bold;
  font-size: large;
}
#signout-button:hover {
 background-color: orangered;
  -moz-transition: all 0.2s ease-in;
  -o-transition: all 0.2s ease-in;
  -webkit-transition: all 0.2s ease-in;
  transition: all 0.2s ease-in;
}
</style>