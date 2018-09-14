<template>
  <div class="hello">
    <!-- <div v-if="visible">
      <p class="animate fadeInUp">
        Hey Shwepo.
      </p>
      <p class="animate fadeInUp one">
        Made dot for you.
      </p>
      <p class="animate fadeInUp two">
        Dot.
      </p>
      <img class="avatar animate fadeInUp three" :src="avatarURL" >
      <br/><br/>
      <button class="animate fadeInUp four" @click="reload()">again?</button>
    </div> -->
    <div class="search">
      <input v-model="username" @keydown.enter="getProfle()" @input="loaded=false; error=false;" placeholder="enter username">
      <button @click="getProfle()">FIND</button>
    </div>

    <div v-if="loaded && !error" class="animate fadeInUp one">
      <img class="avatar" :src="avatarURL">
      <p>Hello my name is <b>{{ username }}</b>.</p>
      <p>I currenty have <b>{{ viewersTotal }}</b> total viewers and <b>{{ numFollowers }}</b> total followers.</p>
    </div>
    <div class="error" v-if="error">
      <p>Error, no such user</p>
    </div>
  </div>
</template>

<script>
import constants from "../../constants.js";
import "./Stats.css";

const Mixer = require("@mixer/client-node");
const client = new Mixer.Client(new Mixer.DefaultRequestRunner());

export default {
  name: "Stats",
  data() {
    return {
      title: "test",
      avatarURL: "",
      username: "",
      viewersTotal: "",
      numFollowers: "",
      loaded: false,
      error: false,
      visible: true
    };
  },
  methods: {
    initMixer() {
      client.use(
        new Mixer.OAuthProvider(client, {
          clientId: constants.clientId
        })
      );
    },
    getProfle() {
      let channelName = this.username;
      client.request("GET", `channels/${channelName}`).then(res => {
        console.log(res.body);
        if (res.statusCode == 200) {
          this.avatarURL = res.body.user.avatarUrl;
          //this.username = res.body.user.username;
          this.viewersTotal = res.body.viewersTotal;
          this.numFollowers = res.body.numFollowers;
          this.loaded = true;
        } else {
          this.error = true;
        }
      });
    },
    reload() {
      this.visible = false;
      setTimeout(() => {
        this.visible = true;
      }, 1000);
    }
  },
  created() {
    this.initMixer();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.avatar {
  width: 50px;
  border-radius: 50%;
}
.error p {
  color: #ff6961;
}
.search {
  margin-bottom: 20px;
}
</style>
