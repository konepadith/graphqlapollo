<template lang="">
  <v-card class="mx-auto ma-4" width="300">
    <v-list>
      <v-list-group :value="true" prepend-icon="mdi-file">
        <template v-slot:activator>
          <v-list-item-title>Files</v-list-item-title>
        </template>
        
        <v-list-group :value="true" no-action sub-group>
          <template v-slot:activator>
            <v-list-item-content>Books</v-list-item-content>
          </template>

          <v-list-item v-for="(book, i) in books" :key="i" link>
            <v-list-item-title v-text="book"></v-list-item-title>
            <v-list-item-icon><v-icon>mdi-book</v-icon></v-list-item-icon>
          </v-list-item>
        </v-list-group>

        <v-list-group no-action sub-group>
          <template v-slot:activator>
            <v-list-item-content>Code</v-list-item-content>
          </template>

          <v-list-item v-for="(code, i) in codes" :key="i" link>
            <v-list-item-title v-text="code"></v-list-item-title>
            <v-list-item-icon><v-icon>mdi-xml</v-icon></v-list-item-icon>
          </v-list-item>
        </v-list-group>
      </v-list-group>
      <v-list-item :to="codes[1]">
        <v-list-item-icon><v-icon>mdi-cog</v-icon></v-list-item-icon>
        <v-list-item-title>Settings</v-list-item-title>
      </v-list-item>
    </v-list>
  </v-card>
</template>
<script>
export default {
  data() {
    return {
      selectedItem: 1,
      books: ["History", "Fiction", "Philosophy"],
      codes: ["C#", "JavaScript", "PHP"],
    };
  },
};
</script>
<style lang=""></style>
