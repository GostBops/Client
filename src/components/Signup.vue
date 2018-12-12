<template>
  <div>
    <b-form @submit="onSubmit" @reset="onReset">
      <b-form-group id="usernameGroup"
                    label="Your Name:"
                    label-for="username">
        <b-form-input id="username"
                      type="text"
                      v-model="form.username"
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
    </b-form>
    <div class="signin-tip">
      You have got an account?
      <router-link to="/signin">
        Sign in
      </router-link>
      now.
    </div>
  </div>
</template>

<script>
export default {
  name: 'signup',
  data () {
    return {
      form: {
        name: '',
        password: ''
      }
    }
  },
  methods: {
    onSubmit (evt) {
      evt.preventDefault();
    },
    onReset (evt) {
      evt.preventDefault();

    }
  }
}
</script>

<style>
.signin-tip {
  text-align: right;
}
</style>
