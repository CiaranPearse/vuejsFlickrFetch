<template>
  <v-flex xs3>
    <v-card>
      {{ photo.id }}
                <v-img
                  class="white--text"
                  height="200px"
                  :src="photo.thumb"
                >
                  <v-container fill-height fluid>
                    <v-layout fill-height>
                      <v-flex xs12 align-end flexbox>
                        <span class="headline">{{ photo.title }}</span>
                      </v-flex>
                    </v-layout>
                  </v-container>
                </v-img>
                <v-card-title primary-title>
                  <div>
                    <span class="grey--text">{{ photo.date }}</span>
                    <h3 class="headline mb-0">{{ photo.tag }}</h3>
                    <span v-for="(tag, index) in photo.tags" :key="index">
                      <span v-if="index < 3">
                        <v-chip row  class="tagText">
                          <a :href="'https://www.flickr.com/photos/tags/' + tag" :alt="tag">{{ tag | truncate(10)}}</a>
                        </v-chip>
                      </span>
                    </span>
                    <div v-if="photo.tagLength > 4">
                      <v-menu offset-y>
                        <v-btn
                          slot="activator"
                          color="primary"
                          dark
                          flat
                          small
                        >
                          {{ photo.tagLength - 3}} more tags
                        </v-btn>
                        <v-list v-for="(tag, index) in photo.tags" :key="index" v-if="index >= 3">
                          <v-list-tile :href="'https://www.flickr.com/photos/tags/' + tag">
                            <v-list-tile-title>{{ tag }}</v-list-tile-title>
                          </v-list-tile>
                        </v-list>
                      </v-menu>
                    </div>
                    <div v-else class="shimNoTags">
                      &nbsp;
                    </div>
                    </span>
                  </div>
                </v-card-title>
                <v-card-actions>
                  <v-btn flat small :href="photo.full" target="_blank">Full Sized <v-icon>open_in_new</v-icon></v-btn>
                </v-card-actions>
              </v-card>
            </v-flex>
          </template>

<script>
export default {
  props: ['photo'],
  data () {
    return {
    }
  }
}
</script>

<style lang="scss">
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


