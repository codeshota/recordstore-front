<template>
  <div>
    <div>
      <h3>Sign Up</h3>
      <form @submit.prevent="signup">
        <div v-if="error">{{ error }}</div>

        <div>
          <label for="email">E-mail Address</label>
          <input type="email" v-model="email" id="email" placeholder="test@mail.com">
        </div>

        <div>
          <label for="password">Password</label>
          <input type="password" v-model="password" id="password" placeholder="Password">
        </div>

        <div>
          <label for="password_confirmation">Password Confirmation</label>
          <input type="password" v-model="password_confirmation" id="password_confirmation" placeholder="Password Confirmation">
        </div>
        <button type="submit">Sign Up</button>

        <div><router-link to="/">Sign in</router-link></div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Signup',
  data () {
    return {
      email: '',
      password: '',
      password_confirmation: '',
      error: ''
    }
  },
  created () {
    this.checkedSignedIn()
  },
  updated () {
    this.checkedSignedIn()
  },
  methods: {
    signup () {
      this.$http.plain.post('/signup', { email: this.email, password: this.password, password_confirmation: this.password_confirmation })
        .then(response => this.signupSuccessful(response))
        .catch(error => this.signupFailed(error))
    },
    signupSuccessful (response) {
      if (!response.data.csrf) {
        this.signupFailed(response)
        return
      }
      localStorage.csrf = response.data.csrf
      localStorage.signedIn = true
      this.error = ''
      this.$router.replace('/artists')
    },
    signupFailed (error) {
      this.error = (error.response && error.response.data && error.response.data.error) || 'Something went wrong'
      delete localStorage.csrf
      delete localStorage.signedIn
    },
    checkedSignedIn () {
      if (localStorage.signedIn) {
        this.$router.replace('/artists')
      }
    }
  }
}
</script>
export default {
  
}
</script>