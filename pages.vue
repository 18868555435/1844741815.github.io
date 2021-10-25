<template>
  <div>
    <ul>
      <li v-for="(item,index) in filterArr" :key="index"><span>{{ item.id }}</span> <span>{{ item.title }}</span></li>
    </ul>
    <div class="pages" @click="selectPages">
      <span v-for="p in pagetotal" :key="p" :style="(p-1)===currentpage?{'background':'green'}:''" :data-pageNum="p">{{ p }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "pages",
  data() {
    return {
      arr: [],
      currentpage: 0,
      Toltal: 0,
      pageNum: "",
      pageSize:5,
      loadDataArr: []
    }
  },
  created() {
    this.addArr()
  },
  methods: {
    selectPages(e) {
      if(e.target.nodeName === 'SPAN'){
        this.currentpage = e.target.dataset.pagenum - 1
      }

    },
    addArr() {
      return fetch('https://jsonplaceholder.typicode.com/todos')
          .then(response => response.json())
          .then(json => {
            json.length = 191;
            this.Toltal = json.length;
            //使用Lodash 数组方法chunk 把源文件折分成一个二维数组
            // [[1-10][11-20]]
            let arr = this.$_.chunk(json, this.pageSize);
            this.arr = arr;
          })
    },
  },
  computed: {
    filterArr() {
      return this.arr[this.currentpage]
    },
    pagetotal() {
      let t = Math.ceil(this.Toltal / this.pageSize);
      return t
    }
  },
}
</script>

<style scoped>
ul, .pages {
  padding: 0;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  flex-wrap: wrap;
  list-style: none;
  width: 700px
}

li {
  margin: 10px;
  width: 350px;
  height: 50px;
  text-align: center;
  line-height: 50px;
  background: red;
  color: white;
  display: flex;
  justify-content: space-between;
}

li span:first-child {
  background: green;
  width: 50px
}

li span:last-child {
  width: 300px;
}

.pages {
  flex-direction: row;
  justify-content: flex-start;
}

.pages span {
  display: inline-block;
  background: #ddd;
  width: 50px;
  height: 50px;
  line-height: 50px;
  color:white;
  margin:10px;
  text-align: center;
}
</style>