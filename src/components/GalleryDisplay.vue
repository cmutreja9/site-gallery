<template >
<div>
  <div class="gallery-panel" v-for="photo in photos" :key=photo.id>
    <router-link :to="`/photo/${photo.id}`">
      <img :src="thumbUrl(photo.filename)">
    </router-link>
  </div>
</div>
</template>

<script>
export default {
  name: 'gallery-display',
  props: {
    photos: Array
  },
  methods: {
    thumbUrl(filename) {
      return require(`../assets/images/thumbnails/${filename}`);
    },
  }
}

</script>

<style >
  .gallery-panel img {
    width: 100%;
    height: 22vw;
    object-fit: cover;
    border-radius: 0.75rem;
  }
</style>
