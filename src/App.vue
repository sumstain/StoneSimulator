<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <h2>LostArk Stone Simulator</h2>
      </div>

      <v-spacer></v-spacer>

    </v-app-bar>

    <v-main>
      <Rating></Rating>
    </v-main>
  </v-app>
</template>

<script>
import Rating from './components/Rating'
export default {
  name: 'App',

  components: {
    'Rating': Rating
  },

  data: () => ({
    //
  }),
};
</script>
