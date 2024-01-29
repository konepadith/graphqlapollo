<template>
  <v-container>
    <v-row>
      <v-col cols="12" lg="6">
        <v-menu
          ref="menu1"
          v-model="menu1"
          :close-on-content-click="false"
          transition="scale-transition"
          offset-y
          max-width="290px"
          min-width="auto"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-text-field
              v-model="dateFormatted"
              label="Date"
              hint="DD/MM/YYYY format"
              persistent-hint
              prepend-icon="mdi-calendar"
              :rules="dateSelectRules"
              v-bind="attrs"
              @blur="date = parseDate(dateFormatted)"
              v-on="on"
            ></v-text-field>
          </template>
          <v-date-picker
            v-model="date"
            no-title
            @input="menu1 = false"
          ></v-date-picker>
        </v-menu>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
export default {
  props: {
    para: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      date: null,
      dateFormatted: null,
      menu1: false,
      dateSelectRules: [(v) => !!v || "Date is required"],
    };
  },
  watch: {
    date(val) {
      this.dateFormatted = this.formatDate(this.date);
      console.log("component", this.date, this.para);
      this.$emit("setdate", this.date, this.para);
    },
  },
  methods: {
    formatDate(date) {
      if (!date) return null;
      const [year, month, day] = date.split("-");
      return `${day}/${month}/${year}`;
    },
    parseDate(date) {
      if (!date) return null;
      const [day, month, year] = date.split("/");
      return `${year}-${month.padStart(2, "0")}-${day.padStart(2, "0")}`;
    },
  },
};
</script>