<template>
    <div id="app">
      <h1>Stock Market Data</h1>
  
      <!-- Top Gainers Table -->
      <section>
        <h2>Top Gainers</h2>
        <table border="1">
          <thead>
            <tr>
              <th>Ticker</th>
              <th>Price</th>
              <th>Change Amount</th>
              <th>Change Percentage</th>
              <th>Volume</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in topGainers" :key="'gainer-' + index">
              <td>{{ item.ticker }}</td>
              <td>{{ item.price }}</td>
              <td>{{ item.changeAmount }}</td>
              <td>{{ item.changePercentage }}%</td>
              <td>{{ item.volume }}</td>
            </tr>
          </tbody>
        </table>
      </section>
  
      <!-- Top Losers Table -->
      <section>
        <h2>Top Losers</h2>
        <table border="1">
          <thead>
            <tr>
              <th>Ticker</th>
              <th>Price</th>
              <th>Change Amount</th>
              <th>Change Percentage</th>
              <th>Volume</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in topLosers" :key="'loser-' + index">
              <td>{{ item.ticker }}</td>
              <td>{{ item.price }}</td>
              <td>{{ item.changeAmount }}</td>
              <td>{{ item.changePercentage }}%</td>
              <td>{{ item.volume }}</td>
            </tr>
          </tbody>
        </table>
      </section>
  
      <!-- Most Actively Traded Table -->
      <section>
        <h2>Most Actively Traded</h2>
        <table border="1">
          <thead>
            <tr>
              <th>Ticker</th>
              <th>Price</th>
              <th>Change Amount</th>
              <th>Change Percentage</th>
              <th>Volume</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in mostActive" :key="'active-' + index">
              <td>{{ item.ticker }}</td>
              <td>{{ item.price }}</td>
              <td>{{ item.changeAmount }}</td>
              <td>{{ item.changePercentage }}%</td>
              <td>{{ item.volume }}</td>
            </tr>
          </tbody>
        </table>
      </section>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    name: "App",
    data() {
      return {
        topGainers: [],
        topLosers: [],
        mostActive: [],
      };
    },
    methods: {
      async fetchStockData() {
        try {
          const response = await axios.get("https://www.alphavantage.co/query?function=TOP_GAINERS_LOSERS&apikey=demo");
  
          // Check for valid data structure before processing
          if (response.data) {
            // Mapping data with proper null/undefined checks
            this.topGainers = (response.data.top_gainers || []).map((item) => ({
              ticker: item.ticker || "N/A",
              price: item.price ? parseFloat(item.price).toFixed(2) : "N/A",
              changeAmount: item.change_amount ? parseFloat(item.change_amount).toFixed(2) : "N/A",
              changePercentage: item.change_percentage ? parseFloat(item.change_percentage).toFixed(2) : "N/A",
              volume: item.volume ? parseInt(item.volume).toLocaleString() : "N/A",
            }));
  
            this.topLosers = (response.data.top_losers || []).map((item) => ({
              ticker: item.ticker || "N/A",
              price: item.price ? parseFloat(item.price).toFixed(2) : "N/A",
              changeAmount: item.change_amount && !isNaN(parseFloat(item.change_amount)) ? parseFloat(item.change_amount).toFixed(2) : "N/A",
              changePercentage: item.change_percentage ? parseFloat(item.change_percentage).toFixed(2) : "N/A",
              volume: item.volume ? parseInt(item.volume).toLocaleString() : "N/A",
            }));
  
            this.mostActive = (response.data.most_actively_traded || []).map((item) => ({
              ticker: item.ticker || "N/A",
              price: item.price ? parseFloat(item.price).toFixed(2) : "N/A",
              changeAmount: item.change_amount ? parseFloat(item.change_amount).toFixed(2) : "N/A",
              changePercentage: item.change_percentage ? parseFloat(item.change_percentage).toFixed(2) : "N/A",
              volume: item.volume ? parseInt(item.volume).toLocaleString() : "N/A",
            }));
          } else {
            console.error("Unexpected response format:", response.data);
          }
        } catch (error) {
          console.error("Error fetching stock data:", error);
        }
      },
    },
    created() {
      this.fetchStockData();
    },
  };

  </script>
  
  <style>
  #app {
    font-family: Arial, sans-serif;
    padding: 20px;
  }
  
  table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 20px;
  }
  
  table th{
    text-align: left;
    padding: 8px;
    color: black;
  }
  
  table td {
    padding: 8px;
    color: white;
  }
  
  table th {
    background-color: #f4f4f4;
  }
  
  h1, h2 {
    color: white;
  }
  </style>
  