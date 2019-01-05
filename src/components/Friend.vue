<template>
  <div>
    <img :src="`${image}`">
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
