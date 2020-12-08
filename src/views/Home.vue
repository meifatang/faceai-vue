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
      tag: ''
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
          if (res.tag != tmp.slice(-1)[0]["username"]) {
            if (tmp.slice(-1)[0]["status"] == "First" || tmp.slice(-1)[0]["status"] == "Frist") {
              res.tag == tmp.slice(-1)[0]["username"]
              let utterance = new SpeechSynthesisUtterance(tmp.slice(-1)[0]["username"]);
              speechSynthesis.speak(utterance);
              console.log(tmp.slice(-1)[0]["username"], "First Time")
            }
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
    }, 500)
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
