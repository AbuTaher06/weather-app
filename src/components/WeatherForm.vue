<template>
    <div class="weather-form">
      <input
        type="text"
        placeholder="Enter City Name"
        v-model="city"
        class="city-input"
      />
      <button @click="getWeather" class="weather-button">Get Weather</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        city: ''
      };
    },
    methods: {
      getWeather() {
        if (this.city) {
          this.$emit('get-weather', this.city);
          
        } else {
          alert('Enter City Name');
        }
      }
    }
  };
  </script>
  
  <style scoped>
  /* Set a background color and center the form */
  .weather-form {
    background-color: #f5f5f5; /* Light grey background */
    padding: 20px;
    border-radius: 8px; /* Rounded corners */
    max-width: 400px;
    margin: 20px auto;
    text-align: center;
    box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1); /* Subtle shadow */
  }
  
  /* Style the input field */
  .city-input {
    width: 80%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
    margin-bottom: 10px;
    color: #333;
  }
  
  /* Style the button */
  .weather-button {
    background-color: #4caf50; /* Green background */
    color: white; /* White text */
    border: none;
    padding: 10px 20px;
    font-size: 16px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  /* Add a hover effect to the button */
  .weather-button:hover {
    background-color: #45a049; /* Darker green on hover */
  }
  
  /* Responsive design for mobile */
  @media (max-width: 600px) {
    .weather-form {
      width: 90%;
      padding: 15px;
    }
  
    .city-input {
      width: 100%;
    }
  }
  </style>
  