<script setup>
import { onMounted, ref } from 'vue';

// const countries = [
  // {
  //   region: 'Africa',
  //   name: 'Nigeria',
  //   flag: '',
  //   capital: 'Abuja',
  //   currencies: ['NGN', 'USD']
  // },
//   {
//     region: 'Africa',
//     name: 'Kenya',
//     flag: '',
//     capital: 'Nairobi',
//     currencies: ['KSH', 'USD', 'Mpesa']
//   }
// ];

const countries = ref(null);

async function getCountries() {
  let response = await fetch('https://restcountries.com/v3.1/all');
  let data = await response.json();

  countries.value = data.map((country) => {
    return {
      region: country.region,
      name: country.name.common,
      flag: country.flag,
      capital: country.capital instanceof Array ? country.capital.join(', ') : country.capital,
      currencies: country.currencies ? Object.keys(country.currencies) : [],
    };
  });
}

onMounted(() => {
  getCountries();
})
</script>

<template>
  <table border="1">
    <thead>
      <tr>
        <th>S/N</th>
        <th>Region</th>
        <th>Name</th>
        <th>Flag</th>
        <th>Capital</th>
        <th>Currencies</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(country, index) in countries" :key="country.name">
        <td>{{ index + 1 }}</td>
        <td>{{ country.region }}</td>
        <td>{{ country.name }}</td>
        <td>{{ country.flag }}</td>
        <td>{{ country.capital }}</td>
        <td>{{ country.currencies.join(', ') }}</td>
      </tr>
    </tbody>
  </table>
</template>

<style scoped>
th, td {
  padding: 10px;
}
</style>
