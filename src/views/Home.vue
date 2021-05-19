<template>
  <main class="" v-if="!loading">
    <DataTitle :title="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats" />
    <CountrySelect @get-country="countryData" :countries="country" />
    <button v-if="stats.Country" @click="Clear" class="bg-green-700 text-white mx-10 rounded p-3 my-10 focus:outline-none hover:bg-green-600">
      Clear
    </button>
  </main>
  <main v-else class="flex flex-col align-center justify-center text-center">
    <div class="text-grey-500 text-3xl mt-10 mb-10">
      memuat data
    </div>
    <img :src="loadingImage" alt="">
  </main>
</template>

<script>
// @ is an alias to /src
import DataTitle from '../components/DataTitle'
import DataBoxes from '../components/DataBoxes'
import CountrySelect from '../components/CountrySelect'

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect
  },
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      country: [],
      loadingImage: require('../assets/load.gif')

    }
  },
  methods: {
    async fetchCovidData() {
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    },
    countryData(country) {
      this.stats = country,
      this.title = country.Country
    },
    async Clear() {
      this.loading = true
      const data = await this.fetchCovidData()
      this.title = 'Global'
      this.stats = data.Global
      this.loading = false
    }
  },
  async created() {
    const data = await this.fetchCovidData()
    
    this.dataDate = data.Date
    this.stats = data.Global
    this.country = data.Countries
    this.loading = false
  }
}
</script>
