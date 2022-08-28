<template lang="pug">
  v-container
    v-row
      v-col(cols)
        DataFilter(
          v-if="items.length < 1000"
          :setFilter="setFilter"
          :clearFilter="clearFilter"
        )
        v-data-table(
          v-if="items.length < 1000"
          :headers="headers"
          :items="items"
          :search="filterSettings.search"
          item-key="email"
          dense
          :options.sync="options"
          :server-items-length="totalItems"
          :loading="loading"
        ).elevation-1.mt-10

        // v-progress-circular(
        //   v-else
        //   width="2"
        //   color="rs__primary"
        //   indeterminate
        // ).mx-auto
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
      totalItems: 0,
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
    this.getDataFromApi();
  },
  watch: {
      options: {
        async handler() {
          await this.getDataFromApi()
        },
        deep: true,
      },
    },
  methods: {
    async getDataFromApi(){
      this.loading = true;
      let tableData = await this.fetchData();
      this.items = tableData.items
      this.totalItems = tableData.totalItems
      this.loading = false
    },
    async fetchData() {
          const response = await sales.results
        
          return new Promise((resolve, reject) => {
          const { sortBy, sortDesc, page, itemsPerPage } = this.options
          let items = response
          const totalItems = response.length

          if (sortBy.length === 1 && sortDesc.length === 1) {
            items = items.sort((a, b) => {
              const sortA = a[sortBy[0]]
              const sortB = b[sortBy[0]]

              if (sortDesc[0]) {
                if (sortA < sortB) return 1
                if (sortA > sortB) return -1
                return 0
              } else {
                if (sortA < sortB) return -1
                if (sortA > sortB) return 1
                return 0
              }
            })
          }

          if (itemsPerPage > 0) {
            items = items.slice((page - 1) * itemsPerPage, page * itemsPerPage)
          }

          setTimeout(() => {
            resolve({
              items,
              totalItems,
            })
          }, 2000)
        })
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