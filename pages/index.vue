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
          item-key="email"
          dense
          :options.sync="options"
          :server-items-length="totalItems"
          :loading="loading"
          :footer-props="{'items-per-page-options':[5, 10, 15, 30, 50]}"
        ).elevation-1.mt-2

</template>

<script>
import DataFilter from '~/components/DataFilter.vue'
import sales from '~/api/sales.js'
import {gender_list, country_list} from '../global/filterData.js'

export default {
  components: {
    DataFilter
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
        { text: 'Gender', value: 'gender'},
        { text: 'Year', value: 'year' },
        { text: 'Sales', value: 'sales' },
        { text: 'Country', value: 'country'},
      ],
    }
  },
    created() {
    this.getDataFromApi();
  },
  watch: {
      options: {
        handler() {
          this.getDataFromApi()
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

          // SORT COLOUMNS
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

          // FILTER BY GENDER
          if (this.filterSettings.gender.length){
            const selectedValues = this.filterSettings.gender
            const filterArg = item => selectedValues.includes(item.gender)

            filterData(filterArg)
          }

          // FILTER BY COUNTRY
          if (this.filterSettings.country.length){
            const selectedValues = this.filterSettings.country
            const filterArg = item => selectedValues.includes(item.country)

            filterData(filterArg)
          }

          // GET SEARCH INPUT FILTERING
          if (this.filterSettings.search){
            let searchInput = this.filterSettings.search
            searchInput = searchInput.toLowerCase();
            const filterArg = item => item.user.first_name.toLowerCase().includes(searchInput) || item.user.last_name.toLowerCase().includes(searchInput) || item.email.toLowerCase().includes(searchInput)

            filterData(filterArg)
          }

          // FILTER DATA
          function filterData(filterArg){
            const filteredItems = items.filter(filterArg)
            items = filteredItems
          }

          // DEFINE ITEMS PER PAGE
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
      if (filterType === "gender"){
        this.filterSettings.gender = selectedValues;
        
        this.getDataFromApi();
      }

      if (filterType === "country"){
        this.filterSettings.country = selectedValues;

        this.getDataFromApi();
      }

      if (filterType === "search"){
        this.filterSettings.search = selectedValues;

        this.getDataFromApi();
      }
    },
    clearFilter(){
      this.filterSettings.gender = [];
      this.filterSettings.country = [];
      this.filterSettings.search = "";

      this.getDataFromApi();
    },
  }
}
</script>

<style lang="sass">
  @import "../assets/sass/_variables.sass"

  .v-main__wrap
    margin-bottom: 5rem

  .v-application .elevation-1
    box-shadow: none !important
    box-shadow: 2.4px 0px 3.6px rgba(0, 0, 0, 0.009), 6.5px 0px 10px rgba(0, 0, 0, 0.05), 15.7px 0px 24.1px rgba(0, 0, 0, 0.013), 52px 0px 80px rgba(0, 0, 0, 0.04) !important
    border: 1px solid $rs__grey-1

  .v-data-table-header
    tr
      background: $rs__grey-2
      height: 32px !important

    span
      color: $rs__grey-6
      text-transform: uppercase
    
    th
      padding-top: 2px !important

    .v-icon
      font-size: 1rem !important
      margin-left: .2rem

      &::before
        margin-bottom: 3px
        color: $rs__primary-7   

  .theme--light.v-data-table > .v-data-table__wrapper > table > tbody > tr:not(:last-child) > td:not(.v-data-table__mobile-row)
    border-bottom: 1px solid $rs__grey-2

  .theme--light.v-data-table > .v-data-table__wrapper > table > thead > tr:last-child > th
    border-bottom: 1px solid $rs__grey-2

  .v-text-field.v-input--is-focused > .v-input__control > .v-input__slot:after
    transform: scale(0)

  tbody
    tr
      &:nth-child(2n)
        background: $rs__grey-1
        transition: .3s
      
      &:hover 
        background-color: $rs__grey-2 !important
        transition: .3s
        
    td
      height: 42px !important


  .v-progress-linear__buffer
    background-color: $rs__grey-3 !important

  .v-list-item
    &__title
      font-size: 0.895rem
      padding: 0.2rem 0
    
    &__action
      i.primary--text
        color: $rs__primary-8 !important 
    
    &--active 
      background-color: white !important
      
      .v-list-item__title 
        color: black !important

  .v-data-table-header
    .v-select-list
      width: 237px

    .v-menu
      &::after
        display: none

    &__content:first-child
      top: 170px !important

  .v-data-footer
    border-top: 1px solid $rs__grey-2 !important
    padding: .5rem .3rem
    justify-content: center

    @media screen and (min-width: 775px) 
      justify-content: end

    .v-input
      &__control
        background: $rs__grey-1 !important
        border-radius: 0.7rem
        padding: 0 .7rem
      
      &__slot::before
        display: none

    .v-select
      margin: 13px 0 13px 15px !important

  .v-data-table-header-mobile__wrapper
    .v-label
      color: $rs__grey-6
      text-transform: uppercase
      font-size: .825rem

    .v-select__slot
      .primary--text
        color: $rs__grey-6 !important

    .v-input__slot::before
      display: none
    
    .v-chip
      background: white !important
      margin: 4px 0 0 0!important

      &__content
        text-transform: capitalize
        font-weight: 400

</style>