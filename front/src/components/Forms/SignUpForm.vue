<template>
  <ValidationObserver ref="observer" v-slot="{ handleSubmit }">
    <b-form @submit.prevent="handleSubmit(onSubmit)" class="form">
      <TextInputWithValidation
        name="E-mail"
        id="email"
        rules="required|email"
        label="E-mail:"
        v-model="email"
      />

      <TextInputWithValidation
        name="Username"
        id="username"
        rules="required"
        label="Username:"
        v-model="username"
      />

      <TextInputWithValidation
        name="Password"
        id="password"
        rules="required"
        label="Password:"
        type="password"
        v-model="password"
      />

      <TextInputWithValidation
        name="Password Confirm"
        id="passwordConfirm"
        rules="required|confirmed:password"
        label="Password Confirm:"
        type="password"
        v-model="passwordConfirm"
      />

      <b-button-group class="w-100">
        <b-button type="submit" variant="outline-primary">Sign In</b-button>
        <b-button variant="outline-danger" @click="handleClickCancel"
          >Cancel</b-button
        ></b-button-group
      >
    </b-form>
  </ValidationObserver>
</template>

<script>
import { ValidationObserver } from 'vee-validate';
import TextInputWithValidation from '../FormItems/TextInputWithValidation.vue';
import { registration } from '../../services/userService';

export default {
  components: {
    ValidationObserver,
    TextInputWithValidation,
  },
  data() {
    return {
      email: '',
      username: '',
      password: '',
      passwordConfirm: '',
    };
  },
  methods: {
    async onSubmit() {
      const { email, username, password } = this;

      try {
        await registration({ email, username, password });
        this.$root.$bvToast.toast('Welcome!', {
          title: 'SUCCESS',
          variant: 'success',
          solid: true,
        });
        this.$router.push('/');
      } catch (e) {
        this.$root.$bvToast.toast(e.response.status, {
          title: 'DANGER',
          variant: 'danger',
          solid: true,
        });
      }
    },
    handleClickCancel() {
      this.$router.push('/');
    },
  },
};
</script>

<style scoped>
.form {
  min-width: 300px;
}
</style>
