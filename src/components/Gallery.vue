<template >
  <div>
      <button type="button" name="Sort by Date" @click="clickSortByDate"> Sort by Date</button>
      <button type="button" name="Years" @click="clickByYear"> Years </button>
      <button type="button" name="Month" @click="clickByMonth"> Months </button>

  <div class="gallery">
    <div v-if="!byMonth & !byYear">
      <gallery-display :photos="photos" />
    </div>
    <div v-else-if="byMonth && byYear">
      <div v-for="(month,year) in byMonth" :key="year">

        <div v-for="(photo_,key) in month" :key="key" >
          <h3> {{key}}, {{year}} </h3>
          <gallery-display :photos="photo_" />

        </div>
      </div>
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
        byMonth: false,
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
        this.byMonth = false;
        this.sortByDate(this.photos)
      },

      groupByMonth(arr){
        console.log("From group by month")
        var temp = arr.map(function(e) { e.month =  (new Date(e.datetime)).getMonth();
                                      return e})
        temp = groupBy(temp, 'month')
        var months = ["January", "February", "March", "April", "May", "June", "July",
                      "August", "September", "October", "November", "December"]
        var keys = Object.keys(temp)
        for (var i = 0; i < keys.length; i++){
          temp[months[keys[i]]] = temp[keys[i]]
          delete temp[keys[i]]
        }

        return temp
      },
      clickByMonth() {
        console.log("Click by month")
        this.groupByYear()
        var keys = Object.keys(this.byYear)
        this.byMonth = {}
        for(var i = 0; i < keys.length; i++){
          this.byMonth[keys[i]] = this.groupByMonth(this.byYear[keys[i]])
        }

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
      },

      clickByYear(){
        this.byMonth = false
        this.groupByYear()
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
