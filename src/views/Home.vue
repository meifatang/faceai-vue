<template>
  <div class="container">
    <div class="card-deck mb-3 text-center">
        <div class="card mb-4 shadow-sm" v-for="item in items" :key="item.datetime">
          <img class="avatar" v-bind:src="item.image_url">
          <h1 class="card-title">{{ item.username }}</h1>
          <p class="list-unstyled">{{ item.datetime }}</p>
          <p class="list-unstyled">{{ item.location }}</p>
        </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Find',
  data() {
    return {
      items: '',
    };
  },
  methods: {
    getResults() {
      const path = 'http://localhost:5000/find';
      axios.post(path)
        .then((res) => {
          var tmp = [];
          res.data["data"].forEach(element => {
            if (element['location'] == "702A") {
              tmp.push(element)
            }
          });
          this.items = tmp.slice(-3);
          console.log(tmp.slice(-3))
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
  created() {
  },
  mounted() {
     setInterval(() => {
      this.getResults();
    }, 1000)
  }
};
</script>

<style scoped>
.container {
  margin-top: 200px;
}
.avatar {
  margin-top: 10px;
  margin-bottom: 10px;
  width: 100px;
  clip-path: circle(50px at center);
  align-self: center;
}
</style>