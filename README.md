<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>India Weather App 🇮🇳</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #ff6b6b, #4ecdc4, #45b7d1, #96ceb4);
            background-size: 400% 400%;
            animation: gradientShift 15s ease infinite;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            color: #2c3e50;
        }

        @keyframes gradientShift {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        .weather-container {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(20px);
            padding: 2.5rem;
            border-radius: 25px;
            box-shadow: 0 25px 50px rgba(0, 0, 0, 0.2);
            width: 95%;
            max-width: 550px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .weather-container::before {
            content: '🇮🇳';
            position: absolute;
            top: 20px;
            right: 20px;
            font-size: 2rem;
            animation: bounce 2s infinite;
        }

        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateY(0); }
            40% { transform: translateY(-10px); }
            60% { transform: translateY(-5px); }
        }

        h1 {
            color: #e74c3c;
            margin-bottom: 1.5rem;
            font-size: 2.2rem;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
        }

        .search-box {
            margin-bottom: 2rem;
            display: flex;
            gap: 10px;
            justify-content: center;
            flex-wrap: wrap;
        }

        #searchInput {
            flex: 1;
            min-width: 250px;
            padding: 18px 25px;
            border: 2px solid #3498db;
            border-radius: 50px;
            font-size: 1.1rem;
            outline: none;
            transition: all 0.3s ease;
            background: rgba(255,255,255,0.8);
        }

        #searchInput:focus {
            border-color: #e74c3c;
            box-shadow: 0 10px 30px rgba(231, 76, 60, 0.3);
            transform: scale(1.02);
        }

        #searchBtn {
            padding: 18px 30px;
            border: none;
            background: linear-gradient(45deg, #e74c3c, #c0392b);
            color: white;
            border-radius: 50px;
            cursor: pointer;
            font-size: 1.1rem;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 8px 20px rgba(231, 76, 60, 0.4);
        }

        #searchBtn:hover {
            transform: translateY(-3px);
            box-shadow: 0 12px 30px rgba(231, 76, 60, 0.6);
        }

        .quick-cities {
            display: flex;
            gap: 10px;
            justify-content: center;
            flex-wrap: wrap;
            margin-bottom: 2rem;
        }

        .city-btn {
            padding: 10px 20px;
            background: rgba(52, 152, 219, 0.2);
            border: 2px solid #3498db;
            color: #2c3e50;
            border-radius: 25px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s ease;
            font-weight: 500;
        }

        .city-btn:hover {
            background: #3498db;
            color: white;
            transform: translateY(-2px);
        }

        .weather-info {
            opacity: 0;
            transform: translateY(30px);
            transition: all 0.6s ease;
        }

        .weather-info.show {
            opacity: 1;
            transform: translateY(0);
        }

        .city-name {
            font-size: 2.8rem;
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 0.5rem;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.1);
        }

        .temp-main {
            font-size: 5rem;
            font-weight: bold;
            background: linear-gradient(45deg, #e74c3c, #f39c12);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            margin-bottom: 1rem;
            text-shadow: none;
        }

        .description {
            font-size: 1.6rem;
            color: #7f8c8d;
            margin-bottom: 2rem;
            text-transform: capitalize;
            font-weight: 500;
        }

        .weather-icon {
            width: 140px;
            height: 140px;
            margin: 0 auto 1rem;
        }

        .details-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
            gap: 1.5rem;
            margin-top: 2rem;
        }

        .detail-card {
            background: linear-gradient(135deg, rgba(52,152,219,0.1), rgba(52,152,219,0.05));
            padding: 1.5rem;
            border-radius: 20px;
            border: 1px solid rgba(52,152,219,0.2);
            backdrop-filter: blur(10px);
            transition: all 0.3s ease;
        }

        .detail-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 35px rgba(52,152,219,0.3);
        }

        .detail-label {
            font-size: 0.95rem;
            color: #7f8c8d;
            margin-bottom: 0.5rem;
            font-weight: 500;
        }

        .detail-value {
            font-size: 1.8rem;
            font-weight: bold;
            color: #2c3e50;
        }

        .loading {
            font-size: 1.4rem;
            color: #3498db;
            padding: 2rem;
        }

        .error {
            color: #e74c3c;
            font-size: 1.2rem;
            margin-top: 1rem;
            padding: 1rem;
            background: rgba(231, 76, 60, 0.1);
            border-radius: 15px;
            border-left: 5px solid #e74c3c;
        }

        @media (max-width: 480px) {
            .search-box {
                flex-direction: column;
                align-items: center;
            }
            #searchInput {
                min-width: 100%;
            }
            .temp-main {
                font-size: 4rem;
            }
            .city-name {
                font-size: 2.2rem;
            }
        }
    </style>
