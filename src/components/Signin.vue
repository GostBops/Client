<template>
  <div>
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">
      <b-form-group id="usernameGroup"
                    label="Your Name:"
                    label-for="username">
        <b-form-input id="username"
                      type="text"
                      v-model="form.name"
                      required
                      placeholder="Enter name">
        </b-form-input>
      </b-form-group>
      <b-form-group id="passwordGroup"
                    label="Your Password:"
                    label-for="password">
        <b-form-input id="password"
                      type="password"
                      v-model="form.password"
                      required
                      placeholder="Enter password">
        </b-form-input>
      </b-form-group>
      <div class="signup-tip">
        You have not registered yet?
        <router-link to="/signup">
          Sign up
        </router-link>
        now.
      </div>
      <b-button-group class="my-btn-group">
        <b-button class="my-btn" type="reset" variant="outline-danger">Reset</b-button>
        <b-button class="my-btn" type="submit" variant="outline-primary">Signin</b-button> 
      </b-button-group>      
    </b-form>
  </div>
</template>

<script>
export default {
  name: 'signin',
  data () {
    return {
      form: {
        name: '',
        password: ''
      },
      show: true
    }
  },
  methods: {
    onSubmit (evt) {
      evt.preventDefault()
      /* Use Api */
    },
    onReset (evt) {
      evt.preventDefault()
      this.form.name = ''
      this.form.password = ''
      /* Trick to reset/clear native browser form validation state */
      this.show = false
      this.$nextTick(() => { this.show = true })
    }
  }
}
</script>

<style>
.signup-tip {
  text-align: right;
}

.my-btn-group {
  margin-top: 10px;
  width: 100%;
}

.my-btn {
  width: 50%;
}
</style>
