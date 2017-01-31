<template>
  <div class="container">
    <img src="../assets/logo.png" class="logo">
    <h1>Vue Movies</h1>
    <div class="form-group">
      <input type="text" class="form-control" placeholder="Search for a movie" v-model="searchString">
    </div>

    <div class="all-movies">
      <router-link
        v-for="item in filteredMovies"
        class="single-movie" 
        v-bind:style="{ backgroundImage: 'url(' + item.poster + ')' }"
        :to="{path: '/single', query: { id: item._id } }"
        >
        <div class="d-layer"></div>
        <div class="data">
          <h3>{{item.title}}</h3>
          <span class="runtime">
            {{item.runtime | toHours}}h
            {{item.runtime | toMinutes}}m

            <svg class="nc-icon outline" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" width="48px" height="48px" viewBox="0 0 48 48">
              <g transform="translate(0, 0)">
                <line data-color="color-2" fill="none" stroke="#FFFFFF" stroke-width="2" stroke-linecap="square" stroke-miterlimit="10" x1="19" y1="3" x2="29" y2="3" stroke-linejoin="miter"></line>
                <line data-color="color-2" fill="none" stroke="#FFFFFF" stroke-width="2" stroke-linecap="square" stroke-miterlimit="10" x1="24" y1="3" x2="24" y2="9" stroke-linejoin="miter"></line>
                <polyline data-color="color-2" fill="none" stroke="#FFFFFF" stroke-width="2" stroke-linecap="square" stroke-miterlimit="10" points="
                  24,19 24,27 32,27 " stroke-linejoin="miter"></polyline>
                <circle fill="none" stroke="#FFFFFF" stroke-width="2" stroke-linecap="square" stroke-miterlimit="10" cx="24" cy="27" r="18" stroke-linejoin="miter"></circle>
              </g>
            </svg>
          </span>
        </div>
      </router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'home',
  data () {
    return {
      allMovies: [],
      searchString: ''
    }
  },
  created: function(){
    this.$http.get('http://moviesapi-95723.app.xervo.io/')
      .then((items)=>{
        this.allMovies = items.data
      })
  },
  computed: {
    filteredMovies: function () {
        var all_data      = this.allMovies,
            searchString  = this.searchString;

        if(!searchString){
            return all_data;
        }

        searchString = searchString.trim().toLowerCase();


        all_data = all_data.filter(function(item){
            if(item.title.toLowerCase().indexOf(searchString) !== -1){
                return item;
            }
        })

        return all_data;
    }
  },
  filters: {
    toHours: function (value) {
      if (!value) return ''
      var hours = Math.floor( value / 60);          
      return hours
    },
    toMinutes: function (value) {
      if (!value) return ''
      var minutes = value % 60;
      return minutes
    }
  }
}
</script>

<style scoped>
  .logo{
    width: 140px
  }
  h1{
    color: #FFF;
  }
  .all-movies{
    text-align: left
  }
  .all-movies{
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
  }
  .single-movie{
    flex-basis: 31.333%;
    margin-bottom: 3%;
    height: 500px;
    position: relative;
    background-size: 100%;
    background-repeat: no-repeat;
    box-shadow: 0 3px 6px rgba(0,0,0,0.16), 0 3px 6px rgba(0,0,0,0.23);
    transition: .14s;
    color: #FFF
  }
  .single-movie:hover{
    box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23);
    transform: translateY(-5px);
    cursor: pointer
  }
  .single-movie:hover .d-layer{
    background: rgba(0, 0, 0, .45);
  }
  .d-layer{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, .6);
    transition: .14s;
    z-index: 1
  }
  .data{
    position: relative;
    z-index: 3;
    height: 500px;
  }
  h3{
    position: absolute;
    bottom: 30px;
    right: 0;
    left: 20px;
    font-size: 33px;
  }
  svg{
    width: 20px;
    vertical-align: middle;
    display: inline-block;
    position: relative;
    top: -3px
  }
  .runtime{
    position: absolute;
    right: 10px;
    bottom: 0;
  }
</style>
