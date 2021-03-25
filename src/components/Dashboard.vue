<template>
  <v-container>
    <v-layout text-xs-center>
      <v-flex md3>
        <v-card class="ma-3">
          <v-container grid-list-sm fluid>
            <v-layout row wrap>
              <v-subheader class="title">MEOW!</v-subheader>
              <v-img alt="A random cat image" :src="randomCatImage" transition="scale-transition" />
            </v-layout>
          </v-container>
        </v-card>
        <v-card class="ma-3">
          <v-container grid-list-sm fluid>
            <v-layout row wrap>
              <v-list mandatory>
                <v-subheader class="title"><strong>CAT</strong>EGORIES</v-subheader>
                <v-list-item-group color="primary">
                  <v-list-item v-for="category in categories" :key="category.id">
                    <v-list-item-content @click="selected_category = category.id">
                      <v-list-item-title v-text="category.name" class="cat-name"></v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                </v-list-item-group>
              </v-list>
            </v-layout>
          </v-container>
        </v-card>
      </v-flex>
      <v-flex md9>
        <div class="ma-3" v-show="selected_category === -1">
          Select a <strong>cat</strong>egory to see more cats ...
        </div>
        <v-card class="ma-3" v-show="selected_category > -1">
          <div class="cat-image__container">
            <div class="ma-2 cat-image" v-for="image in images" :key="image.id">
              <img alt="" :src="image.url" />
            </div>
          </div>
          <div>
            <v-btn @click="getImages()" class="button float-right">Show more cats</v-btn>
          </div>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>
<script>
import axios from 'axios';

export default {
  data: () => ({
    categories: [],
    images: [],
    selected_category: -1,
    page: 0,
    order: 'Desc',
    limit: 10,
    randomCatImage: require('@/assets/images/cat-placeholder.png') // placeholder
  }),
  created() {
    this.getCategories();
    this.getRandomCatImage();
  },
  watch: {
    limit: function() {
      this.getImages();
    },
    selected_category: function() {
      this.getImages();
    }
  },
  methods: {
    async getCategories() {
      try {
        axios.defaults.headers.common['x-api-key'] = 'fad892f5-2617-4d82-bf16-3d28911a38a1';

        let response = await axios.get('https://api.thecatapi.com/v1/categories/');

        this.categories = response.data;
      } catch (err) {
        console.log(err);
      }
    },
    async getImages() {
      try {
        axios.defaults.headers.common['x-api-key'] = 'fad892f5-2617-4d82-bf16-3d28911a38a1';

        let query_params = {
          limit: this.limit,
          order: this.order,
          page: this.page,
          category_ids: this.selected_category.id
        };

        let response = await axios.get('https://api.thecatapi.com/v1/images/search', {
          params: query_params
        });

        this.images = response.data;
        this.page++;
      } catch (err) {
        console.log(err);
      }
    },
    async getRandomCatImage() {
      try {
        axios.defaults.headers.common['x-api-key'] = 'fad892f5-2617-4d82-bf16-3d28911a38a1';

        let query_params = {};
        let response = await axios.get('https://api.thecatapi.com/v1/images/search', {
          params: { query_params }
        });

        this.randomCatImage = response.data[0].url;
      } catch (err) {
        console.log(err);
      }
    }
  }
};
</script>
<style scoped>
.title {
  font-size: 1.5rem;
  color: #6080e0 !important;
}

.cat-image__container {
  display: flex;
  flex-wrap: wrap;
  padding: 5px 7px;
}

.cat-image {
  flex: 1 0 16%;
  border: 1px solid #aaa;
  border-radius: 10px;
  max-width: 150px;
  max-height: 150px;
  object-fit: cover;
  object-position: center center;
}

.cat-image:after {
  content: '';
  display: block;
  padding-bottom: 100%;
}

.cat-image img {
  object-fit: cover;
  width: 150px;
  height: 150px;
}

.cat-name {
  text-transform: capitalize;
}

.button {
  margin: 10px;
  margin-right: 0;
}
</style>
