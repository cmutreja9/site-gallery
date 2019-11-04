<template >
  <div>
      <button type="button" name="Sort by Date" @click="clickSortByDate"> Sort by Date</button>
      <button type="button" name="Years" @click="groupByYear"> Years </button>
  <div class="gallery">
    <div v-if="!byYear">
      <gallery-display :photos="photos" />
    </div>
    <div v-else>
      <div  v-for="(photo_, key) in byYear" :key="key">
        <h2> {{key}} </h2>
        <gallery-display :photos="photo_" />
      </div>
    </div>
  </div>
</div>
</template>

<script>
  import photos from '@/photos.json'
  import groupBy from 'lodash/groupBy'
  import GalleryDisplay from '@/components/GalleryDisplay.vue';

  export default {
    name: 'Gallery',
    components: {
      GalleryDisplay
    },
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

      sortByDate(arr) {

        return arr.sort((a, b) => new Date(b.datetime) - new Date(a.datetime))
      },

      clickSortByDate () {
        this.byYear = false;
        this.sortByDate(this.photos)
      },

      groupByYear(){
        var temp = Array.from(this.photos)
        temp = temp.map(function(e) { e.year =  (new Date(e.datetime)).getFullYear();
                                      e.month = (new Date(e.datetime)).getMonth();
                                      return e})
        temp = groupBy(temp,'year')
        var keys = Object.keys(temp).reverse()
        console.log("keys",keys)

        for (var i=0; i < temp.length; i++){
          temp[keys[i]] = this.sortByDate(temp[keys[i]])
        }
        console.log(temp)
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
</style>
