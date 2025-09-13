<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Weather App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(to right, #83a4d4, #b6fbff);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
    }

    h1 {
      margin-bottom: 20px;
      color: #333;
    }

    .weather-container {
      background-color: white;
      padding: 30px 40px;
      border-radius: 10px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.1);
      text-align: center;
    }

    input[type="text"] {
      padding: 10px;
      width: 200px;
      border-radius: 5px;
      border: 1px solid #ccc;
      margin-right: 10px;
    }

    button {
      padding: 10px 15px;
      border: none;
      background-color: #007BFF;
      color: white;
      border-radius: 5px;
      cursor: pointer;
    }

    button:hover {
      background-color: #0056b3;
    }

    .result {
      margin-top: 20px;
      font-size: 1.2em;
    }

    .error {
      color: red;
      margin-top: 10px;
    }
  </style>
</head>
<body>

  <h1>Simple Weather App</h1>
  <div class="weather-container">
    <input type="text" id="locationInput" placeholder="Enter location..." />
    <button onclick="getWeather()">Get Weather</button>
    <div class="result" id="weatherResult"></div>
    <div class="error" id="errorMessage"></div>
  </div>

  <script>
    async function getWeather() {
      const location = document.getElementById('locationInput').value;
      const resultDiv = document.getElementById('weatherResult');
      const errorDiv = document.getElementById('errorMessage');

      resultDiv.innerHTML = '';
      errorDiv.innerHTML = '';

      if (!location) {
        errorDiv.innerHTML = 'Please enter a location.';
        return;
      }

      const apiKey = 'a3d99b6e5ddf442fad790934251209';
      const url = `http://api.weatherapi.com/v1/current.json?key=${apiKey}&q=${encodeURIComponent(location)}&aqi=yes`;

      try {
        const response = await fetch(url);
        if (!response.ok) {
          throw new Error('Location not found');
        }

        const data = await response.json();
        const tempC = data.current.temp_c;
        const condition = data.current.condition.text;
        const city = data.location.name;
        const country = data.location.country;

        resultDiv.innerHTML = `
          <strong>${city}, ${country}</strong><br/>
          Temperature: ${tempC}°C<br/>
          Condition: ${condition}
        `;
      } catch (error) {
        errorDiv.innerHTML = 'Could not retrieve weather data. Please check the location and try again.';
      }
    }
  </script>

</body>
</html>
