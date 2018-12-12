<template>
  <main>
    <carousel :perPage="1" :paginationEnabled="false" :scrollPerPage="false" :mouseDrag="false" :spacePadding="400" :navigate-to="activeSlide">
      <slide v-for="(game, key, i) in GameData" :key="i">
        <Game :key="i" :image="game.image"></Game>
      </slide>
    </carousel>
    <div class="title-container">
      <h1>{{ GameData[activeSlide].name }}</h1>
      <p>{{ GameData[activeSlide].description }}</p>
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
        GameData: [
          { name: "Pacman", description: "Pacman is a good game", image: "game_pacman.png", url: "google.com"},
          { name: "Pong", description: "Pong is gebaseerd op tafeltennis. De naam Pong is afgeleid van pingpong. Het spel werkt met twee batjes en een balletje en twee muren aan de zijkant. De bedoeling van het spel is dat het balletje achter het batje van de tegenspeler terechtkomt.", image: "game_pong.png", url: "google.com"},
          { name: "Tetris", description: "Tetris is a good game", image: "game_tetris.png", url: "google.com"},
          { name: "Pacman", description: "Pacman is a good game", image: "game_pacman.png", url: "google.com"},
          { name: "Pong", description: "Pong is gebaseerd op tafeltennis. De naam Pong is afgeleid van pingpong. Het spel werkt met twee batjes en een balletje en twee muren aan de zijkant. De bedoeling van het spel is dat het balletje achter het batje van de tegenspeler terechtkomt.", image: "game_pong.png", url: "google.com"},
          { name: "Tetris", description: "Tetris is a good game", image: "game_tetris.png", url: "google.com"}
        ],
        activeSlide: 0
      };
    },
    methods: {
      handleKeyPress: function (event) {
        if (event.key == "a") {
          if (this.activeSlide > 0) {
            this.activeSlide -= 1;
          }
        }
        if (event.key == "z") {
          if (this.activeSlide < this.GameData.length - 1) {
            this.activeSlide += 1;
          }
        }
      }
    },
    mounted: function() {
      document.addEventListener('keypress', this.handleKeyPress);
    }
  }
</script>
