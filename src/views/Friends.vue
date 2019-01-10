<template>
  <main>
    <div v-if="friends == null">
      
    </div>
    <div v-else>
      <div class="title-container">
        <h1>{{ friends.friend[activeSlide] }}</h1>
      </div>
      <carousel :perPage="1" :paginationEnabled="false" :scrollPerPage="false" :mouseDrag="false" :spacePadding="600" :navigate-to="activeSlide">
        <slide v-for="(avatar, key, i) in friends.avatar" :key="i">
          <Friend :key="i" :image="avatar"></Friend>
        </slide>
      </carousel>
      <!-- <div class="title-container">
        <p>Times played: {{ friends[activeSlide].times_played }}</p>
        <p>Wins: {{ friends[activeSlide].wins }}</p>
        <p>Losses: {{ friends[activeSlide].losses }}</p>
        <p>Last played: {{ friends[activeSlide].last_played }}</p>
      </div> -->
    </div>
    <div class="buttons">
        <div>
          <img src="@/assets/images/bluebutton.png">
          <span>Left</span>
        </div>
        <div>
          <img src="@/assets/images/redbutton.png">
          <span>Right</span>
        </div>
        <div>
          <img src="@/assets/images/yellowbutton.png">
          <span>Select</span>
        </div>
        <div>
          <img src="@/assets/images/graybutton.png">
          <span>Back</span>
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
        friends: null,
        activeSlide: 0
      };
    },
    methods: {
      handleKeyPress: function (event) {
        //left
        if (event.code == "KeyQ") {
          if (this.activeSlide > 0) {
            this.activeSlide -= 1;
          }
          else {
            this.activeSlide = this.friends.friend.length - 1;
          }
        }
        //right
        if (event.code == "KeyW") {
          if (this.activeSlide < this.friends.friend.length - 1) {
            this.activeSlide += 1;
          }
          else {
            this.activeSlide = 0;
          }
        }
        //start
        if (event.code == "KeyE") {
          window.location.href = this.$route.params.gameUrl;
        }
        //back
        if (event.code == "KeyR") {
          window.location.href = "/";
        }
      }
    },
    mounted: function() {
      const axios = require('axios');
      var self = this;
      const baseUrl = 'https://arcon.mats.vingerhoets.mtantwerp.eu/api';
      var urls = {
        friends: `${baseUrl}/friends`
      };

      const userdata = {
        name: 'JensVanAssche',
        email: 'mats@gmail.com',
        password: 'secret',
        console_id: 'abc123',
        user_id: '1'
      }

      const userUrl = `${urls.friends}` + "/" + userdata.user_id;

      // GET FRIENDS
      axios.get(userUrl, { headers: { Authorization: self.$route.params.authStr }})
      .then(response => {
        self.friends = response.data;
      })
      .catch(function (error) {
        console.log(error);
      })
      .then(function () {
      });

      document.addEventListener('keypress', this.handleKeyPress);
    }
  }
</script>
