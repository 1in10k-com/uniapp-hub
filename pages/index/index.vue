<template>
  <view class="content">
    <image class="logo" src="/static/logo.png"></image>
    <view class="text-area">
      <text class="title">{{ title }}</text>
    </view>

    <view class="text-area">
      <input type="text" v-model="greeting" /> <br />
      <button @click="modify()">modify</button>
      <button @click="fetchGreeting()">fetch</button>
    </view>
  </view>
</template>

<!-- 


<script>
export default {
  data() {
    return {
      title: "Hello",
    };
  },
  onLoad() {
    console.log("index onload");
  },
  methods: {},
};
</script>

 -->

<script>
import { ethers, providers } from "ethers";
import Greeter from "../../src/artifacts/contracts/Greeter.sol/Greeter.json";
console.log("ether: " + ethers);
console.log("Greeter: " + Greeter);

const greeterAddress = "0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0";
console.log(greeterAddress);

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },

  data() {
    return {
      title: "titile2",
      greeting: "not yet generated",
    };
  },
  onload() {
    console.log("index onload");
  },
  async mounted() {
    let that = this;
    console.log("that is :" + that);
    console.log("provider is :" + providers);
  },
  methods: {
    async requestAccount() {
      await window.ethereum.request({ method: "eth_requestAccounts" });
    },

    async fetchGreeting() {
      if (typeof window.ethereum !== "undefined") {
        const provider = new ethers.providers.Web3Provider(window.ethereum);
        const contract = new ethers.Contract(
          greeterAddress,
          Greeter.abi,
          provider
        );
        try {
          const data = await contract.greet();
          this.greeting = data;
          console.log("data: ", data);
        } catch (err) {
          console.log("Error: ", err);
        }
      }
    },

    async modify() {
      let that = this;
      if (!that.greeting) return;
      if (typeof window.ethereum !== "undefined") {
        await that.requestAccount();
        const provider = new ethers.providers.Web3Provider(window.ethereum);
        const signer = provider.getSigner();
        const contract = new ethers.Contract(
          greeterAddress,
          Greeter.abi,
          signer
        );
        const transaction = await contract.setGreeting(that.greeting);
        await transaction.wait();
        that.fetchGreeting();
      }
    },
  },
};
</script>

<style>
.content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.logo {
  height: 200rpx;
  width: 200rpx;
  margin-top: 200rpx;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 50rpx;
}

.text-area {
  display: flex;
  justify-content: center;
}

.title {
  font-size: 36rpx;
  color: #8f8f94;
}
</style>
