<template>

  <div id="app" class="w-screen flex flex-col">
    <div class="flex justify-center">
      <div class="container">
        <h1 class="pt-3 pb-3 pl-2 text-center font-extrabold">OMDB</h1>
        <p class="pl-2 text-center font-bold">Jakub Ingvar Pitak</p>
  <!--       <div class="input_all"> -->
        <div class="">
          <div class="utan_input p-5 flex justify-center">
            <label class="pt-1">Title:</label>
            <input id="leit" class="rounded-l-lg w-1/3" type="text" name="t" v-model="search" placeholder="Search movies, tv shows and more..." @keyup.enter="leitaAllt" autofocus>

            <input type="text" class="w-24 no_border" name="y" v-model="year" placeholder="By Year" @keyup.enter="leitaAllt">

            <button @click="leitaAllt" class="rounded-r-lg">Search All</button>
            <button @click="leitaID" class="rounded-lg ml-2 border-r-4">Search By ID</button>
          </div>
        </div>
      </div>
    </div>

    <div v-model="appear" v-if="appear == ''" class="text-4xl flex justify-center height_css text-grey-dark">search results will appear here</div>

    <div v-model="string" v-if="link_id != false" class="flex justify-center text-2xl">
      <div class="background_setting p-2 border-b-2 border-black flex justify-center">
        <span class="font-bold">Link for API:</span>
        <a :href="string">{{ string }}</a>
      </div> 
    </div>

    <div v-if="link_id == false" v-for="movie in all" class="flex flex-col items-center">
      <div class="background_setting">
        <div class="movies ml-5 mt-10">
          <div @click="sjaMeira(movie.imdbID)" class="flex-1 cursor-pointer">
            <div class="img mr-2"><img :src="movie.Poster" alt=""></div>
            <div><h1>{{ movie.Title }}</h1></div>
            <div><h2>({{ movie.Year }})</h2></div>
            <br>
            <div class="absolute drop_down"><img src="arrow2.png" width="40px"></div>
          </div>
          <br>
          <br>

        </div>
      </div>
      <!-- hér -->
      <div v-if="details === movie.imdbID" class="width_info text-xl flex flex-row bg-white">
        <div class="p-8 info"><span class="font-bold text-2xl">plot:</span><br>{{ plot }}</div>
        <div class="p-8 text-2xl">
          <span class="font-bold">User Rating:</span> {{ ratingUser }} <span class="font-bold">Out of:</span> {{ votes }} users
          <br><br>
          <span class="font-bold">Metascore:</span> <span class="border-4 border-grey-dark p-1 primary">{{ metascore }}</span>
          <br><br>
          <span class="font-bold">Director:</span> {{ director }}
        </div>
      </div>
      <div v-if="details === movie.imdbID" @click="sjaMeira2(movie.imdbID)" class="pl-8 pt-2 pb-2 more_details font-bold text-xl cursor-pointer">
        more information
        <img class="more absolute" src="arrow2.png" width="20px">
      </div>
      <div v-if="details2 === movie.imdbID & details === movie.imdbID" class="width_info2 text-xl flex flex-row bg-white">
        <div class="w-full">
          <div class="p-2 pt-4 pl-8 text-xl important">
            <span class="font-bold">Writer:</span> {{ writer }}
          </div>
          <div class="p-2 pl-8 text-xl important">
            <span class="font-bold">Actors:</span> {{ actors }}
          </div>
          <div class="p-2 pl-8 text-lg">
            <span class="font-bold">Genre:</span> {{ genre }}
          </div>
          <div class="p-2 pl-8 text-lg">
            <span class="font-bold">Runtime:</span> {{ runtime }}
          </div>
          <div class="p-2 pl-8 text-lg">
            <span class="font-bold">Awards:</span> {{ awards }}
          </div>
          <div class="p-2 pl-8 text-lg">
            <span class="font-bold">Rated:</span> {{ rated }}
          </div>
          <div class="p-2 pl-8 text-lg">
            <span class="font-bold">Released:</span> {{ released }}
          </div>
          <div class="p-2 pl-8 text-lg">
            <span class="font-bold">Box Office:</span> {{ boxOffice }}
          </div>
          <div class="p-2 pl-8 text-lg">
            <span class="font-bold">Production:</span> {{ production }}
          </div>
          <div class="p-2 pb-4 pl-8 text-lg">
            <span class="font-bold">Movie ID:</span> {{ id }}
          </div>
        </div>
      </div>



    </div>
    <div v-if="line == 'show'" class="flex justify-center">
      <p class="lina"></p>
    </div>
  </div>

