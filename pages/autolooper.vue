<template>
  <v-app>
    <v-container>
      <div v-for="(item, index) in 2" :key="index">
        <v-autocomplete
          :key="'autocomplete-' + index"
          v-model="selectedItems[index]"
          :items="items"
          :search-input.sync="searchInputs[index]"
          label="Search for a customer"
          @change="indexes = index"
        ></v-autocomplete>
        <v-btn color="success" @click="fillData(index)" :key="'btn-' + index">Search</v-btn>
        <v-text-field v-model="selectedCustomers[index]" label="Selected Customer" disabled></v-text-field>
      </div>
    </v-container>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      indexes:null,
      selectedCustomers: [null, null], // Stores selected customers
      selectedItems: [null, null], // Initialize with two null values
      items: ["John", "Dow", "Anna", "Robert", "Mint", "Coe"],
      searchInputs: ['', ''] // Initialize search input values
    };
  },
  watch: {
    searchInputs: {
      handler(newValue, oldValue) {
        console.log(newValue)
        console.log(newValue[this.indexes])
      // If index is found, handle the change
      }
    }
  },
  methods: {
    fillData(index) {
      // Update selected customer text field for the specific autocomplete
      this.$set(this.selectedCustomers, index, this.selectedItems[index]);
    }
  }
};
</script>

<style>
/* Add any custom styles here */
</style>
