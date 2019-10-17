<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Cover</span>
        <span class="font-weight-light">BACK</span>
        <span style="padding-left: 10px;" class="overline">Switch Edition </span>


      </v-toolbar-title>
      <v-spacer></v-spacer>
      <span class="caption" style="padding-top:10px;">powered by jeywhat</span>
      <v-btn
        text icon color="grey darken-2"
        href="https://github.com/jeywhat/"
        target="_blank"
      >
        <v-icon>fa-github</v-icon>
      </v-btn>

      <v-bottom-sheet v-model="sheet">
        <template v-slot:activator="{ on }">
          <v-btn
                  text icon color="grey darken-2"
                  v-on="on"
          >
            <v-icon>fa-cog</v-icon>
          </v-btn>
        </template>
        <v-sheet class="text-center" height="200px">
          <v-btn
                  class="mt-6"
                  text
                  color="red"
                  @click="sheet = !sheet"
          >close settings</v-btn>
          <input  class="text-center" id="api" style="width:100%" type="text" v-model.lazy='apiUrl'/>
        </v-sheet>
      </v-bottom-sheet>
    </v-app-bar>

    <v-content>
      <GameTable :url="this.apiUrl"/>
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
    ],
    sheet : false,
    apiUrl : ''
  }),
  mounted () {
    if (localStorage.url) {
      this.apiUrl = localStorage.url;
    }
  },
  watch: {
    apiUrl(apiUrl) {
      localStorage.url = apiUrl;
    }
  }
};
</script>
