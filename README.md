import React, { useState } from 'react';

const WeatherApp = () => {
  const [city, setCity] = useState('');
  const [weather, setWeather] = useState(null);
  const [error, setError] = useState(null);

  const apiKey = '0.00012; // Replace with your OpenWeatherMap API key

  const fetchWeather = async () => {
    if (!city) return;
    try {
      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`
      );
      if (!response.ok) {
        throw new Error('City not found');
      }
      const data = await response.json();
      setWeather(data);
      setError(null);
    } catch (err) {
      setError(err.message);
      setWeather(null);
    }
  };

  return (
    <div className="min-h-screen flex flex-col items-center justify-center bg-blue-100 p-4">
      <h1 className="text-3xl font-bold mb-4">Weather App</h1>
      <div className="flex space-x-2 mb-4">
        <input
          type="text"
          value={city}
          onChange={(e) => setCity(e.target.value)}
          placeholder="Enter city name"
          className="px-4 py-2 border rounded"
        />
        <button
          onClick={fetchWeather}
          className="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700"
        >
          Get Weather
        </button>
      </div>

      {error && <p className="text-red-600">{error}</p>}

      {weather && (
        <div className="bg-white p-4 rounded shadow text-center">
          <h2 className="text-2xl font-semibold">{weather.name}</h2>
          <p className="text-xl">{weather.main.temp} °C</p>
          <p className="capitalize">{weather.weather[0].description}</p>
        </div>
      )}
    </div>
  );
};

export default WeatherApp;
