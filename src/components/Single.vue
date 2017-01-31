<template>
  <div class="container">
    <div class="back-to-home">
      <router-link
        :to="{path: '/'}"
      >
        << Back to home page
      </router-link>
    </div>
    <div class="single-movie-page">
      <h1>{{item.title}} | {{item.year}}</h1>
      <div class="info">
        <div class="image">
          <img :src="item.poster" :alt="item.title" :title="item.title">
        </div>
        <div class="description">
          <p>
            {{item.plot}}
          </p>
          <p>
            {{item.genre}}
          </p>
        </div>
      </div>
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
  </div>
</template>

<script>
export default {
  name: 'single',
  data () {
    return {
      item: ''
    }
  },
  
  created () {
    this.getSingleMovie()
  },
  
  watch: {
    '$route': 'getSingleMovie'
  },
  
  methods: {
    getSingleMovie () {
      this.$http.get('http://moviesapi-95723.app.xervo.io/'+this.$route.query.id)
        .then( (res) => {
          console.log(res.data);
          this.item = res.data
        })
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
  .single-movie-page{
    background: #34495e;
    padding: 20px 30px;
    text-align: left;
    position: relative
  }
  h1{
    border-bottom: 1px solid rgba(238, 238, 238, 0.38);
    padding-bottom: 10px
  }
  .info{
    display: flex;
  }
  .image, .image img{
    width: 300px;
  }
  .description{
    margin-left: 20px;
  }
  .description p{
    font-size: 16px;
    font-weight: 100;
    font-family: arial;
    margin-top: 13px;
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
  .back-to-home{
    position: absolute;
    left: 20px;
    top: 20px;
  }
  .back-to-home a{
    color: #FFF;
  }
</style>