</template>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
import axios from "axios";
export default {
  name: 'app',

  data() {
    return {
      search: '',
      year: '',
      string: '',
      all: [],
      titill: [],
      umTitil: [],
      plot: [],
      ratingUser: [],
      metascore: [],
      id: [],
      director: [],
      writer: [],
      actors: [],
      rated: [],
      released: [],
      runtime: [],
      genre: [],
      awards: [],
      boxOffice: [],
      production: [],
      votes: [],
      appear: '',
      line: '',
      show: false,
      details: '',
      details2: false,
      rotate: false,
      link_id: false
    }
  },

  methods: { 
    sjaMeira(imdbID) {
      if (this.details == '') {
        this.details = imdbID;
      }
      else if(this.details == imdbID) {
        this.details = '';
      }
      else {
        this.details = imdbID;
      }
      
      this.titill = 'http://www.omdbapi.com/?i=' + imdbID + '&apikey=a1a6a959';

      axios.get(this.titill)
      .then((response) => {
        this.umTitil = response.data.Title;
        this.plot = response.data.Plot;
        this.ratingUser = response.data.imdbRating;
        this.metascore = response.data.Metascore;
        this.director = response.data.Director;
        this.votes = response.data.imdbVotes;

      if (this.rotate == false) {
        this.rotate = true;
      }
      else if(this.rotate == true){
        this.rotate = false;
      }
      })
    },

    sjaMeira2(imdbID) {
      if (this.details2 == false) {
        this.details2 = imdbID;
      }
      else if(this.details2 == imdbID) {
        this.details2 = false;
      }
      else {
        this.details2 = imdbID;
      }
      
      this.titill = 'http://www.omdbapi.com/?i=' + imdbID + '&apikey=a1a6a959';

      axios.get(this.titill)
      .then((response) => {
        this.id = response.data.imdbID;
        this.writer = response.data.Writer;
        this.actors = response.data.Actors;
        this.rated = response.data.Rated;
        this.released = response.data.Released;
        this.runtime = response.data.Runtime;
        this.genre = response.data.Genre;
        this.awards = response.data.Awards;
        this.boxOffice = response.data.BoxOffice;
        this.production = response.data.Production;
      })
    },

    leitaAllt(){
      if(this.year == ""){
        this.string = 'http://www.omdbapi.com/?s=' + this.search +'&apikey=a1a6a959';
      }
     else{
        this.string = 'http://www.omdbapi.com/?s=' + this.search + "&y=" + this.year + '&apikey=a1a6a959';
      }
      this.link_id = false;
      axios.get(this.string)
      .then((response) => {
        this.all = response.data.Search;

        for(var child = 0; child < this.all.length; child++){
          if (this.all[child].Poster == "N/A"){
            this.all[child].Poster = "/noImg.png";
            this.line = "show";
          }
        }
      })

      this.appear = "hide";
    },

    leitaID(){
      this.line = "";
      this.string = 'http://www.omdbapi.com/?i=' + this.search +'&apikey=a1a6a959';
      this.appear = "hide";
      this.link_id = "show";
    }
  }
}
</script>



<style lang="scss">
@import "./assets/app.scss";
</style>
