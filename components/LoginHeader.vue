<template>
    <v-layout>
         <v-toolbar color="transparent" class="blue--text" flat fixed app :clipped-left="clipped">
      <v-toolbar-side-icon class="hidden-lg-and-up" @click="drawer = !drawer"></v-toolbar-side-icon>
      
      <v-toolbar-title v-text="title"></v-toolbar-title>
      
      <v-spacer></v-spacer>
      <v-btn @click="dialog = true" class="mb-5 login pink lighten-3 white--text">Login </v-btn>
      
    </v-toolbar>
      <!-- <p class="text-xs-center">{{result}}</p>  -->
        <v-dialog v-model="dialog"  max-width="500px">
     
        <v-card class="modal">
          
         <v-container>
        <v-text-field
        v-model="username"
          required
          solo
          label="User name"
        ></v-text-field>
        <v-text-field
        v-model="password"
         :type="show ? 'text' : 'password'"
          required
          solo
          label="Password"
        ></v-text-field>

       
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn @click="postPost()" class="pink lighten-3 white--text" >Login</v-btn>
          </v-card-actions>
            </v-container>
        </v-card>
      </v-dialog>
          <v-navigation-drawer
      :mini-variant.sync="miniVariant"
      :clipped="clipped"
      v-model="drawer"
      fixed
      app
    >
      <v-list>
        <v-list-tile
          router
          :to="item.to"
          :key="i"
          v-for="(item, i) in items"
          exact
        >
          <v-list-tile-action>
            <v-icon v-html="item.icon"></v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title v-text="item.title"></v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
 
    </v-layout>
</template>
<script>
import axios from 'axios';
  export default {
    name: 'nav-menu',
    data () {
      return {
        result: [],
        username:'',
        password:'',
        show: false,
        dialog: false,
        clipped: false,
        drawer: false,
        fixed: false,
        items: [
          { icon: 'apps', title: 'Welcome', to: '/' },
          { icon: 'bubble_chart', title: 'Inspire', to: '/inspire' }
        ],
        miniVariant: false,
        right: true,
        rightDrawer: false,
        title: 'Spanky WAP'
      }
    },
    methods: {
 postPost() {

      return axios.post(process.env.login, {
      email: this.username,
      password: this.password,
      private: process.env.private

    })
    .then(
    
      response => {
      this.result = response.data;
      
    })
    .catch(e => {
      this.errors.push(e)
    })

    }
    }
    
  }
</script>
<style scoped>
.login {
  margin-top: 50px;
}
.v-btn {
    color: dimgray;
}
.modal {
background: rgb(109,150,154);
background: radial-gradient(circle, rgba(109,150,154,1) 0%, rgba(156,165,172,1) 92%);
}
</style>

