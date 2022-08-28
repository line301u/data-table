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
        <template v-slot:selection="{ item, index }">
          <v-chip v-if="index === 0">
            <span>{{ item }}</span>
          </v-chip>
          <span
            v-if="index === 1"
            class="grey--text text-caption"
          >
            (+{{ selectedFilterValues.gender.length - 1 }} others)
          </span>
      </template>
      </v-select>
    </v-col>
    
    <v-col>
      <v-select
        multiple
        label="Filter by country"
        :items="country_list"
        v-model="selectedFilterValues.country"
        @change="setFilter(selectedFilterValues.country, 'country')"
        >
        <template v-slot:selection="{ item, index }">
          <v-chip v-if="index === 0">
            <span>{{ item }}</span>
          </v-chip>
          <span
            v-if="index === 1"
            class="grey--text text-caption"
          >
            (+{{ selectedFilterValues.country.length - 1 }} others)
          </span>
        </template>
      </v-select>
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

<style lang="sass">
  .v-select
    &__selections
      min-height: 40px

  .v-input
    &__control
      min-height: 40px

</style>
