<template>
  <v-app>
    
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>
   
 
  </v-app>
</template>

<script>
  export default {
    data() {
      return {
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
        title: 'Funny Pal'
      }
    }
  }
</script>

<style scoped>
.v-content{
  background: rgb(34,193,195);
  background: linear-gradient(142deg, rgba(34,193,195,0.5704656862745099) 0%, rgba(253,197,45,0.30996148459383754) 100%);
}
</style>
