<template>
  <v-app dark>
    <div v-if="loading">
      <PageLoading />
    </div>
    <div v-else>
      <v-container grid-list-md>
        <v-layout row wrap>
          <v-flex xs10 sm10>
            <v-text-field
              v-model="searchText"
              label="Enter tag to search"
              regular
              clearable
              v-on:keyup.13="preformSearch"
            ></v-text-field>
          </v-flex>
          <v-flex xs2 sm2>
            <v-btn dark color="red" @click="preformSearch" large>Search Flickr</v-btn>
          </v-flex>
        </v-layout>
        <div v-if="hasError">
          <ErrorMessage :errorMsg="errorMsg" />
        </div>
        <div v-else>
          <v-layout row wrap v-if="photos.length > 0">
            
              <PhotoCard :photo="photo" v-for="photo in photos" :key="photo.id" />
           
          </v-layout>
          <v-layout row wrap v-else>
            <h3>Please search for a Tag</h3>
          </v-layout>
        </div>
      </v-container>
    </div>
  </v-app>
</template>

<script>
import axios from 'axios'
import PhotoCard from './components/PhotoCard/'
import PageLoading from './components/PageLoading/'
import ErrorMessage from './components/ErrorMessage/'
export default {
  data () {
    return {
      flickrApiKey: '6347d99644f5a2b060c5559e70ecbfbd',
      loading: true,
      searchText: '',
      photos: [],
      photoPage: 0,
      perPage: 20,
      hasError: false,
      errorMsg: 'Something went wrong!'
    }
  },
  components: {
    PhotoCard,
    PageLoading,
    ErrorMessage
  },
  created () {
    this.loading = false
  },
  beforeMount () {
  },
  mounted () {
    this.scroll()
  },
  methods: {
    preformSearch () {
      this.photos = []
      this.photoPage = 0
      this.getPhotos()
    },
    getPhotos () {
      this.photoPage = this.photoPage + 1
      axios.get('https://api.flickr.com/services/rest/?method=flickr.photos.search&api_key=' + this.flickrApiKey + '&tags=' + this.searchText + '&extras=description%2C+license%2C+date_upload%2C+date_taken%2C+owner_name%2C+icon_server%2C+original_format%2C+last_update%2C+geo%2C+tags%2C+o_dims%2C+views%2C+media%2C+path_alias%2C+url_sq%2C+url_t%2C+url_s%2C+url_q%2C+url_m%2C+url_n%2C+url_z%2C+url_c%2C+url_l%2C+url_o&per_page=' + this.perPage + '&page=' + this.photoPage + '&format=json&nojsoncallback=1', {
      })
      .then(response => {
        if (response.data.stat === 'fail') {
          this.hasError = true
          this.errorMsg = response.data.message
        } else {
          let allPhotos = response.data.photos.photo
          if (allPhotos.length === 0) {
            this.hasError = true
            this.errorMsg = 'No Photos found for ' + this.searchText
          } else {
            this.hasError = false
            for (var i = 0; i <= (allPhotos.length - 1); i++) {
              var photoId = allPhotos[i].id
              var thumbPhoto = allPhotos[i].url_q
              var pathalias = ''
              if (allPhotos[i].pathalias) {
                pathalias = allPhotos[i].pathalias
              } else {
                pathalias = allPhotos[i].owner
              }
              var fullPhoto = 'https://www.flickr.com/photos/' + pathalias + '/' + photoId
              var title = allPhotos[i].title
              var allTags = allPhotos[i].tags
              var tags = allTags.split(' ')
              var tagLength = tags.length
              var owner = allPhotos[i].ownername
              var dateTaken = allPhotos[i].datetaken
              this.photos.push({
                thumb: thumbPhoto,
                full: fullPhoto,
                title: title,
                tags: tags,
                tagLength: tagLength,
                owner: owner,
                date: dateTaken
              })
            }
          }
        }
      })
      .catch(error => {
        this.hasError = true
        this.errorMsg = 'Something went wrong!'
        console.log(error)
      })
      this.loading = false
    },
    scroll () {
      window.onscroll = () => {
        let bottomOfWindow = document.documentElement.scrollTop + window.innerHeight === document.documentElement.offsetHeight
        if (bottomOfWindow) {
          this.getPhotos()
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.headline {
  font-size: 20px!important;
  line-height: 28px!important;
}
.tagText {
  font-size: 11px;
  a {
    color: #000;
    text-decoration: none;
    &:hover {
      text-decoration: none;
    }
  }
}
.shimNoTags {
  height: 40px;
}

</style>
