<script>
  import { PUBLIC_API_KEY } from '$env/static/public'
  let location = ""
  let weather = ""

  async function getWeater() {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${location}&units=metric&appid=${PUBLIC_API_KEY}`
    )
    weather = await response.json()
  }
</script>

<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Anuphan:wght@400;600;700&display=swap');
  * {
    font-family: 'Anuphan', sans-serif;
  }
</style>

<div class="container mx-auto">
  <h1 class="text-4xl my-5 font-semibold text-center">
    Weather Report
  </h1>
  <form class="w-1/2 mx-auto" on:submit|preventDefault={getWeater}>
    <div class="flex flex-col mb-2"> 
      <input class="border border-gray-400 p-2 bg-gray-100 rounded-lg" type="text" placeholder="Find City. Nakhon Si Thammarat" bind:value={location} />
    </div>
    <button class="border w-full bg-blue-600 text-white p-2 rounded-lg shadow-lg hover:bg-blue-700">Get Weather</button>
  </form>

  {#if weather}
    <div class="w-1/2 mx-auto flex flex-row justify-between items-center border p-4 my-3 rounded-lg shadow-lg h-48">
      <div class="flex flex-col font-semibold text-5xl justify-center items-center">
        <h1>
          {weather.main.temp} °C
        </h1>
      </div>
      <div class="text-xl">
        <p>Sunset: {new Date(weather.sys.sunset * 1000).toLocaleTimeString()}</p>
        <p>{weather.name}</p>
        <p>Humidity: {weather.main.humidity}%</p>
        <p>Pressure: {weather.main.pressure}mb</p>
      </div>
    </div>
  {/if}

  <footer class="bottom-3 right-3 text-sm absolute">
    Created by <a class="py-0.5 px-2 border rounded-lg bg-blue-600 text-white" href="https://github.com/JcsnP" target="_blank">@JcsnP</a>
  </footer>
</div>