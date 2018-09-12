<template>
  <div class="hello">
    <img class="avatar" v-if="avatarURL.length > 0" :src="avatarURL">
    <p>Hello my name is <b>{{ username }}</b>.</p>
    <p>I currenty have <b>{{ viewersTotal }}</b> total viewers.</p>
  </div>
</template>

<script>
import constants from "../constants.js";
// import Mixer from 'mixer/client-node';

const Mixer = require("@mixer/client-node");

export default {
  name: "Stats",
  data() {
    return {
      title: "test",
      avatarURL: "",
      username: "",
      viewersTotal: ""
    };
  },
  methods: {
    initMixer() {
      const client = new Mixer.Client(new Mixer.DefaultRequestRunner());
      const channelName = "ladymedusa";

      client.use(
        new Mixer.OAuthProvider(client, {
          clientId: constants.clientId
        })
      );

      client.request("GET", `channels/${channelName}`).then(res => {
        this.avatarURL = res.body.user.avatarUrl;
        this.username = res.body.user.username;
        this.viewersTotal = res.body.viewersTotal;

        console.log(res.body);
      });
    }
  },
  created() {
    this.initMixer();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
</style>
