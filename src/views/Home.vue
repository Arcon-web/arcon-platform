<template>
  <main>
    <div class="title-container">
      <h1>{{ games[activeSlide].title }}</h1>
    </div>
    <carousel :perPage="1" :paginationEnabled="false" :scrollPerPage="false" :mouseDrag="false" :spacePadding="400" :navigate-to="activeSlide">
      <slide v-for="(game, key, i) in games" :key="i">
        <Game :key="i" :image="game.game_image"></Game>
      </slide>
    </carousel>
    <div class="title-container">
      <p>{{ games[activeSlide].description }}</p>
    </div>
  </main>
</template>

<script>
  import Game from "@/components/Game";
  import { Carousel, Slide } from 'vue-carousel';

  export default {
    name: "Home",
    components: {
      Game,
      Carousel,
      Slide
    },
    data() {
      return {
        games: null,
        activeSlide: 0
      };
    },
    methods: {
      handleKeyPress: function (event) {
        if (event.code == "KeyQ") {
          if (this.activeSlide > 0) {
            this.activeSlide -= 1;
          }
        }
        if (event.code == "KeyW") {
          if (this.activeSlide < this.games.length - 1) {
            this.activeSlide += 1;
          }
        }
        if (event.code == "Space") {
          window.location.href = this.games[this.activeSlide].game_link;
          //window.location.replace(this.GameData[this.activeSlide].url);
        }
      }
    },
    mounted: function() {
      const axios = require('axios');
      let USER_TOKEN;
      var self = this

      axios.post('http://arcon.mats.vingerhoets.mtantwerp.eu/api/register', {
        name: 'JensVanAssche',
        email: 'jens.va@hotmail.com',
        password: 'wachtwoord123',
        console_id: 'abc123'
      })
      .then(function (response) {
        console.log("register:");
        console.log(response.data);
      })
      .catch(function (error) {
        console.log(error);
      });

      axios.post('http://arcon.mats.vingerhoets.mtantwerp.eu/api/login', {
        email: 'jens.va@hotmail.com',
        password: 'wachtwoord123',
        console_id: 'abc123'
      })
      .then(function (response) {
        console.log("login:");
        console.log(response.data);
        USER_TOKEN = response.data.data.access_token;
        const AuthStr = 'Bearer '.concat(USER_TOKEN);

        axios.get('http://arcon.mats.vingerhoets.mtantwerp.eu/api/games', { headers: { Authorization: AuthStr }})
        .then(response => {
          console.log(response.data);
          self.games = response.data;
        })
        .catch(function (error) {
          // handle error
          console.log(error);
        })
        .then(function () {
          // always executed
        });
      })
      .catch(function (error) {
        console.log(error);
      });

      

      document.addEventListener('keypress', this.handleKeyPress);
    }
  }
</script>
