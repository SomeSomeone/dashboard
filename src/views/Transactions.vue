<template>
  <div class="about">
    <div class="buttons">
      <button class="button button-success" v-on:click="startListening">
        Start
      </button>
      <button class="button button-warning" v-on:click="pauseListening">
        Pause
      </button>
      <button class="button button-error" v-on:click="stopListening">
        Stop
      </button>
    </div>
    <h1>Sum: {{ sum }}</h1>
    <table border="1">
      <thead>
        <tr>
          <th style="width: 33vw">Input Address</th>
          <th style="width: 33vw">Out Address</th>
          <th style="width: 33vw">Sum in / Sum out</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="i in data" v-bind:key="i.hash">
          <td v-html="i.inputs.map(u => u.prev_out.addr).join('<br />')"></td>
          <td v-html="i.out.map(u => u.addr).join('<br />')"></td>
          <td>
            {{ i.inputs.reduce((acOut, out) => acOut + out.prev_out.value, 0) }}
            / {{ i.out.reduce((acOut, out) => acOut + out.value, 0) }}
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "Transactions",
  data: () => ({
    socket: null,
    data: []
  }),
  computed: {
    sum() {
      return (
        0 ||
        this.data.reduce(
          (ac, item) =>
            ac + item.out.reduce((acOut, out) => acOut + out.value, 0),
          0
        )
      );
    }
  },
  methods: {
    initSocket() {
      let socket = new WebSocket("wss://ws.blockchain.info/inv");
      socket.onmessage = this.handlerSocket;
      this.socket = socket;
    },
    handlerSocket(event) {
      this.data.push(JSON.parse(event.data).x);
    },
    startListening() {
      this.socket.send('{"op":"unconfirmed_sub"}');
    },
    stopListening() {
      this.pauseListening();
      this.$set(this, "data", []);
    },
    pauseListening() {
      this.socket.send('{"op":"unconfirmed_unsub"}');
    }
  },
  mounted() {
    this.initSocket();
  }
};
</script>
<style lang="scss" scoped>
.buttons {
  display: flex;
  justify-content: center;
  .button {
    margin: 10px;
    border: 2px solid #2c3e50;
    font-size: 20px;
    padding: 5px 10px;
    border-radius: 20px;
    &:focus {
      outline: 0;
    }
    &.button-success {
      background: #78db04;
    }
    &.button-warning {
      background: #ffaa00;
    }
    &.button-error {
      background: #ff0067;
    }
  }
}
</style>
