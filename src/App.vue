<template>
  <div id="app">
    <div class="content">
      <h1>{{ selectedCoin.charAt(0).toUpperCase() + selectedCoin.slice(1) }} Price</h1>
      <select v-model="selectedCoin">
        <option value="bitcoin">Bitcoin</option>
        <option value="ethereum">Ethereum</option>
        <option value="dogecoin">Dogecoin</option>
      </select>
      <h2>${{ price }}</h2>
    </div>
  </div>
</template>


<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      price: 0,
      selectedCoin: 'bitcoin',  // Default selection
    };
  },
  methods: {
    fetchBitcoinPrice() {
      axios.get(`https://api.coingecko.com/api/v3/simple/price?ids=${this.selectedCoin}&vs_currencies=usd`)
        .then(response => {
          this.price = response.data[this.selectedCoin].usd;
        })
        .catch(error => {
          console.error('Error fetching price:', error);
        });
    }
  },
  watch: {
    selectedCoin() {
      this.fetchBitcoinPrice();
    }
  },
  created() {
    this.fetchBitcoinPrice();
    setInterval(this.fetchBitcoinPrice, 10000); // Update every 60 seconds
  }
};
</script>

<style scoped>
#app {
  background-image: url('./assets/btc.jpg'); /* Sets the Bitcoin image as background */
  background-size: cover; /* Cover the entire area */
  background-position: center; /* Center the background image */
  height: 100vh; /* Full viewport height */
  display: flex;
  align-items: center; /* Center content vertically */
  justify-content: center; /* Center content horizontally */
  color: white; /* Text color */
  font-family: Arial, sans-serif; /* Simple, clean font */
}

.content {
  padding: 40px; /* Padding around the text */
  text-align: center; /* Center the text inside the content box */
  background: rgba(0, 0, 0, 0.7); /* Semi-transparent black background */
  border-radius: 8px; /* Rounded corners */
}
</style>
