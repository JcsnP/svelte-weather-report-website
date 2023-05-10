<script>
  import { PUBLIC_API_KEY } from '$env/static/public'
  let location = ""
  let weather = ""

  async function getWeater() {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${location}&units=metric&appid=${PUBLIC_API_KEY}`
    )
    weather = await response.json()
    console.log(weather)
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
    <div class={`w-1/2 mx-auto flex flex-row justify-between items-center border my-3 rounded-lg shadow-xl p-8 ${weather.main.temp > 20 ? 'hot' : 'cool'}`}>
      <div class="flex flex-col text-start gap-4">
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

  <footer class="bottom-3 right-3 text-sm absolute">
    Created by <a class="py-0.5 px-2 border rounded-lg bg-blue-600 text-white" href="https://github.com/JcsnP" target="_blank">@JcsnP</a>
  </footer>
</div>