<template>
  <v-container>
    <h3>Paga 1 nano a questo indirizzo:</h3>

    <v-row class="d-flex justify-center mt-7">
      <qrcode-vue :value="genNanoUrl(address, amount)" :size="size" level="H" />
    </v-row>

    <h4 class="d-flex justify-center mt-7">AMOUNT</h4>
    <p class="d-flex justify-center">
      <span :style="{ color: received >= amount ? '#2196f3' : '#ff0000' }">{{
        received == 0 ? received.toFixed(2) : received
      }}</span>
      /{{ amount }} NANO
    </p>
  </v-container>
</template>

<script>
import BigNumber from "bignumber.js";
import QrcodeVue from "qrcode.vue";

var vm = {
  components: {
    QrcodeVue,
  },

  sockets: {
    connect: function () {
      console.log("socket connected");
    },
  },

  props: {
    address: {
      type: String,
      required: true,
    },
    amount: {
      type: Number,
      required: true,
    },
  },

  name: "VueNano", // vue component name
  data: () => ({
    connection: null,

    value: false,
    active: false,
    timeLeft: null,
    time: 120,
    received: 0.0,
    api_key: 123456,
    size: 300,
  }),

  created: function () {
    console.log("Starting connection to WebSocket Server");
    //this.connection = new WebSocket("ws://localhost:7090/call")

    var url = "wss://yapraiwallet.space/call";
    this.connection = new WebSocket(url);
    this.connection.onopen = this.onOpen;
    this.connection.onclose = this.onClose;
    this.connection.onmessage = this.onMessage;
    this.connection.onerror = this.onError;
  },

  methods: {
    onOpen: function (event) {
      console.log("Connected to WebSocket Server");
      console.log(event);
      this.connection.send(
        JSON.stringify({
          api_key: this.api_key,
          address: this.address,
        })
      );
    },

    onError: function (event) {
      console.log(event);
    },

    onMessage: function (event) {
      var data = JSON.parse(event.data);
      console.log(data);

      this.received = this.received + data.amount / 10 ** 30;

      if (this.received >= this.amount) {
        this.$emit("input", true);
      }
    },

    genNanoUrl: function (addr, amount) {
      let x = new BigNumber(amount * 10 ** 30);
      return `nano:${addr}?amount=${x.toFixed()}`;
    },
  },
  watch: {
    active: function (val) {
      if (val) {
        this.loadQrCode();
      }
      this.$emit("input", val);
    },
    value: function (val) {
      this.active = val;
    },
  },
};

export default vm;
</script>

