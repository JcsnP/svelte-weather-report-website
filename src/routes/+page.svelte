<svelte:head>
  <title>Weather Report</title>
</svelte:head>

<script>
  import Swal from "sweetalert2"
  import moment from "moment"

  let location = ""
  let weather = ""
  let forecast = ""

  async function getForecast() {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/forecast?q=${location}&units=metric&cnt=7&appid=${import.meta.env.VITE_API_KEY}`
    )
    const data = await response.json()
    if(data.cod == "200") {
      forecast = data
    }
  }

  async function getWeater() {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${location}&units=metric&appid=${import.meta.env.VITE_API_KEY}`
    )
    const data = await response.json()
    if(data.cod === "404") {
      Swal.fire({
        icon: 'error',
        title: 'Oops...',
        text: 'City Not Found!',
      })
      location = ""
    } else {  
      weather = data
    }
  }

  async function getWeatherAndForecast() {
    await getWeater()
    await getForecast()
  }
</script>

<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Anuphan:wght@400;600;700&display=swap');
  * {
    font-family: 'Anuphan', sans-serif;
  }

  .hot {
    background: rgb(255,255,255);
    background: -moz-linear-gradient(90deg, rgba(255,255,255,1) 0%, rgba(255,236,213,1) 100%);
    background: -webkit-linear-gradient(90deg, rgba(255,255,255,1) 0%, rgba(255,236,213,1) 100%);
    background: linear-gradient(90deg, rgba(255,255,255,1) 0%, rgba(255,236,213,1) 100%);
  }

  .cool {
    background: rgb(255,255,255);
    background: -moz-linear-gradient(90deg, rgba(255,255,255,1) 0%, rgba(131,137,226,1) 100%);
    background: -webkit-linear-gradient(90deg, rgba(255,255,255,1) 0%, rgba(131,137,226,1) 100%);
    background: linear-gradient(90deg, rgba(255,255,255,1) 0%, rgba(131,137,226,1) 100%);
  }

  .weather-card {
    background: radial-gradient(circle at 10% 20%, rgba(216, 241, 230, 0.46) 0.1%, rgba(233, 226, 226, 0.28) 90.1%);
  }
</style>

<div class="container mx-auto">
  <h1 class="text-4xl my-5 font-semibold text-center">
    Weather Report
  </h1>
  <form class="w-1/2 mx-auto" on:submit|preventDefault={getWeatherAndForecast}>
    <div class="flex flex-col mb-2"> 
      <input class="border border-gray-400 p-2 bg-gray-100 rounded-lg" type="text" placeholder="Find City." bind:value={location} />
    </div>
    <button class="border w-full bg-blue-600 text-white p-2 rounded-lg shadow-lg hover:bg-blue-700">Get Weather</button>
  </form>

  {#if weather}
    <div class={`w-1/2 mx-auto flex flex-col-reverse md:flex-row items-center justify-between text-start gap-4 border my-3 rounded-lg shadow-xl p-8 ${weather.main.temp > 20 ? 'hot' : 'cool'}`}>
      <div class="w-100">
        <h1 class="text-6xl">
          {weather.main.temp} °C
        </h1>
        <div class="text-base text-slate-500">
          <p>{weather.name}</p>
          <p>Sunset: {new Date(weather.sys.sunset * 1000).toLocaleTimeString()}</p>
          <p>Humidity: {weather.main.humidity}%</p>
          <p>Pressure: {weather.main.pressure}mb</p>
        </div>
      </div>
      {#if weather.main.temp > 20}
        <img src="https://cdn-icons-png.flaticon.com/512/869/869869.png" width="150" alt="hot" />
      {:else}
        <img src="https://cdn-icons-png.flaticon.com/512/642/642000.png" width="150" alt="cool" />
      {/if}
    </div>
  {/if}

  {#if forecast}
    <div class="w-1/2 mx-auto flex md:flex-nowrap flex-wrap gap-2 items-center justify-between my-4">
      {#each forecast.list as item}
        <div class="p-2 rounded-lg shadow-lg border weather-card">
          <h1 class="text-sm text-center">{moment(item.dt_txt).calendar()}</h1>
          <img class="mx-auto" src={`https://openweathermap.org/img/wn/${item.weather[0].icon}@2x.png`} alt="cloud" width="64" height="64">
          <div class="flex flex-col items-center justify-between text-xs mt-3">
            <h1>{item.main.temp} °C</h1>
            <h1>{item.weather[0].main}</h1>
          </div>
        </div>
      {/each}
    </div>
  {/if}

  <footer class="bottom-3 right-3 text-sm absolute">
    Created by <a class="py-0.5 px-2 border rounded-lg bg-blue-600 text-white" href="https://github.com/JcsnP" target="_blank">@JcsnP</a>
  </footer>
</div>