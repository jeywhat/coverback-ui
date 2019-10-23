<template>
  <v-card>
    <v-card-title>
      <v-text-field
              v-model="search"
              append-icon="fa-search"
              label="Search"
              single-line
              hide-details
      ></v-text-field>
      <v-spacer></v-spacer>
    </v-card-title>
    <v-data-table
            :headers="headers"
            :items="info"
            :search="search"
            item-key="title"
            class="elevation-1">

      <template v-slot:item.image="{ item }">
          <img v-bind:src="'data:image/jpeg;base64,'+item.image"/>
      </template>

      <template v-slot:item.score="{ item }">
        <v-chip :color="getColor(item.score)" dark>{{ item.score }}</v-chip>
      </template>

      <template v-slot:item.superXCI="{ item }">
        <v-icon color="green" v-if='item.superXCI'>fa-check</v-icon>
        <v-icon color="red" v-else>fa-times</v-icon>
      </template>

      <template v-slot:item.canBeDownloaded="{ item }">
        <div v-if='item.canBeDownloaded'><a :href="api+item.namefile+'/download'"><v-icon>fa-download</v-icon></a></div>
      </template>

    </v-data-table>
  </v-card>
</template>

<script>

  const axios = require('axios').default;

  export default {
      props: {
          url: {
              type: String
          }
      },
    data () {
      return {
        expanded: [],
        singleExpand: true,
        search: '',
        headers: [
          { text: '', value: 'image', sortable: false },
          { text: 'Title', align: 'left', value: 'title'},
          { text: 'Size (Mo)', value: 'size' },
          { text: 'SuperXCI', value: 'superXCI' },
          { text: 'Nb DLC', value: 'nbDLC' },
          { text: 'Version', value: 'version' },
          { text: 'Score', value: 'score' },
          { text: 'Release Date', value: 'releaseDate' },
          { text: 'Genre', value: 'genre' },
          { text: 'Developer', value: 'developer' },
          { text: 'Rating', value: 'rating' },
          { text: '', value: 'canBeDownloaded' }
        ],
        info: [],
          sheet: false,
          api: ''
      }
    },
    methods: {
      getColor (score) {
        if (score < 40) return 'red'
        else if (score > 70) return 'green'
        else return 'orange'
      }
    },
      watch: {
          url: function() {
              this.api = this.url;
              axios.get(this.url + 'all').then(response => (this.info = response.data)).catch(error => {
                  this.info = [];
                  console.log(error.response);
              });
          }
      }
  }
</script>
