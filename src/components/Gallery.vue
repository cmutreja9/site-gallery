<template >
  <div>
      <button type="button" name="Sort by Date" @click="sortByDate"> Sort by Date</button>
      <button type="button" name="Years" @click="groupByYear"> Years </button>
  <div class="gallery">
    <div v-if="!byYear">

      <div class="gallery-panel" v-for="photo in photos" :key=photo.id>
        <router-link :to="`/photo/${photo.id}`">
          <img :src="thumbUrl(photo.filename)">
        </router-link>
      </div>
    </div>
    <div v-else>
      By Year is coming soon..
    </div>
  </div>
</div>
</template>

<script>
  import photos from '@/photos.json'
  import groupBy from 'lodash/groupBy'

  export default {
    name: 'Gallery',
    data() {
      return {
        byYear: false,
        photos: photos
      }
    },
    methods: {
      thumbUrl(filename) {
        return require(`../assets/images/thumbnails/${filename}`);
      },

      sortByDate() {
        console.log("Sorting by date!")
        this.byYear = false;
        return this.photos.sort((a, b) => new Date(b.datetime) - new Date(a.datetime))

      },

      groupByYear(){
        var temp = Array.from(this.photos)
        temp = temp.map(function(e) { e.year =  (new Date(e.datetime)).getFullYear(); return e})
        temp = groupBy(temp,'year')

        this.byYear = temp;

      }
    }
  }
</script>

<style >
  .gallery {
    display: grid;
    grid-template-columns:  repeat(auto-fill, minmax(20rem, 1fr));
    grid-gap:  2rem;
    max-width: 80rem;
    margin: 5rem auto;
    padding: 5rem 5rem;
  }

  .gallery-panel img {
    width: 100%;
    height: 22vw;
    object-fit: cover;
    border-radius: 0.75rem;

  }
</style>
