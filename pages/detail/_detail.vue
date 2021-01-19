<template>
  <div>
    <Spinner v-if="Loading" />

    <v-container fluid class="mt-0 pa-0" v-if="!Loading">
      <v-app-bar app color="transparent" elevation="0">
        <v-btn to="/" text icon>
          <v-icon>mdi-{{ `chevron-left` }}</v-icon>
        </v-btn>
      </v-app-bar>
      <v-layout>
        <v-flex xs12 sm8 md6 offset-sm2 offset-md3 class="text-xs-right">
          <v-img
            lazy-src="https://image-cdn.hypb.st/https%3A%2F%2Fhypebeast.com%2Fimage%2F2020%2F03%2Fwonder-woman-1984-new-poster-release-gal-gadot-tw.jpg?w=960&cbr=1&q=90&fit=max"
            class=" rounded-card ma-0 d-flex"
            height="300"
            :src="`https://image.tmdb.org/t/p/w342${this.movies.poster_path}`"
          >
          </v-img>
        </v-flex>
      </v-layout>
      <v-layout>
        <v-flex xs12 sm8 md6 offset-sm2 offset-md3>
          <!-- {{ this.searchRes }} -->
          <v-card
            class="mx-auto rounded-card-info position-card"
            max-width="380"
          >
            <v-container>
              <v-row class="mt-2 mb-2 mr-2">
                <v-col>
                  <v-row align="center" justify="center">
                    <img
                      class=""
                      height="30"
                      src="../../assets/img/starsDetail.png"
                    />
                  </v-row>
                  <v-row class="mt-4" align="center" justify="center">
                    <span>{{ movies.vote_average }}</span>
                  </v-row>
                </v-col>
                <v-col>
                  <v-row align="center" justify="center">
                    <img
                      class=""
                      height="30"
                      src="../../assets/img/starsRate.png"
                    />
                  </v-row>
                  <v-row class="mt-4" align="center" justify="center">
                    <span>Rate This</span>
                  </v-row>
                </v-col>
                <v-col>
                  <v-row align="center" justify="center">
                    <v-container
                      style="backgroundColor: #51CF66;  width: 50%; paddingTop:2px;  borderRadius: 8px;
                  paddingBottom:2px"
                    >
                      <span
                        style="fontSize: 15px; fontWeight:bold; color: white;"
                        >{{ movies.vote_count }}</span
                      >
                    </v-container>
                  </v-row>
                  <v-row class="mt-4" align="center" justify="center">
                    <span>Metascore</span>
                  </v-row>
                </v-col>
              </v-row>
            </v-container>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
import axios from "axios";
import Spinner from "../../components/UI/Spinner.vue";
export default {
  components: {},
  data() {
    return {
      Loading: true,
      movies: {}
    };
  },
  mounted() {
    this.getDetail();
  },
  methods: {
    getDetail() {
      axios
        .get(
          `https://api.themoviedb.org/3/movie/${this.$route.params.detail}?api_key=${process.env.API_SECRET}`
        )
        .then(response => {
          this.movies = response.data;
          console.log(
            `https://image.tmdb.org/t/p/w342${this.movies.poster_path}`
          );
          this.Loading = false;
        });
    }
  }
};
</script>
<style scoped>
.rounded-card {
  border-bottom-left-radius: 40px;
}
.rounded-card-info {
  border-bottom-left-radius: 40px;
  border-top-left-radius: 40px;
}
.position-card {
  position: absolute;
  top: 240px;
  right: 0px;
  width: 100%;
  height: auto;
}
</style>
