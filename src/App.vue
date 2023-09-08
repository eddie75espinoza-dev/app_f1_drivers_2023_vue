<script setup>
  import {ref, computed} from "vue"
  import {data_driver, data_team, data_country} from "./data.js"

  const count = ref(0)
  const total_divers = data_driver.length;

  const nextdriver = () => {
    count.value++
    if (count.value >= total_divers) {
      count.value = 0;
    }
  }

  const backdriver = () => {
    if (count.value <= 0) {
      count.value = total_divers;
    }
    count.value--
  }

  const currentDriver = computed(() => data_driver[count.value]);

  const image_driver = computed(() => currentDriver.value.driver_image);

  const logo_team = computed(() => currentDriver.value.logo_team);
  
  const img_flag_country = computed(() => {
    const currentDriverCountry = currentDriver.value.Country;
    const countryEntry = data_country.find(entry => {
      const countryName = Object.keys(entry)[0]
      return countryName === currentDriverCountry
    })
    if (countryEntry) {
      const currentCountry = Object.keys(countryEntry)[0];
      return countryEntry[currentCountry];
    }
    return null // Manejo de caso en el que no se encuentra el país
  })

</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />
    <img alt="F1 logo" class="logo" src="./assets/f1_logo.png" width="275" height="125" />
  </header>
 
  <main>
    <h1 class="fw-900">FIA Formula One World Championship™ Drivers 2023</h1>
    <h2 class="fw-800 container-label">{{ data_driver[count].Driver }} <span>{{ data_driver[count].number }}</span></h2>
    <div class="fw-600">
      {{ data_driver[count].Team }}
    </div>
    <div class="diver-team">
      <img :src="image_driver" alt="" class="driver"/>
      <img :src="logo_team" alt="" class="logo_team"/>
    </div>
    <p class="fw-500 driver-team">
      <span>
        <img :src="img_flag_country" alt="Flag" width="50" height="30"/>
      </span>
      {{ data_driver[count].Country }}
    </p>
    <p class="fw-600">World Championships: {{ data_driver[count]["World Championships"] }}</p>
    <div class="sec-buttons">
      <button @click="backdriver" class="buttons">Go Back</button>
      <button @click="nextdriver" class="buttons">Next</button>
    </div>
  </main>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Fraunces:wght@100;200;300;400;500;600;700;800;900&display=swap");

.fw-900 {
  font-weight: 900;
}

.fw-800 {
  font-weight: 800;
}

.fw-600 {
  font-weight: 600;
}

.fw-500 {
  font-weight: 500;
}
.container-label {
  color: white;
  background: #555;
  padding: 5px 14px 2px;
  max-width: 100%;
  box-shadow: 0 0 10px rgba(0 0 0 / 30%);
  border-radius: 4px;
}

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

.driver-team{
  display: flex;
  justify-content: space-around;
  place-items: center;
  padding: 1rem;
}

.driver {
  width: 300px;
  height: 300px;
  border-radius:4px;
}

.logo_team{
  width: 300px;
  height: 200px;
  place-items: center;
  
}

.sec-buttons{
  display: flex;
  justify-content: space-around;
  padding: 1rem;
}

.buttons{
  box-shadow: 0 0 10px rgba(0 0 0 / 30%);
  background-color: #555;
  color: white;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
