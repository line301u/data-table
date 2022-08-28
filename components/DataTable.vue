<template lang="pug">
  v-data-table(
    :headers="headers"
    :items="items"
    :search="search"
    item-key="email"
    dense
    :options.sync="options"
    :server-items-length="totalItems"
    :loading="loading"
  ).elevation-1.mt-10

</template>


<script>
export default {
  props: ['headers', 'items', 'search', 'options', 'totalItems', 'loading'],
  data() {
    return {
      newOptions: options
    }
  },
    onMount:{
    run(){
      console.log("hej")
    }
  },
  methods:{
    getOptions(){
      console.log(this.newOptions)
    }
  }
}
</script>

<style lang="sass" scoped>
  .v-data-table
    max-width: 100%
</style>
