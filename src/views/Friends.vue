<template>
  <main>
    <div v-if="friends == null">
      
    </div>
    <div v-else>
      <div class="title-container">
        <h1>{{ friends[activeSlide].name }}</h1>
      </div>
      <carousel :perPage="1" :paginationEnabled="false" :scrollPerPage="false" :mouseDrag="false" :spacePadding="600" :navigate-to="activeSlide">
        <slide v-for="(friend, key, i) in friends" :key="i">
          <Friend :key="i" :image="friend.image"></Friend>
        </slide>
      </carousel>
      <div class="title-container">
        <p>Times played: {{ friends[activeSlide].times_played }}</p>
        <p>Wins: {{ friends[activeSlide].wins }}</p>
        <p>Losses: {{ friends[activeSlide].losses }}</p>
        <p>Last played: {{ friends[activeSlide].last_played }}</p>
      </div>
    </div>
  </main>
</template>

<script>
  import Friend from "@/components/Friend";
  import { Carousel, Slide } from 'vue-carousel';

  export default {
    name: "Friends",
    components: {
      Friend,
      Carousel,
      Slide
    },
    data() {
      return {
        friends: [
            { name: "Blossom", image: "friend_blossom.png", times_played: "12", wins: "12", losses: "0", last_played: "12:34 18/12/2018", url: "https://google.com" },
            { name: "Jerry", image: "friend_jerry.png", times_played: "52", wins: "12", losses: "40", last_played: "20:24 17/12/2018", url: "https://google.com" },
            { name: "Duck", image: "friend_duck.png", times_played: "4", wins: "0", losses: "4", last_played: "08:45 12/12/2018", url: "https://google.com" },
            { name: "Homer", image: "friend_homer.png", times_played: "532", wins: "245", losses: "287", last_played: "23:04 15/12/2018", url: "https://google.com" },
            { name: "Felix", image: "friend_felix.png", times_played: "35", wins: "12", losses: "23", last_played: "14:56 04/12/2018", url: "https://google.com" }
        ],
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
          if (this.activeSlide < this.friends.length - 1) {
            this.activeSlide += 1;
          }
        }
        if (event.code == "Space") {
          window.location.href = this.friends[this.activeSlide].url;
        }
      }
    },
    mounted: function() {
        document.addEventListener('keypress', this.handleKeyPress);
    }
  }
</script>
