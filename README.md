<!DOCTYPE html>
<html lang="mn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Цаг агаарын мэдээлэл</title>
    <style>
        body {
    font-family: Arial, sans-serif;
    background-color: #f3f3f3;
    margin: 0;
    padding: 20px;
}

.container {
    max-width: 600px;
    margin: 0 auto;
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1 {
    text-align: center;
}

input[type="text"] {
    width: 100%;
    padding: 10px;
    margin: 10px 0;
    border-radius: 4px;
    border: 1px solid #ccc;
}

button {
    padding: 10px;
    background-color: #000000;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

button:hover {
    background-color: #0027ff;
}

.weather-info {
    margin-top: 20px;
    text-align: center;
}

.weather-info h2 {
    margin: 10px 0;
}

.weather-info p {
    margin: 5px 0;
}

.error {
    color: red;
    font-size: 18px;
    text-align: center;
}
    </style>

</head>
<body>

<div class="container">
    <h1>Цаг агаарын мэдээлэл</h1>

    <label for="city">Хот сонгох эсвэл хайх:</label>
    <input type="text" id="city" placeholder="Хотын нэр оруулна уу">
    <button onclick="getWeather()">Цаг агаар харах</button>

    <div class="weather-info" id="weatherInfo">

    </div>

    <div class="error" id="errorMessage"></div>
</div>

<script>
    const apiKey = 'af1d8df6ba50761ba966de31e6689151'; 

async function getWeather() {
    const city = document.getElementById('city').value.trim();
    const errorMessage = document.getElementById('errorMessage');
    const weatherInfo = document.getElementById('weatherInfo');

    if (city === "") {
        errorMessage.textContent = "Хотын нэр оруулна уу!";
        weatherInfo.innerHTML = "";
        return;
    }

    const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric&lang=mn`;

    try {
        const response = await fetch(url);
        const data = await response.json();

        if (data.cod === 200) {
            errorMessage.textContent = "";
            displayWeather(data);
        } else {

            weatherInfo.innerHTML = "";
            errorMessage.textContent = "Хотын мэдээлэл олдсонгүй.";
        }
    } catch (error) {
        console.error('Алдаа:', error);
        weatherInfo.innerHTML = "";
        errorMessage.textContent = "Цаг агаарын мэдээллийг татаж авахад алдаа гарлаа.";
    }
}

function displayWeather(data) {
    const temp = data.main.temp;
    const weatherDesc = data.weather[0].description;
    const humidity = data.main.humidity;
    const windSpeed = data.wind.speed;
    const cityName = data.name;

    document.getElementById('weatherInfo').innerHTML = `
        <h2>${cityName} хотын цаг агаар</h2>
        <p>Температур: ${temp}°C</p>
        <p>Цаг агаар: ${weatherDesc}</p>
        <p>Чийгшил: ${humidity}%</p>
        <p>Салхи: ${windSpeed} м/с</p>
    `;
}
</script>

</body>
</html>
