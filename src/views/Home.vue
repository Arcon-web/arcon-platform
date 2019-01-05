<template>
  <main>
    <div v-if="games == null"></div>
    <div v-else>
      <div class="title-container">
        <h1>{{ games[activeSlide].title }}</h1>
      </div>
      <carousel
        :perPage="1"
        :paginationEnabled="false"
        :scrollPerPage="false"
        :mouseDrag="false"
        :spacePadding="350"
        :navigate-to="activeSlide"
      >
        <slide v-for="(game, key, i) in games" :key="i">
          <Game :key="i" :image="game.game_image"></Game>
        </slide>
      </carousel>
      <div class="title-container">
        <p>{{ games[activeSlide].description }}</p>
      </div>
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
          this.$router.push({ name: 'friends', params: { gameTitle: this.games[this.activeSlide].title, gameUrl: this.games[this.activeSlide].game_link } });
        }
      }
    },
    mounted: function() {
      const axios = require('axios');
      var USER_TOKEN;
      var self = this;
      const baseUrl = 'http://arcon.mats.vingerhoets.mtantwerp.eu/api';
      var urls = {
        register: `${baseUrl}/register`,
        login: `${baseUrl}/login`,
        games: `${baseUrl}/games`
      };

      const userdata = {
        name: 'JensVanAssche',
        email: 'mats@gmail.com',
        password: 'secret',
        console_id: 'abc123',
      }

      // POST REGISTER
      // axios.post(urls.register, {
      //   name: userdata.name,
      //   email: userdata.email,
      //   password: userdata.password,
      //   console_id: userdata.console_id
      // })
      // .then(function (response) {
      //   // console.log("register:");
      //   // console.log(response.data);
      // })
      // .catch(function (error) {
      //   console.log(error);
      // });

      // POST LOGIN
      axios.post(urls.login, {
        email: userdata.email,
        password: userdata.password,
        console_id: userdata.console_id
      })
      .then(function (response) {
        // console.log("login:");
        // console.log(response.data);
        USER_TOKEN = response.data.data.access_token;
        const AuthStr = 'Bearer '.concat(USER_TOKEN);

        // GET GAMES
        axios.get(urls.games, { headers: { Authorization: AuthStr }})
        .then(response => {
          // console.log("games:");
          // console.log(response.data);
          self.games = response.data;
        })
        .catch(function (error) {
          console.log(error);
        })
        .then(function () {
        });
      })
      .catch(function (error) {
        console.log(error);
      });

      document.addEventListener('keypress', this.handleKeyPress);
    }
  }
</script>
