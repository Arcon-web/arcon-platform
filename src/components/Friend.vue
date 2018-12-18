<template>
  <div>
    <img :src="`./images/${image}`">
  </div>
</template>

<script>
  export default {
    name: "Friend",
    props: {
      image: String
    }
  }
</script>
