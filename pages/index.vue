<template>
  <v-layout column justify-center align-center>
    <LoginHeader/>
  <div class="text-xs-center">
   
    
    <v-dialog
      v-model="spinner"
      persistent
      width="300"
    >
      <v-card
        class="spinner"
        dark
      >
        <v-card-text>
          Please stand by
          <v-progress-linear
            indeterminate
            color="white"
            class="mb-0"
          ></v-progress-linear>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
      <!-- <v-dialog class="blue" persistent v-model="spinner"  max-width="300px">
         <v-container>
          <div class="text-xs-center">
            <v-card-text>
              Please stand by
              <v-progress-linear
              :size="100"
              :width="7"
              color="blue"
              indeterminate
              fullscreen
            ></v-progress-linear>
            </v-card-text>
            
          </div>
        </v-container>
      </v-dialog> -->
    <v-flex xs12 sm12 md12 lg12>
       <div class="text-xs-center">
        <img width="40%" src="/patlogo.png" alt="Vuetify.js"/>
      </div>
       <v-card class="transparent text-xs-center" flat>
      <v-form v-model="valid" ref="form" lazy-validation  @submit.prevent="submit" >
        <v-text-field

        v-model="form.firstName"
          :rules="rules.name"
          required
          solo
          label="First Name"
        ></v-text-field>
        <v-text-field
        v-model="form.lastName"
          :rules="rules.name"
          required
          solo
          label="Last Name"
        ></v-text-field>
        <v-text-field
         v-model="form.contactNo"
          :rules="rules.contactNumber"
          required
          solo
          mask="####-###-####"
          counter="11"
          label="contact number ex.(0928*******)"
        ></v-text-field>
        <v-text-field
         v-model="form.username"
           :rules="rules.name"
          label="Username"
          required
          solo
        ></v-text-field>
        <v-text-field
        v-model="form.pass"
          :type="show ? 'text' : 'password'"
            :rules="rules.name"
          required
          solo
          label="Password"
        ></v-text-field>
        <v-btn
         @click="register"
          :disabled="disable"
          class="mb-5 light-blue darken-1 white--text"
        >
            Register
        </v-btn>

      </v-form>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from 'axios';
import LoginHeader from '~/components/LoginHeader.vue'
  import { validationMixin } from 'vuelidate'
  import { required, maxLength, email, minLength } from 'vuelidate/lib/validators'

export default {
  
  data () {
    const defaultForm = Object.freeze({
      firstName: '',
      lastName: '',
      contactNo: '',
      username: '',
      pass: '',
      })
    return {
      form: Object.assign({}, defaultForm),
      rules: {
         contactNumber: [
           v => (v && v.length == 11) || 'Must be a valid Philippines contact number'
           
           ],
           name: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 20) || 'Name must be less than 10 characters'
      ],
        },
      valid: false,
      show: false,
      disable: true,
      result: [],
      spinner: false,
      defaultForm
    }
  },
  components: {
   LoginHeader
  },
      computed: {
      formIsValid () {
        return (
          this.form.firstName.length <= 10 && 
          this.form.lastName.length <= 10 &&
          this.form.username.length <= 10 &&
          this.form.pass.length <= 10 &&
          this.form.contactNo.length == 11
        )
      }
    },
  methods: {
    movetoMain() {
        this.$router.push('inspire');
    },
    register() {
      this.spinner = true;
      if (this.$refs.form.validate()) {
      return axios.post(process.env.register, {
        first_name: this.form.firstName,
        last_name: this.form.lastName,
        contact_no: this.form.contactNo,
        email: this.form.username,
        password: this.form.pass
    })
      .then(
        response => {
          this.$router.push('inspire');
      })
      .catch(e => {
        console.log(this.result);
      })
      }
    }
  },
 updated() {
   if ( this.form.firstName.length <= 10 && this.form.firstName.length > 2 && 
          this.form.lastName.length <= 20 && this.form.lastName.length > 3 &&
          this.form.username.length <= 10 && this.form.username.length >= 7 &&
          this.form.pass.length <= 10 && this.form.pass.length > 5 &&
          this.form.contactNo.length == 11) {
            this.disable = false;
   }
   else {
          this.disable = true;
   }
 },
}
</script>


<style scoped>
.spinner {
  background: rgb(34,193,195);
  background: linear-gradient(142deg, rgba(34,193,195,0.5704656862745099) 0%, rgba(253,197,45,0.30996148459383754) 100%);
}
</style>

