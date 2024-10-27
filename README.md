<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Weather App</title>
  <style>
    /* Basic styling */
    body {
      font-family: Arial, sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background: linear-gradient(135deg, #A3C4FF 0%, #C4E0E5 100%);
      color: #333;
    }
    .weather-card {
      background-color: white;
      padding: 20px;
      width: 90%;
      max-width: 300px;
      text-align: center;
      border-radius: 10px;
      box-shadow: 0px 5px 20px rgba(0, 0, 0, 0.1);
    }
    .app-title {
      font-size: 2em;
      color: #808080;
      margin-bottom: 20px;
      font-weight: bold;
    }
    .temperature {
      font-size: 3em;
      font-weight: bold;
      color: #333;
    }
    .city-name {
      font-size: 1.5em;
      color: #000000;
      margin-bottom: 15px;
    }
    .city-input {
      width: 100%;
      padding: 10px;
      font-size: 1em;
      margin-bottom: 10px;
      border-radius: 5px;
      border: 1px solid #FFF8DC;
      outline: none;
      box-sizing: border-box;
    }
    .get-weather-button {
      width: 100%;
      padding: 10px;
      background-color: #F5F5F5;
      color: white;
      font-size: 1.1em;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      transition: background-color 0.3s;
    }
    .get-weather-button:hover {
      background-color: #FAF0E6;
    }
    .loading {
      margin-top: 10px;
      color: #555;
    }
    .error-message {
      margin-top: 10px;
      color: red;
      font-weight: bold;
    }
  </style>
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
</head>
<body ng-app="weatherApp" ng-controller="WeatherController as weatherCtrl">

  <div class="weather-card">
    <h1 class="app-title">Weather App</h1>
    
    <div ng-if="weatherCtrl.weatherData">
      <p class="temperature">{{ weatherCtrl.weatherData.main.temp }}°</p>
      <p class="city-name">{{ weatherCtrl.weatherData.name }}</p>
    </div>

    <input
      type="text"
      ng-model="weatherCtrl.city"
      placeholder="Enter city name"
      class="city-input"
    />
    
    <button ng-click="weatherCtrl.getWeather()" class="get-weather-button">Get Weather</button>
    
    <div ng-if="weatherCtrl.loading" class="loading">Loading...</div>
    <div ng-if="weatherCtrl.errorMessage" class="error-message">{{ weatherCtrl.errorMessage }}</div>
  </div>

  <script>
    angular.module('weatherApp', [])
      .controller('WeatherController', ['$http', function($http) {
        const vm = this;
        vm.city = '';
        vm.weatherData = null;
        vm.loading = false;
        vm.errorMessage = '';

        vm.getWeather = function() {
          const apiKey = '80c6cbafc886f08c0045760e2ffc4672';  
          const url = `https://api.openweathermap.org/data/2.5/weather?q=${vm.city}&appid=${apiKey}&units=metric`;

          vm.loading = true;
          vm.weatherData = null;
          vm.errorMessage = '';

          $http.get(url)
            .then(function(response) {
              vm.weatherData = response.data;
              vm.loading = false;
            })
            .catch(function(error) {
              vm.errorMessage = 'City not found. Please try again.';
              vm.loading = false;
            });
        };
      }]);
  </script>

</body>
</html>
