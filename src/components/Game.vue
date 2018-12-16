<template>
  <div>
    <img :src="`${image}`">
  </div>
</template>

<script>
  export default {
    name: "Game",
    props: {
      image: String
    }
  }
</script>
