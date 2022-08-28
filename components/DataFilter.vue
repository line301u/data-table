<template v-slot:body.append>
  <v-container class="d-flex">
    <v-col>
      <template>
        <v-text-field
          v-model="selectedFilterValues.search"
          append-icon="mdi-magnify"
          label="Search"
          single-line
          hide-details
          @keyup="setFilter(selectedFilterValues.search, 'search')"
        ></v-text-field>
      </template>
    </v-col>
    <v-col>
      <v-select
        multiple
        label="Filter by gender"
        :items="gender_list"
        v-model="selectedFilterValues.gender"
        @change="setFilter(selectedFilterValues.gender, 'gender')"
        >
      </v-select>

      <div>
          <span v-if="selectedFilterValues.gender.length > 0"> {{selectedFilterValues.gender[0]}}</span>
          <span v-if="selectedFilterValues.gender.length > 1">{{(selectedFilterValues.gender.length)-1}}</span>
      </div>
    </v-col>
    
    <v-col>
      <v-select
        multiple
        label="Filter by country"
        :items="country_list"
        v-model="selectedFilterValues.country"
        @change="setFilter(selectedFilterValues.country, 'country')"
        >
      </v-select>
      <div>
          <span v-if="selectedFilterValues.country.length > 0"> {{selectedFilterValues.country[0]}}</span>
          <span v-if="selectedFilterValues.country.length > 1">{{(selectedFilterValues.country.length)-1}}</span>
      </div>
    </v-col>

    <button @click="clearFilter(); clearFilterValues();">Clear filter</button>
  </v-container>
</template>


<script>
import {gender_list, country_list} from '../global/filterData.js'

export default {
  props: ['setFilter', 'clearFilter'],
  data() {
    return {
      selectedFilterValues:{
        gender: [],
        country: [],
        search: ""
      },
      gender_list,
      country_list
    }
  },
  methods: {
    clearFilterValues(){
      this.selectedFilterValues.gender = "";
      this.selectedFilterValues.country = "";
      this.selectedFilterValues.search = "";
    }
  }
}
</script>

<style lang="sass" scoped>
  .v-data-table
    max-width: 100%
</style>
