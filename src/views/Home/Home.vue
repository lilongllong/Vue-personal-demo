<template>
  <div class="home">
    <img alt="Vue logo" src="../../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <p>{{ countPlusLocalState }}</p>
    <input v-model="localCount" />
    <button v-on:click="handleFilter">筛选</button>
    <button v-on:click="handleIncrement">增加</button>
    <label>{{message}}</label>
    <ul id="movies">
      <li v-bind:key="item" v-for="item in filterMovies">{{ item }}</li>
    </ul>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import { mapState, mapMutations, mapActions } from 'vuex';
import { INCREMENT } from '@/mutation_types';

export default {
  name: 'home',
  data() {
    return {
      localCount: 0,
      movies: this.$store.getters.singleMovies(3),
      message: 'abc'
    };
  },
  components: {
    HelloWorld
  },
  computed: mapState({
    count: state => state.count,
    countAlias: 'count',
    countPlusLocalState (state) {
      return parseInt(state.count) + parseInt(this.localCount || 0)
    },
    filterMovies: (state, getters) => {
      return getters.filterMovies;
    }
  }),
  watch: {
    localCount: function (val) {
      console.log(val);
    }
  },
  methods: {
    ...mapMutations({
      increment: INCREMENT,
    }),
    ...mapActions({
      incrementAsync: 'incrementAsync',
      filterAsync: 'filterAsync'
    }),
    handleFilter: function (event) {
      console.log(event);
      this.filterAsync({ value: parseInt(this.localCount || 0)}).then(() => {
        this.message = '筛选成功'
      });
    },
    handleIncrement: function(event) {
      console.log(event);
      this.incrementAsync({ value: parseInt(this.localCount || 0)}).then(() => {
        this.message = '增值成功'
      });
    }
  }
}
</script>