</head>
<body>
    <div class="weather-container">
        <h1>🌡️ India Weather</h1>
        
        <div class="search-box">
            <input type="text" id="searchInput" placeholder="Delhi, Mumbai, Bangalore..." />
            <button id="searchBtn">🔍 Search</button>
        </div>

        <div class="quick-cities">
            <button class="city-btn" onclick="fetchWeather('Delhi')">Delhi</button>
            <button class="city-btn" onclick="fetchWeather('Mumbai')">Mumbai</button>
            <button class="city-btn" onclick="fetchWeather('Bangalore')">Bangalore</button>
            <button class="city-btn" onclick="fetchWeather('Chennai')">Chennai</button>
            <button class="city-btn" onclick="fetchWeather('Kolkata')">Kolkata</button>
        </div>
        
        <div id="weatherInfo" class="weather-info"></div>
    </div>

    <script>
        // 🔥 GET YOUR FREE API KEY: https://openweathermap.org/api
        const API_KEY = 'fa95ca2f36db46b190848e05a97158f6'; 
        const API_URL = 'https://api.openweathermap.org/data/2.5/weather';

        const searchInput = document.getElementById('searchInput');
        const searchBtn = document.getElementById('searchBtn');
        const weatherInfo = document.getElementById('weatherInfo');

        // Load Delhi weather by default
        window.addEventListener('load', () => {
            fetchWeather('Delhi');
        });

        searchBtn.addEventListener('click', getWeather);
        searchInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') getWeather();
        });

        async function getWeather() {
            const city = searchInput.value.trim();
            if (city) fetchWeather(city);
        }

        async function fetchWeather(city) {
            try {
                showLoading();
                searchInput.value = city;
                
                const response = await fetch(`${API_URL}?q=${city}&appid=${API_KEY}&units=metric`);
                const data = await response.json();
                
                if (response.ok) {
                    displayWeather(data);
                } else {
                    showError(`❌ "${city}" not found! Try Delhi, Mumbai...`);
                }
            } catch (error) {
                showError('🌐 Please get API key from openweathermap.org');
            }
        }

        function displayWeather(data) {
            const { name, main, weather, sys, wind } = data;
            const icon = weather[0].icon;
            const description = weather[0].description;

            weatherInfo.innerHTML = `
                <div class="city-name">${name}, ${sys.country}</div>
                <div class="temp-main">${Math.round(main.temp)}°C</div>
                <img src="https://openweathermap.org/img/wn/${icon}@4x.png" alt="${description}" class="weather-icon">
                <div class="description">${description}</div>
                <div class="details-grid">
                    <div class="detail-card">
                        <div class="detail-label">Feels Like</div>
                        <div class="detail-value">${Math.round(main.feels_like)}°C</div>
                    </div>
                    <div class="detail-card">
                        <div class="detail-label">Humidity</div>
                        <div class="detail-value">${main.humidity}%</div>
                    </div>
                    <div class="detail-card">
                        <div class="detail-label">Wind</div>
                        <div class="detail-value">${wind.speed} m/s</div>
                    </div>
                    <div class="detail-card">
                        <div class="detail-label">Pressure</div>
                        <div class="detail-value">${main.pressure} hPa</div>
                    </div>
                </div>
            `;
            
            weatherInfo.classList.add('show');
        }

        function showLoading() {
            weatherInfo.innerHTML = '<div class="loading">⏳ Loading weather...</div>';
            weatherInfo.classList.add('show');
        }

        function showError(message) {
            weatherInfo.innerHTML = `<div class="error">${message}</div>`;
            weatherInfo.classList.add('show');
        }
    </script>
</body>
</html>
