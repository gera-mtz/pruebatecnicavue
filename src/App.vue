<script>
import axios from "axios";
export default { 
  data() {
    return {
      url: 'https://rickandmortyapi.com/api/character',
      characters: [],
      pages: [],
      title: 'Rick and Morty',
      modal: false,
    }
  },
  created(){
    this.exec(this.url)
  },
  methods:{
    exec(url){
      axios.get(url)
      .then(response => {
        this.characters = response.data.results;
        this.pages = response.data.info;
      })
      .catch(error => {
        console.log(error);
      })
    },
    next(){
      this.exec(this.pages.next)
    },
    prev(){
      this.exec(this.pages.prev)
    },
    change(val){
      this.exec(this.url + '?page=' + val)
    },
    detail(id){
      this.$router.push('/detail/' + id)
      this.modal = true
    }
  }
}
</script>

<template>
  <div id="app">
    <h1>{{ title }}</h1>
    <el-row :gutter="20">
      <el-col :span="6" v-for="item in characters" v-bind:key="item.id">
        <a href="#" v-on:click="detail(item.id)">
          <div class="grid-content bg-purple">
            <el-avatar :size="50" :src="item.image">
            </el-avatar>
            {{ item.name }}
          </div>
        </a>
      </el-col>
    </el-row>
    <div class="block">
      <el-pagination
        layout="prev, pager, next"
        :page-count="pages.pages"
        @prev-click="prev"
        @next-click="next"
        @current-change="change"
        >
      </el-pagination>
    </div>
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}

.el-row {
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
  }
  .el-col {
    border-radius: 4px;

    margin-bottom: 10px;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }

</style>
