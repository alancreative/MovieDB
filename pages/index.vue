<template>
  <div class="mt-0 pa-0">
    <v-container class="mt-0 pa-2">
      <v-tabs light color="#3A3A3A">
        <v-tab @click="isPopular()" class="capitalize  ">In Theatres</v-tab>
        <v-tab class="capitalize ">Popular</v-tab>
        <v-tab class="capitalize ">TV Shows</v-tab>
      </v-tabs>
    </v-container>
    <v-spacer></v-spacer>

    <v-sheet class="mx-auto">
      <v-slide-group center-active>
        <v-slide-item
          v-for="movie in movies"
          :key="movie.id"
          v-slot="{ active, toggle }"
        >
          <div>
            <v-card
              draggable
              class=" rounded-card ma-2"
              width="280"
              height="auto"
              color="transparent"
              :to="`/detail/${movie.id}`"
            >
              <v-img
                lazy-src="https://image-cdn.hypb.st/https%3A%2F%2Fhypebeast.com%2Fimage%2F2020%2F03%2Fwonder-woman-1984-new-poster-release-gal-gadot-tw.jpg?w=960&cbr=1&q=90&fit=max"
                class=" rounded-card ma-0 d-flex"
                height="458.92"
                width="350"
                gradient="rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.75)"
                :src="`${movie.poster_path}`"
              >
              </v-img>
            </v-card>
            <v-container width="10px">
              <v-row align="center" justify="center">
                <span class="font-weight-bold title-movie" id="card-title">
                  {{ movie.title }}
                </span>
              </v-row>
              <v-row class="mt-2" align="center" justify="center">
                <v-col cols="2" sm="4">
                  <img class="" src="../assets/img/stars.png" />
                  {{ movie.vote_average }}
                </v-col>
              </v-row>
            </v-container>
          </div>
        </v-slide-item>
      </v-slide-group>
    </v-sheet>

    <v-spacer></v-spacer>
  </div>
</template>

<script>
import axios from "axios";
export default {
  components: {},
  data() {
    return {
      query: "",
      movies: []
    };
  },
  mounted() {
    console.log("========<<<<<<<<<<<========== HELLO");
    console.log("your id is: " + this.$route.name);
    this.getPopular();
    console.log("your id is: " + this.$route.params.id);
  },
  methods: {
    isPopular() {
      this.getPopular();
    },
    getPopular() {
      axios
        .get(
          `https://api.themoviedb.org/3/discover/movie?sort_by=popularity.desc&api_key=e2f3842d3dc69632ce5239805027600e`
        )
        .then(response => {
          this.movies = response.data.results;
          console.log(this.movies);
          for (let i = 0; i < this.movies.length; i++) {
            // Preprocess
            this.movies[i].id = this.movies[i].id.toString();
            // Check for movie poster image
            if (this.movies[i].poster_path === null) {
              this.movies[
                i
              ].poster_path = `https://via.placeholder.com/300x450`;
            } else {
              this.movies[
                i
              ].poster_path = `https://image.tmdb.org/t/p/w342/${this.movies[i].poster_path}`;
            }
            // Add description if null
            if (this.movies[i].overview === "") {
              this.movies[i].overview = "This movie has no description.";
            }
            // Convert rating to price
            let rating = this.movies[i].vote_average;
            if (rating < 3) this.movies[i].price = 3500;
            else if (rating < 6) this.movies[i].price = 8250;
            else if (rating < 8) this.movies[i].price = 16350;
            else this.movies[i].price = 21250;
            // console.log(this.movies[i]);
          }
        });
      console.log(this.movies);
    }
  },
  name: "index",
  head: () => ({
    title: "Homepage"
  })
};
</script>
<style scoped>
.capitalize {
  text-transform: none !important;
  font-size: 20px;
  font-weight: 400;
  padding-left: 0;
}
::v-deep .v-tabs-slider-wrapper {
  color: #63a5e1;
  width: 40px !important;
  height: 5px !important;
}
.rounded-card {
  border-radius: 40px;
}
.title-movie {
  width: 250px;
  display: inline-block;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
  font-size: 2.5vh;
  font-style: normal;
  font-weight: 700;
  line-height: 32px;
  letter-spacing: 0em;
  text-align: center;
}
</style>
