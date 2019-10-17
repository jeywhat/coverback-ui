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
          <img v-bind:src="'data:image/jpeg;base64,'+item.image" @click="downloadGame(item)"/>
      </template>

      <template v-slot:item.score="{ item }">
        <v-chip :color="getColor(item.score)" dark>{{ item.score }}</v-chip>
      </template>

      <template v-slot:item.canBeDownloaded="{ item }">
        <v-icon @click='downloadGame(item)' v-if='item.canBeDownloaded'>fa-download</v-icon>
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
          { text: 'Score', value: 'score' },
          { text: 'Release Date', value: 'releaseDate' },
          { text: 'Genre', value: 'genre' },
          { text: 'Developer', value: 'developer' },
          { text: 'Rating', value: 'rating' },
          { text: 'Extension', value: 'extension' },
          { text: '', value: 'canBeDownloaded' }
        ],
        info: [],
          sheet: false,
          api: ''
      }
    },
    methods: {
      getColor (calories) {
        if (calories < 40) return 'red'
        else if (calories > 70) return 'green'
        else return 'orange'
      },
      forceFileDownload(response, item){
        const url = window.URL.createObjectURL(new Blob([response.data]))
        const link = document.createElement('a')
        link.href = url
        link.setAttribute('download', item.title+'.'+item.extension) //or any other extension
        document.body.appendChild(link)
        link.click()
      },downloadGame(item){
        axios({
          method: 'get',
          url: this.api+item.namefile+'/download',
          responseType: 'arraybuffer'
        }).then(response => { this.forceFileDownload(response, item) })
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
