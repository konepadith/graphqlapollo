<template>
  <v-app>
    <v-container>
      <v-autocomplete
        v-for="(autocomplete, index) in autocompletes"
        :key="'autocomplete-' + index"
        v-model="autocomplete.selectedItem"
        :items="autocomplete.items"
        :search-input.sync="search"
        label="Search for an item"
        @search="searchItems(autocomplete, $event)"
      >
      </v-autocomplete>
    </v-container>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      search:null,
      autocompletes: [
        {
          selectedItem: null,
          items: [
            'Apple',
            'Banana',
            'Cherry',
            'Date',
            'Elderberry',
            'Fig',
            'Grape',
            'Honeydew'
          ],
          filteredItems: []
        },
        {
          selectedItem: null,
          items: [
            'Red',
            'Green',
            'Blue',
            'Yellow',
            'Orange',
            'Purple'
          ],
          filteredItems: []
        }
      ]
    };
  },
  methods: {
    searchItems(autocomplete, searchTerm) {
      console.log("autocomplete",autocomplete)
      console.log("searchTerm",searchTerm)
      autocomplete.filteredItems = autocomplete.items.filter(item =>
        item.toLowerCase().includes(searchTerm.toLowerCase())
      );
    }
  }
};
</script>

<style>
/* Add any custom styles here */
</style>
