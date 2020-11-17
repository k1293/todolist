<template>
<div id="todolist">
<h1 v-text="title">My ToDoList</h1>
<div>
  <input type="text" v-model="newItem">
  <button @click="addNewItem">新增</button>
</div>
<ul>
<li v-for="item in items" :key="item.id">
<!-- 待辦事項 -->
<span v-text="item.task"></span>
<!-- 完成按鈕 -->
<button v-on:click="toggleFinish(item.id)">完成</button>
</li>
<p></p>
</ul>
</div>
</template>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
const axios = require('axios').default;
const vm = this;
export default {
  name: 'todolist',
  data () {
    return {
      title: 'Welcome to My ToDoList App',
      items: null
    }
  },
  methods: {
    toggleFinish (recycleItem) {

    let config = {
      headers: {
        header: "Access-Control-Allow-Methods",
      }
    }
      axios.delete('http://127.0.0.1:8000/api/app/'+recycleItem,config)
    },
   update(){
    this.$forceUpdate();
  },

    addNewItem () {
      axios.post('http://127.0.0.1:8000/api/app/', {
        task: this.newItem
      })
      .then(function (response) {
        console.log(response);        
      })
      .catch(function (error) {
        console.log(error);
      });
      this.$forceUpdate();
    }
  },
  mounted () {
    axios
      .get('http://127.0.0.1:8000/api/app/')
      .then(response => (this.items = response.data))
      .then(r =>(console.log(this.items)))
      .catch(error => {
        console.log(error)
      })
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 10px;
}
li {
  padding: 10px;
  margin: 0 10px;
}
a {
  color: #42b983;
}
#todolist {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
