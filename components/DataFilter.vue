<template v-slot:body.append>
  <div class="filter-container">
      <div class="filter d-flex">
        <div class="filter__search">
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
        </div>
        <div class="filter__select d-flex">
          <v-select
            multiple
            label="Filter by gender"
            :items="gender_list"
            v-model="selectedFilterValues.gender"
            color="unset"
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
        
          <v-select
            multiple
            label="Filter by country"
            :items="country_list"
            v-model="selectedFilterValues.country"
            color="unset"
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
          <div class="button-wrapper">
            <button class="button" @click="clearFilter(); clearFilterValues();">Clear filter</button>
          </div>
        </div>
      </div>
  </div>
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
  },
}
</script>

<style lang="sass">
  @import "../assets/sass/_variables.sass"
  
  .filter-container
    margin-top: 1.5rem

    .filter
      flex-direction: column
      justify-content: flex-start

      @media screen and (min-width: 775px) 
        flex-direction: row
        justify-content: space-between
        margin-bottom: 2rem

      &__select
        flex-shrink: 1
        flex-direction: column

        @media screen and (min-width: 775px) 
          flex-direction: row
          justify-content: space-between
          justify-content: flex-start

      .v-select
        input
          cursor: pointer

      &__search
        flex-grow: 1
        width: 100%
        
        @media screen and (min-width: 775px) 
          max-width: 350px

        .v-input
          &__slot
            border: 1.5px solid $rs__grey-2
            border-radius: 0.3rem
          
          &__control
            @media screen and (min-width: 775px) 
              margin-right: 1rem
        
          .v-label
            top: 6px

          .primary--text
            color: $rs__primary-7 !important

        .v-text-field input
          color: black
          font-size: 0.825rem
          padding-left: .5rem

      .v-select
        &__selections
          min-height: 40px

        &__slot
          width: 100%
          background-color: $rs__grey-1
          border-radius: 0.3rem

          @media screen and (min-width: 775px) 
            margin-right: 1rem
            width: 220px

          &::before
            border-color: transparent !important
            border-style: none

      .v-chip
        background: $rs__grey-2 !important

    .v-text-field
      .v-label
        top: 10px
        font-size: 0.825rem
        color: $rs__grey-6 !important
        margin-left: .5rem
        font-weight: 500
      
      .v-label--active
        transform: translateY(-28px) scale(0.9)
        color: red
        margin-left: 0rem
        color: $rs__grey-6 !important

      .v-input__control > .v-input__slot:before
        border-color: transparent !important
        border-style: none

    .v-input
      &__control
        min-height: 40px
      
      &__slot
        margin-bottom: .4rem !important

      &__icon
        i
          color: $rs__grey-4 !important

      &__append-inner
        margin-top: 7px !important
        margin-right: 5px

    .button-wrapper
      display: flex
      margin-bottom: 3rem
      margin-top: 1rem
      justify-content: flex-end

      @media screen and (min-width: 775px) 
        margin-bottom: 0rem
        margin-top: 8px
        align-self: center
        align-items: center
        justify-content: flex-start


    .button 
      font-size: 0.825rem
      color: $rs__grey-6
      min-width: 100px
      background-color: $rs__grey-2
      padding: .2rem 1rem
      border-radius: 0.3rem
      max-height: 30px
      font-weight: 500
  
  .v-messages, .v-text-field__details
    display: none !important

  div.filter__select.d-flex > div.v-input.v-input--is-focused.theme--light.v-text-field.v-text-field--is-booted.v-select.v-select--is-menu-active.v-select--is-multi.primary--text > div > div.v-input__slot > div.v-select__slot > div.v-input__append-inner > div > i
    color: $rs__primary-8 !important

  div.filter__select.d-flex > div.v-input.v-input--is-focused.theme--light.v-text-field.v-text-field--is-booted.v-select.v-select--is-multi.primary--text > div > div.v-input__slot > div.v-select__slot > div.v-input__append-inner > div > i
    color: $rs__grey-4 !important


</style>
