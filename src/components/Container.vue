<template>
  <div>
    <Post
      v-for="(item, i) in instaData"
      :key="`instaData-${i}`"
      :instaData="item"
    />
  </div>
</template>

<script>
  import Post from '@/components/Post.vue'
  export default {
    name: 'Container',
    components: {
      Post
    },
    props: {
      instaData: Array,
    }
  }
</script>

<style>

</style>