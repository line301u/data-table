<template lang="pug">
  v-container
    v-row
      v-col(cols)
        DataFilter(
          v-if="items.length"
          :setFilter="setFilter"
          :clearFilter="clearFilter"
        )
        DataTable(
          v-if="items.length"
          :headers="headers"
          :items="items"
          :search="filterSettings.search"
        )
        v-progress-circular(
          v-else
          width="2"
          color="rs__primary"
          indeterminate
        ).mx-auto
</template>

<script>
import DataTable from '~/components/DataTable.vue'
import DataFilter from '~/components/DataFilter.vue'
import sales from '~/api/sales.js'
import {gender_list, country_list} from '../global/filterData.js'

export default {
  components: {
    DataFilter,
    DataTable,
  },
  data() {
    return {
      sales,
      items: [],
      loading: true,
      options: {},
      filterSettings:{
          gender:[],
          country: [],
          search: ""
      },
      headers: [
        { text: 'Firstname', value: "user.first_name", align: 'start'},
        { text: 'Lastname', value: 'user.last_name'},
        { text: 'Email', value: 'email' },
        { text: 'Gender', value: 'gender', 
            filter: value => {
              const selectedValues = this.filterSettings.gender;

              if (selectedValues.length <= 0) return true
              if (selectedValues.includes(value)) return value
            },
        },
        { text: 'Year', value: 'year' },
        { text: 'Sales', value: 'sales' },
        { text: 'Country', value: 'country',
            filter: value => {
              const selectedValues = this.filterSettings.country;

              if (selectedValues.length <= 0) return true
              if (selectedValues.includes(value)) return value
            },
        },
      ],
    }
  },

  async created() {
    this.items = await this.fetchData(0, 50)
  },
  methods: {
    async fetchData(page, size) {
      const start = page * size
      await this.delay(3000)
      return await sales.results.slice(start, start + size)
    },
    delay(ms) {
      return new Promise(resolve => setTimeout(resolve, ms))
    },
    setFilter(selectedValues, filterType) {
      console.log(filterType)
      if (filterType === "gender"){
        this.filterSettings.gender = selectedValues;
      }

      if (filterType === "country"){
        this.filterSettings.country = selectedValues;
      }

      if (filterType === "search"){
        this.filterSettings.search = selectedValues;
      }
    },
    clearFilter(){
      this.filterSettings.gender = [];
      this.filterSettings.country = [];
      this.filterSettings.search = "";
    },
  }
}
</script>

<style lang="sass" scoped>
.v-progress-circular
  position: absolute
  top: 50%
  left: 50%
</style>