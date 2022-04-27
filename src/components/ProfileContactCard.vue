<template>
  <v-app>
    <v-main>
      <v-container align="center" justify="center" fill-height>
        <v-row class="bg-img" justify="space-around">
          <v-col cols="12" class="mt-8">
            <v-card width="350">
              <v-img
                height="200px"
                src="https://bandaumnikov.ru/build/css-images/b4c7e2d4ce6cc892f0560c26c7e7c717.jpg"
              >
                <v-app-bar class="mt-8" flat color="rgba(0, 0, 0, 0)">
                  <v-avatar size="100" color="white">
                    <v-lazy-image :src="avatar" />
                  </v-avatar>
                </v-app-bar>

                <v-card-title class="white--text mt-8">
                  <p class="ml-3">{{ posts.first_name }}</p>
                </v-card-title>
              </v-img>

              <v-card-text>
                <div class="font-weight-bold ml-8 mb-2">Details</div>

                <v-list two-line>
                  <v-list-item>
                    <v-list-item-icon>
                      <v-icon color="indigo"> mdi-baby-carriage </v-icon>
                    </v-list-item-icon>

                    <v-list-item-content>
                      <v-list-item-title>{{ age }}</v-list-item-title>
                      <v-list-item-subtitle>Age</v-list-item-subtitle>
                    </v-list-item-content>
                  </v-list-item>

                  <v-divider inset></v-divider>

                  <v-list-item>
                    <v-list-item-icon>
                      <v-icon color="indigo"> mdi-account-hard-hat </v-icon>
                    </v-list-item-icon>

                    <v-list-item-content>
                      <v-list-item-title v-if="posts.employment.title">{{
                        posts.employment.title
                      }}</v-list-item-title>
                      <v-list-item-subtitle>Work position</v-list-item-subtitle>
                    </v-list-item-content>
                  </v-list-item>

                  <v-divider inset></v-divider>
                  <v-list-item v-on:click="getBeer()">
                    <v-list-item-icon>
                      <v-icon color="indigo"> mdi-baby-bottle </v-icon>
                    </v-list-item-icon>

                    <v-list-item-content>
                      <v-list-item-title v-if="beer.name"
                        >{{ beer.name }}
                      </v-list-item-title>
                      <v-list-item-subtitle>{{
                        beer.brand
                      }}</v-list-item-subtitle>
                      <v-list-item-subtitle
                        >(alcohol:{{ beer.alcohol }})</v-list-item-subtitle
                      >
                    </v-list-item-content>
                  </v-list-item>
                </v-list>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<style>
.bg-img {
  background-image: url("https://bandaumnikov.ru/build/css-images/d2f453ad5c34983c85ca2c219ba421db.svg");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center top;
  border-radius: 3px;
}
p {
  font-size: 26px;
}
.btn {
  margin-right: 10px;
}
</style>

<script>
import axios from "axios";
import moment from "moment";
import VLazyImage from "v-lazy-image/v2";

export default {
  components: {
    VLazyImage,
  },
  data() {
    return {
      posts: {
        date_of_birth: 0,
        employment: {
          title: null,
        },
      },
      beer: {},
      errors: [],
      age: " ",
      avatar: " ",
    };
  },
  methods: {
    getBeer: function () {
      axios
        .get(`https://random-data-api.com/api/beer/random_beer`)
        .then((response) => {
          this.beer = response.data;
        })
        .catch((e) => {
          this.errors.push(e);
        });
    },
  },
  created() {
    axios
      .get(`https://random-data-api.com/api/users/random_user`)
      .then((response) => {
        this.posts = response.data;
        this.age = moment().diff(`${this.posts.date_of_birth}`, "years");
        this.avatar = this.posts.avatar;
      })
      .catch((e) => {
        this.errors.push(e);
      });
    axios
      .get(`https://random-data-api.com/api/beer/random_beer`)
      .then((response) => {
        this.beer = response.data;
      })
      .catch((e) => {
        this.errors.push(e);
      });
  },
};
</script>
