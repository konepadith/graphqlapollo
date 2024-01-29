<template>
 <v-app>
  <v-app-bar
      app
      color="white"
      flat
    >
      <v-container class="py-0 fill-height">
        <v-avatar
          class="mr-10"
          color="grey darken-1"
          size="32"
        ></v-avatar>

        <v-btn
          v-for="item in items"
          :key="item.to.name"
          :elevation="0"
          :to="item.to"
          class="ms-2"
        >
        {{ item.title }}
        </v-btn>

        <v-spacer></v-spacer>

        <v-responsive max-width="260">
          <v-text-field
            dense
            flat
            hide-details
            rounded
            solo-inverted
          ></v-text-field>
        </v-responsive>
      </v-container>
    </v-app-bar>

  <v-main>
    <v-container>
      <Nuxt />
    </v-container>
  </v-main>
</v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data(){
    return{
      appTitle: 'Awesome App',
      sidebar: false,
      items: [
        {
          icon:'mdi-apps',
          title:'Home',
          to: {name:'index', query:''}
        },
        {
          icon:'mdi-apps',
          title:'inspire',
          to: {name:'inspire', query:''}
        },
        {
          icon:'mdi-apps',
          title:'validation',
          to: {name:'validation', query:''}
        }
      ]
    }
  }
}
</script>
