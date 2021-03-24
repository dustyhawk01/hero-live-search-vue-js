<template>
  <div class="container">
    <input type="text" id="search" placeholder="Search for a superhero..." v-model="searchQuery">

    <div class="row" v-if="loaded">
      <div class="col-md-4" v-for="item in searchHero" v-bind:key="item.Name">
        <div class="card">
          <div class="card-body">
            <h5 class="card-title">{{item.Name}}</h5>
            <h6 class="card-subtitle mb-2 text-muted">{{item.Location}}</h6>
            <p class="card-text">
              🧠 <font class="f-w-700">Intelligence:</font> {{item.Intelligence}}<br>
              💪 <font class="f-w-700">Strength:</font> {{item.Strength}}<br>
              🚀 <font class="f-w-700">Speed:</font> {{item.Speed}}<br>
              🏹 <font class="f-w-700">Power:</font> {{item.Power}}
            </p>
          </div>
        </div>
      </div>
    </div>

    <div v-else>
      <h2 class="loading">Loading...</h2>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        loaded: false,
        data: {},
        searchQuery: ''
      }
    },

    beforeMount(){
      this.assignData();
    },
    methods: {
      async assignData() {
        const res = await fetch('https://superheroes-api.herokuapp.com/api/getall');
        const data = await res.json();
        this.data = data;
        this.loaded = true;
      }
    },

    computed: {
      searchHero: function () {

        var heroes = this.data;
        var searchQuery = this.searchQuery;

        if(!searchQuery) {
          return this.data;
        }

        heroes = heroes.filter(function(item) {
          if(item.Name.toLowerCase().indexOf(searchQuery) !== -1){
            return item;
          }
        })

        return heroes;
      }
    }
  };
</script>
