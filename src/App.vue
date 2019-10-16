<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Cover</span>
        <span class="font-weight-light">BACK</span>
        <span style="padding-left: 10px;" class="overline">Switch Edition </span>


      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        text icon color="grey darken-2"
        href="https://github.com/jeywhat/"
        target="_blank"
      >
        <v-icon>fa-github</v-icon>
      </v-btn>

      <v-btn
              text icon color="grey darken-2"
              href="https://github.com/vuetifyjs/vuetify/releases/latest"
              target="_blank"
      >
        <v-icon>fa-cog</v-icon>
      </v-btn>
    </v-app-bar>

    <v-content>
      <GameTable/>
    </v-content>

  </v-app>
</template>

<script>
import GameTable from './components/GameTable';



export default {
  name: 'App',
  components: {
    GameTable,
  },
  data: () => ({
    icons: [
      'fab fa-facebook',
      'fab fa-twitter',
      'fab fa-google-plus',
      'fab fa-linkedin',
      'fab fa-instagram',
    ]
  })
};
</script>
