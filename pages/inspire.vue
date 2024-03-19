<template>
  <div>
    <v-form @submit.prevent="submitForm" ref="form">
      <v-container>
        <v-row>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="formData.firstname"
              :rules="nameRules"
              :counter="10"
              label="First name"
              required
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="formData.lastname"
              :rules="lastnameRules"
              :counter="10"
              label="Last name"
              required
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="formData.email"
              :rules="emailRules"
              label="Email"
              required
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12" md="4">
            <v-select
              v-model="formData.select"
              :items="items"
              :rules="selectRules"
              label="item"
              required
            ></v-select>
          </v-col>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="formData.phone"
              :rules="phoneRules"
              label="Phone"
              required
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="4">
            <v-file-input
              v-model="formData.file"
              chips
              :rules="fileRules"
              label="Choose a file"
              accept=".pdf, .doc, .docx"
            ></v-file-input>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="12" md="4">
            <v-file-input
              v-model="formData.multifile"
              :rules="multifileRules"
              accept=".pdf, .doc, .docx"
              ref="fileUpload"
              chips
              multiple
              label="File input w/ chips and Multi file"
              style="visibility: hidden;"
            ></v-file-input>
            <v-menu
              ref="menu1"
              v-model="menu1"
              :close-on-content-click="false"
              transition="scale-transition"
              offset-y
              min-width="auto"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  v-model="dateFormatted"
                  label="Date"
                  hint="MM/DD/YYYY format"
                  prepend-icon="mdi-calendar"
                  :rules="dateSelectRules"
                  readonly
                  @blur="formData.selectedDate = parseDate(dateFormatted)"
                  v-bind="attrs"
                  v-on="on"
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="formData.selectedDate"
                no-title
                scrollable
                @input="menu1 = false"
              >
                <v-spacer></v-spacer>
              </v-date-picker>
            </v-menu>
          </v-col>
          <dateSelect :para="'componentDate'" @setdate="setDate_componentDate"></dateSelect>
          <dateSelect :para="'componentDate2'" @setdate="setDate_componentDate"></dateSelect>
        </v-row>
        <v-row>
          <v-col cols="12">
            <v-btn type="submit">Submit</v-btn>
            <v-btn color="error" class="ms-2" @click="reset">
              Reset Form
            </v-btn>
            <v-btn color="warning" class="ms-2" @click="uploadfilds">
              Reset Validation
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
  </div>
</template>

<script>
import dateSelect from '../components/utility/dateSelect.vue';
export default {
  name: "InspirePage",
  components:{
    dateSelect
  },
  data() {
    return {
      menu1: false,
      dateFormatted: null,
      formData: {
        firstname: "",
        lastname: "",
        email: "",
        phone: "",
        select: null,
        file: null,
        multifile: [],
        selectedDate: null,
        componentDate:null,
        componentDate2:null
      },
      items: ["Item 1", "Item 2", "Item 3", "Item 4"],
      nameRules: [
        (v) => !!v || "Name is required",
        (v) => /[^0-9]/.test(v) || "Lastname can not contain digits.",
        (v) => (v && v.length <= 50) || "Name must be less than 50 characters",
      ],
      dateSelectRules: [(v) => !!v || "Date is required"],
      requiredRule: [(v) => !!v || "This field is required"],
      emailRules: [
        (v) => !!v || "Eamil is require",
        (v) => /.+@.+\..+/.test(v) || "Enter a valid email address",
      ],
      phoneRules: [
        (v) => !!v || "Phone number is required",
        (v) =>
          /^(\+\d{1,3})?\d{10}$/.test(v) || "Phone number can not contain text",
        (v) =>
          (v && v.length <= 50) || "Phone number must be less than 15 digit",
      ],
      selectRules: [(v) => !!v || "Item is required"],
      fileRules: [
        (file) => !!file || "File is require",
        (file) =>
          (file ? file.size <= 1024 * 1024 * 8 : true) ||
          "File size should be less than 8MB",
      ],
      multifileRules: [
        (files) => !!files.length || "Files are required",
        (files) =>
          files.every((file) => file.size <= 1024 * 1024 * 8) ||
          "Each file size should be less than 8MB",
      ],
    };
  },
  methods: {
    submitForm() {
      console.log("File Submitted:", this.formData);
      console.log("Valitdate", this.$refs.form.validate());
    },
    reset() {
      this.$refs.form.reset();
    },
    resetValidation() {
      this.$refs.form.resetValidation();
    },
    formatDate(date) {
      if (!date) return null;

      const [year, month, day] = date.split("-");
      return `${day}/${month}/${year}`;
    },
    parseDate(date) {
      if (!date) return null;

      const [ day,month, year] = date.split("/");
      return `${year}-${month.padStart(2, "0")}-${day.padStart(2, "0")}`;
    },
    setDate_componentDate(date,para){
      this.formData[para] = date
      console.log(this.formData)
    },
    uploadfilds(){
      this.$refs.fileUpload.$refs.input.click();
    }
  },
  computed: {
    lastnameRules() {
      // If firstname is filled, lastname is optional; otherwise, it is required
      return this.formData.firstname ? [] : [(v) => !!v || "Last name is required"];
    },
  },
  watch: {
    "formData.selectedDate"(val) {
      this.dateFormatted = this.formatDate(this.formData.selectedDate);
    },
  },
};
</script>