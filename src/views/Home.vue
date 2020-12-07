<template>
  <div class="container">
    <div class="card-deck mb-3 text-center">
        <div class="card mb-4 shadow-sm" v-for="item in items" :key="item.datetime">
          <div class="avatar-wrapper">
            <img v-bind:src="'/static/33cn/' + item.username + '.jpg'">
          </div>
          <h1 class="card-title">{{ item.username }}</h1>
          <p class="list-unstyled">{{ item.time }}</p>
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
      const path = '/find';
      axios.post(path)
        .then((res) => {
          var tmp = [];
          res.data["data"].forEach(element => {
            if (element['location'] == "702A") {
              tmp.push(element)
            }
          });
          this.items = tmp.slice(-3)
          console.log(this.items)
          if (tmp.slice(-1)["status"] == "First") {
            let utterance = new SpeechSynthesisUtterance(tmp.slice(-1)["username"]);
            speechSynthesis.speak(utterance);
          }
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
.avatar-wrapper {
  position: relative;
  width: 100px;
  height: 100px;
  overflow: hidden;
  border-radius: 50%;
  align-self: center;
  margin-top: 15px;
  margin-bottom: 15px;
}
.avatar-wrapper img {
  width: 100%;
  height: auto;
  margin-top: -50px;
}
</style>
